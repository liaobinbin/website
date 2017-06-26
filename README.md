# 项目网站

WebAssembly 中文项目预览: [webassembly.org.cn](http://webassembly.org.cn)

## 依赖

- Ruby >= 2.0.0
- [Bundler](http://bundler.io/)

## 构架站点

克隆本项目和`design`子模块:

```
$ git clone https://github.com/WebAssembly/website
$ git submodule update --init --recursive
```

安装gem依赖:

```
$ bundle install
```

通过Jekyll生成或者本地服务器预览:

```
$ bundle exec jekyll build
$ bundle exec jekyll serve
```

> 在每次修改后你必须执行 `bundle exec jekyll build` 命令，并在你的提交中包含 `docs` 目录！

## 部署

这个站点使用Jekyll插件, 所以GitHub Pages不能自动的构建它. 为了部署它, 你需要手动检查构建的静态站点文件到 `docs` 目录.

The static site should be configured to build to the `docs` directory. The `docs` directory is a special directory from which GitHub pages can publish directly. The naming convention is unfortunate given the confusing overlap with the site's own `docs` pages (output to `docs/docs`) which are themselves generated from the design docs submodule located at `design`.

## What is the role of `_config.yml` and the custom Jekyll plugins?

> Note: the following plugins are all hacks to make the workflow of generating website docs from the `design` repo work without updating the sources in the design repo.

- `gem 'jekyll-optional-front-matter'` loaded directly in the `Gemfile` allows markdown files without YAML frontmatter to be consumed directly. This is included to allow `design` repo `.md` files to be used as pages without modifying their source to add frontmatter.
- `gem 'jemoji'` loaded directly in the `Gemfile` replaces GitHub-style emoji markdown (e.g. `:+1:`) with images for compat.
- The `defaults` section of `_config.yml` adds default values to the YAML frontmatter of documents from the `design` repo. In particular, it specifies that all `.md` files in the design submodule should be labelled as type `doc` and given layout `doc.html`. It also manually moves a few docs into the `community` tree where they fit the site organization better.
- `auto_titles.rb` adds a `title` value to YAML frontmatter by looking for the first header tag in the source files. It also orders the design docs based on a hardcoded list.
- `link_converter.rb` turns the `design` repo's links (e.g. `[threads](FutureFeatures.md#threads)`) into their respective locations on this website (e.g. `[threads](/docs/future-features/#threads)`).
- `underscore_paths.rb` rewrites Jekyll page permalinks to convert `/design/FutureFeatures/` to `/docs/future-features/`.
