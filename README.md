# JoeAngel Blogger

## Table of Contents

<!--ts-->
* [JoeAngel Blogger](README.md#joeangel-blogger)
   * [Table of Contents](README.md#table-of-contents)
      * [How to auto generate Table of Contents](README.md#how-to-auto-generate-table-of-contents)
   * [Get Started](README.md#get-started)
      * [Commands](README.md#commands)
      * [Setup Guide](README.md#setup-guide)
   * [Hints](README.md#hints)
      * [Navigating to Sections in Vim](README.md#navigating-to-sections-in-vim)
      * [Validate YAML file](README.md#validate-yaml-file)
   * [Common issues](README.md#common-issues)
      * [Set up GitHub Pages with GitHub Actions](README.md#set-up-github-pages-with-github-actions)
   * [References](README.md#references)
   * [TODOs](README.md#todos)
<!--te-->

### How to auto generate `Table of Contents`

Commands

```shell
npm run doc-gen
npm run doc-clean
```

Generate with backup mode

```shell
gh-md-toc --insert --hide-footer README.md guides/*.md
```

Generate without backup mode

```shell
gh-md-toc --insert --no-backup --hide-footer README.md guides/*.md
```

Clean temp files

```shell
rm -f ./*.md.orig.* ./*.md.toc.*
```

ref: https://github.com/ekalinin/github-markdown-toc

## Get Started

This project is built using [Hexo](https://hexo.io/), a fast, simple, and powerful static site generator. Follow the commands below to manage and deploy your Hexo site.

### Commands

- `npm run clean`: Cleans the public directory and removes generated files. Use this command to ensure that old files are removed before generating new ones.
- `npm run build`: Generates static files in the `public` directory. This command compiles your site’s content, applying the theme and configuration settings.
- `npm run server`: Starts a local development server at `http://localhost:4000`. This is useful for previewing your site as you make changes.
- `npm run deploy`: Deploys the generated static files to your configured deployment destination, such as GitHub Pages.

To get started, clone this repository, install the dependencies, and then use the above commands to manage your site.

### Setup Guide

For detailed instructions on how to configure your Hexo project similar to `blog.orange.tw`, please refer to the [Setup Guide](./guides/setup-hexo-config-for-blog-orange-tw.md).

## Hints

### Navigating to Sections in Vim

If you're working in Vim and want to quickly jump to a specific section in a markdown file, check out our detailed [Navigating to Sections in Vim Guide](./guides/navigate-to-section-in-vim.md).

### Validate YAML file

Requirement

```shell
npm install -g yaml-lint
```

Validate YAML file

```shell
npx yaml-lint .github/workflows/deploy.yml
```

## Common issues

### Set up GitHub Pages with GitHub Actions

- [GitHub Pages - Hexo](https://hexo.io/docs/github-pages)

## References

- Hexo
  - [Hexo](https://hexo.io)
  - [Hexo - Github](https://github.com/hexojs/hexo)
  - [hexo GitHub Actions Deploy - blog.yangjerry.tw](https://blog.yangjerry.tw/2022/04/19/hexo-github-actions-deploy/)
- Orange blog
  - [blog.orange.tw](https://blog.orange.tw/)
  - [blog.orange.tw - Github](https://github.com/orangetw/blog.orange.tw)

## TODOs

- use `hexo-generator-sitemap`

