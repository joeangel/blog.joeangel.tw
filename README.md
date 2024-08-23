# JoeAngel Blogger


## Table of Contents

<!--ts-->
* [JoeAngel Blogger](#joeangel-blogger)
   * [Table of Contents](#table-of-contents)
      * [How to auto generate Table of Contents](#how-to-auto-generate-table-of-contents)
      * [Clean temp files](#clean-temp-files)
   * [Get Stareted](#get-stareted)
<!--te-->

### How to auto generate `Table of Contents`

Generate with backup mode

```shell
gh-md-toc --insert --hide-footer README.md
```

Generate without backup mode

```shell
gh-md-toc --insert --no-backup --hide-footer README.md
```

ref: https://github.com/ekalinin/github-markdown-toc

### Clean temp files

```shell
rm -f ./README.md.orig.* ./README.md.toc.*
```

## Get Stareted

This project is build from [Hexo](https://hexo.io/)

