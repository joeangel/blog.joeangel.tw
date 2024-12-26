---
title: VPN 問題排查與解決指南
date: 2024-12-26 15:54:16
tags:
---

## VPN 問題排查與解決指南

### **目錄**
- [OpenVPN 基礎知識](#openvpn-基礎知識)
- [常見問題與解決方法](#常見問題與解決方法)
- [排查步驟與工具](#排查步驟與工具)
- [伺服器配置（server.conf）](#伺服器配置serverconf)
- [客戶端配置（client.ovpn）](#客戶端配置clientovpn)
- [QA 自我檢修清單](#qa-自我檢修清單)

---

### **OpenVPN 基礎知識**

1. **VPN 隧道與網段**：
   - 伺服器內部地址：`10.8.0.1`。
   - 客戶端分配地址：`10.8.0.6`。
   - 點對點對端地址（伺服器端）：`10.8.0.5`，用於路由通信。

2. **NAT（網絡地址轉換）**：
   - 伺服器執行 NAT，將 VPN 子網（`10.8.0.0/24`）的流量轉換為伺服器外部網卡的流量。

3. **IP 轉發**：
   - 伺服器需要啟用 IP 轉發，允許 VPN 子網的流量轉發到外部網絡。

4. **路由與 DNS**：
   - 默認路由設置：客戶端默認流量應指向 VPN 的對端地址（如 `10.8.0.5`）。
   - DNS 配置：伺服器推送 DNS，確保客戶端可以解析域名。

---

### **常見問題與解決方法**

#### **1. VPN 隧道正常，但無法訪問外網**
- **原因**：伺服器未正確配置 NAT 或 IP 轉發。
- **解決方法**：
  1. 確認伺服器啟用了 IP 轉發：
     ```bash
     cat /proc/sys/net/ipv4/ip_forward
     ```
     如果為 `0`，請啟用：
     ```bash
     sudo sysctl -w net.ipv4.ip_forward=1
     ```
  2. 添加或檢查 NAT 規則：
     ```bash
     sudo iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
     ```

---

#### **2. 客戶端無法解析域名**
- **原因**：伺服器未正確推送 DNS。
- **解決方法**：
  1. 檢查伺服器是否推送 DNS：
     ```plaintext
     push "dhcp-option DNS 8.8.8.8"
     push "dhcp-option DNS 8.8.4.4"
     ```
  2. 在 macOS 客戶端手動設置 DNS：
     ```bash
     sudo networksetup -setdnsservers Wi-Fi 8.8.8.8 8.8.4.4
     ```

---

#### **3. 默認路由未正確設置**
- **原因**：客戶端配置未設置 `redirect-gateway`，或伺服器未正確推送路由。
- **解決方法**：
  1. 確認伺服器配置中包含：
     ```plaintext
     push "redirect-gateway def1 bypass-dhcp"
     ```
  2. 手動在 macOS 客戶端設置默認路由：
     ```bash
     sudo route add default 10.8.0.5
     ```

---

### **排查步驟與工具**

#### **伺服器端（Fedora）**

1. **檢查 NAT 配置**：
   ```bash
   sudo iptables -t nat -L POSTROUTING -v -n
   ```

2. **啟用 IP 轉發**：
   ```bash
   sudo sysctl -w net.ipv4.ip_forward=1
   ```

3. **查看伺服器的網卡名稱**：
   ```bash
   ip addr
   ```

4. **捕獲流量**：
   ```bash
   sudo tcpdump -i tun0 icmp
   ```

5. **查看 OpenVPN 日誌**：
   ```bash
   sudo journalctl -u openvpn-server@server
   ```

---

#### **客戶端（macOS）**

1. **檢查路由表**：
   ```bash
   route -n get default
   ```

2. **手動設置默認路由**：
   ```bash
   sudo route add default 10.8.0.5
   ```

3. **檢查網絡接口**：
   ```bash
   ifconfig
   ```

4. **測試內外網連通性**：
   ```bash
   ping 10.8.0.1
   ping 8.8.8.8
   ping google.com
   ```

5. **設置 DNS（臨時）**：
   ```bash
   sudo networksetup -setdnsservers Wi-Fi 8.8.8.8 8.8.4.4
   ```

---

### **伺服器配置（server.conf）**

```plaintext
port 1194
proto udp
dev tun
server 10.8.0.0 255.255.255.0
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 8.8.4.4"
persist-key
persist-tun
keepalive 10 120
cipher AES-256-GCM
user nobody
group nobody
status openvpn-status.log
verb 3
```

---

### **客戶端配置（client.ovpn）**

```plaintext
client
dev tun
proto udp
remote <server-ip> 1194
nobind
resolv-retry infinite
persist-key
persist-tun
remote-cert-tls server
cipher AES-256-GCM
verb 3
```

---

### **QA 自我檢修清單**

1. **VPN 隧道是否正常？**
   - 伺服器和客戶端是否可以互相 `ping`？

2. **伺服器是否啟用了 IP 轉發？**
   - `cat /proc/sys/net/ipv4/ip_forward` 是否為 `1`？

3. **伺服器 NAT 是否正確？**
   - `iptables -t nat -L POSTROUTING` 是否有 `MASQUERADE` 規則？

4. **客戶端默認路由是否正確？**
   - `route -n get default` 是否指向 `10.8.0.5` 或其他對端地址？

5. **DNS 是否正常工作？**
   - 是否可以解析域名（`ping google.com`）？

6. **防火牆是否阻止了流量？**
   - `iptables -L -v -n` 是否允許 `FORWARD` 和 `INPUT` 流量？

---

#### **注意事項**
- 確保伺服器的外部網卡名稱正確，否則 NAT 可能無法正常工作。
- 排查時，逐步測試內部網絡（VPN 隧道）和外部網絡（Internet）連通性，確認每一步的設置是否正確。
