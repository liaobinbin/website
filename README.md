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

应该将静态站点配置为生成到`docs`目录. 这个`docs` 目录是Github Pages可以直接进行部署的特殊目录. 不幸的是由于本站的 `docs` 页面（输出到`docs/docs`）造成混淆, 所以这些 `docs` 页面本身从设计文档的时候就将其放置在 `design` 子模块中生成.

## `_config.yml` 和自定义的Jekyll插件的作用?

> Note: 以下插件都是不需要更改 `design` 仓库中的源文件而引入的可供生成网站文档的工作流模块.

- `gem 'jekyll-optional-front-matter'` 通过 `Gemfile` 文件直接加载，可以直接使用没有YAML头的markdown文件. 这包括允许 `design` 仓库里的 `.md` 文件用作页面的时候而不需要添加YAML头.
- `gem 'jemoji'` 通过 `Gemfile` 文件直接加载来替换 markdown 里面的 GitHub emoji 样式 (e.g. `:+1:`) 并通过图片的方式来达到兼容.
- The `defaults` section of `_config.yml` adds default values to the YAML frontmatter of documents from the `design` repo. In particular, it specifies that all `.md` files in the design submodule should be labelled as type `doc` and given layout `doc.html`. It also manually moves a few docs into the `community` tree where they fit the site organization better.
- `auto_titles.rb` adds a `title` value to YAML frontmatter by looking for the first header tag in the source files. It also orders the design docs based on a hardcoded list.
- `link_converter.rb` turns the `design` repo's links (e.g. `[threads](FutureFeatures.md#threads)`) into their respective locations on this website (e.g. `[threads](/docs/future-features/#threads)`).
- `underscore_paths.rb` rewrites Jekyll page permalinks to convert `/design/FutureFeatures/` to `/docs/future-features/`.
