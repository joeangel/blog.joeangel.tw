# Setup Hexo config for `blog.orange.tw`

<!--ts-->
* [Setup Hexo config for blog.orange.tw](guides/setup-hexo-config-for-blog-orange-tw.md#setup-hexo-config-for-blogorangetw)
   * [Question](guides/setup-hexo-config-for-blog-orange-tw.md#question)
   * [Answer by ChatGPT](guides/setup-hexo-config-for-blog-orange-tw.md#answer-by-chatgpt)
      * [1. Title and Description](guides/setup-hexo-config-for-blog-orange-tw.md#1-title-and-description)
      * [2. Meta Tags and SEO](guides/setup-hexo-config-for-blog-orange-tw.md#2-meta-tags-and-seo)
      * [3. Favicon](guides/setup-hexo-config-for-blog-orange-tw.md#3-favicon)
      * [4. Custom CSS and JavaScript](guides/setup-hexo-config-for-blog-orange-tw.md#4-custom-css-and-javascript)
      * [5. Social Media Metadata (Open Graph, Twitter Cards)](guides/setup-hexo-config-for-blog-orange-tw.md#5-social-media-metadata-open-graph-twitter-cards)
      * [6. Analytics](guides/setup-hexo-config-for-blog-orange-tw.md#6-analytics)
      * [Example Configuration](guides/setup-hexo-config-for-blog-orange-tw.md#example-configuration)
      * [Final Steps](guides/setup-hexo-config-for-blog-orange-tw.md#final-steps)
<!--te-->

## Question

基於 blog.orange.tw 的 html head 部分，我如果要做得一樣， `_config.yml` 要怎麼設定

## Answer by ChatGPT

To set up your Hexo blog's `_config.yml` to have an HTML `<head>` section similar to `blog.orange.tw`, you will need to configure a few specific parameters. The HTML `<head>` typically includes meta tags, title, link tags for CSS, and scripts. Below is a guide on how you might configure these settings in `_config.yml`.

### 1. Title and Description
Set the title and description for your site, which will be reflected in the `<title>` and meta description tags.

```yaml
title: blog.orange.tw
subtitle: "Tech and Programming Insights"
description: "A blog about tech and programming."
author: Orange Team
language: en
timezone: "Asia/Taipei"
```

### 2. Meta Tags and SEO
Hexo allows you to insert custom meta tags and other head elements by modifying the `theme` or directly using the `_config.yml`.

**Option 1**: Through `_config.yml`
You can include custom meta tags or other head content using the `theme_config` section in `_config.yml`:

```yaml
theme_config:
  head:
    meta:
      - name: "viewport"
        content: "width=device-width, initial-scale=1"
      - name: "description"
        content: "A blog about tech and programming."
      - name: "keywords"
        content: "tech, programming, blog"
```

**Option 2**: Modify the Theme's `_layout Files`
If you need more control, edit the theme’s `layout/_partial/head.ejs` file to include specific meta tags, CSS links, or scripts:

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="description" content="<%= config.description %>">
  <meta name="keywords" content="tech, programming, blog">
  <link rel="stylesheet" href="path/to/custom.css">
  <script src="path/to/custom.js"></script>
</head>
```

### 3. Favicon
You can add a favicon by including it in the `_config.yml` under the `favicon` setting, or by directly placing the favicon link in your theme’s head section:

```yaml
favicon: /images/favicon.ico
```

### 4. Custom CSS and JavaScript
You can specify custom CSS and JS files to be included in the head section:

```yaml
theme_config:
  custom_css:
    - /css/custom.css
  custom_js:
    - /js/custom.js
```

### 5. Social Media Metadata (Open Graph, Twitter Cards)
For better social media integration, you can include Open Graph and Twitter Cards meta tags:

```yaml
theme_config:
  social:
    twitter_username: orange_blog
    facebook_app_id: your_facebook_app_id
    og_image: /images/og-image.jpg
    twitter_card:
      card: summary_large_image
      site: "@orange_blog"
```

### 6. Analytics
If `blog.orange.tw` uses Google Analytics or any other analytics service, you can add that in the `_config.yml`:

```yaml
google_analytics:
  tracking_id: UA-XXXXX-Y
  anonymize_ip: true
```

### Example Configuration
Here’s an example snippet combining all the above aspects:

```yaml
title: blog.orange.tw
subtitle: "Tech and Programming Insights"
description: "A blog about tech and programming."
author: Orange Team
language: en
timezone: "Asia/Taipei"
favicon: /images/favicon.ico

theme_config:
  head:
    meta:
      - name: "viewport"
        content: "width=device-width, initial-scale=1"
      - name: "description"
        content: "A blog about tech and programming."
      - name: "keywords"
        content: "tech, programming, blog"
  custom_css:
    - /css/custom.css
  custom_js:
    - /js/custom.js
  social:
    twitter_username: orange_blog
    facebook_app_id: your_facebook_app_id
    og_image: /images/og-image.jpg
    twitter_card:
      card: summary_large_image
      site: "@orange_blog"
google_analytics:
  tracking_id: UA-XXXXX-Y
  anonymize_ip: true
```

### Final Steps
After configuring your `_config.yml`, regenerate the static files and deploy your site:

```bash
hexo clean
hexo generate
hexo deploy
```

This will ensure that your site’s head section is correctly configured to match the desired setup.

