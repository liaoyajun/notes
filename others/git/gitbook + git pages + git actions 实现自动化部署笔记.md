# gitbook + git pages + git actions 实现自动化部署笔记

## gitbook 部分

- 切换 node 版本到 node/12.16.3 ，最新的不支持
    ```bash
    sudo n i 12.16.3
    ```

- 安装 gitbook-cli
    ```bash
    sudo npm i gitbook-cli -g
    ```

- 创建 gitbook
    ```bash
    gitbook init notes
    ```

- 初始化 gitbook
    - 修改目录文件 SUMMARY.md
        ```markdown
        # Summary

        * [简介](README.md)
        * [前端](front-end/README.md)
            * [基础](front-end/basic/README.md)
                * [html](front-end/basic/html/README.md)
                * [css](front-end/basic/css/README.md)
                * [js](front-end/basic/js/README.md)
        * [后端](back-end/README.md)
        * [其他](others/README.md)
            * [git](others/git/README.md)
                * [gitbook + git pages + git actions 实现自动化部署笔记](others/git/gitbook + git pages + git actions 实现自动化部署笔记.md)
        ```
    - 执行 gitbook init ，将根据 SUMMARY.md 自动创建 markdown 文件
        ```bash
        cd notes
        gitbook init
        ```

- 添加自定义样式
    ```css
    /*新建文件styles/customize.css*/
    /*隐藏 Published with GitBook*/
    .gitbook-link {
        display: none !important;
    }
    /*隐藏右上角分享*/
    .pull-right.js-toolbar-action {
        display: none !important;
    }
    /*隐藏上一页下一页*/
    .navigation {
        display: none !important;
    }
    ```

- 配置插件
    ```json
    // 新建文件book.json
    {
      "title": "notes",
      "links": {
        "sidebar": {
          "github": "https://github.com/liaoyajun"
        }
      },
      "styles": {
        "website": "styles/customize.css"
      },
      "plugins" : [
        "anchor-navigation-ex",  // 序号/悬浮目录/回到顶部
        "code",  // 复制代码按钮
        "editlink",  // 编辑按钮
        "expandable-chapters-small",  // 折叠侧边栏菜单
        "page-footer-ex",  // 定制每篇文章的页脚
        "prism",  // 代码高亮
        "-highlight",
        "search-pro",  // 支持中文内容搜索
        "-search",
        "-lunr",
        "splitter",  // 自由调节侧边栏的宽度
        "-sharing"
      ],
      "pluginsConfig": {
        "code": {
            "copyButtons": true
        },
        "editlink": {
          "base": "https://github.com/liaoyajun/notes/tree/main/",
          "label": "Edit This Page"
        },
        "page-footer-ex": {
          "copyright": "[liaoyajun](https://github.com/liaoyajun)",
          "markdown": true,
          "update_label": "<i>updated</i>",
          "update_format": "YYYY-MM-DD HH:mm:ss"
        },
        "prism": {
          "css": ["prismjs/themes/prism-okaidia.css"],
          "lang": {"flow": "typescript"}
        }
      },
      "ignores" : ["_book", "node_modules"]
    }
    ```
    ```bash
    # 安装依赖
    gitbook install
    ```

- 本地运行
    ```bash
    gitbook serve
    ```

- 打包
    ```bash
    gitbook build
    ```

## git actions 部分

- 创建 yml 文件
    ```yml
    # .github/workflows/gitbook.yml
    name: GitBook Deploy

    on:
      push:
        branches: [ "main" ]

    jobs:
      build:
        runs-on: ubuntu-latest

        steps:
          - name: Setup Node.js
            uses: actions/setup-node@v1
            with:
              node-version: '12.16.3'

          - name: Log env
            run: node -v

          - name: Checkout
            uses: actions/checkout@v3

          - name: Install GitBook CLI
            run: npm install gitbook-cli -g

          - name: Install GitBook Plugins
            run: gitbook install

          - name: Build
            run: gitbook build

          - name: Deploy
            uses: JamesIves/github-pages-deploy-action@releases/v4
            with:
              branch: gh-pages
              folder: _book
    ```

## git pages 部分

- 创建公开仓库，如 notes

- 设置 actions 权限，仓库 - Settings - Actions - General [地址](https://github.com/liaoyajun/notes/settings/actions)
    - `Fork pull request workflows from outside collaborators` 设置为 `Require approval for first-time contributors who are new to GitHub`
    - `Workflow permissions` 设置为 `Read and write permissions` ，并勾选 `Allow GitHub Actions to create and approve pull requests`

- 提交一次代码

- 设置 github pages，仓库 - Settings - Pages
    - `Source` 选择 `Deploy from a branch`
    - `Branch` 选择 `gh-pages`

- 保存之后，会执行 `pages build and deployment` Actions，执行完成之后，[即可访问](https://liaoyajun.github.io/notes/)
