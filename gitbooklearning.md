<!-- toc -->
# gitBook Editor 
##### gitbook Editor的bug太多了,这里给一些建议

Editor是gitbook出的编辑器,可以支持上传到gitHub和gitBook端,想要同步到gitHub就在gitHub新建依赖然后在gitBook上新建gitHub类型的书,gitHub上首页默认显示README.md内容.

1. 进入官网下载安装
2. book分为本地和云端,云端即是你在GitBook上的书,本地就是在本地新建的
3. 当你更新书的时候,gitBook上会updating,如果update失败,一般是编辑的内容有误,根据错误内容的行数你更改内容就可以再次上传成功

### 使用注意


1. 进行增删改最好在Editor中进行,不能再目录中进行,Editor会自动检测改变状况
2. 每次要增加删除文件记住要先save(保存到本地)和push(上传到gitHub和gitBook),不然会回倒,让人摸不着头脑.
3. 刚开始的文件名必须是英文(可以先新建英文文件再改成中文目录)
4. 使用时最好先制定好目录,比如我就把每个章节的图片,md等放在那个章节的文件夹里
5. 要使用多级目录可以编辑SUMMARY.md文件,详情参考我的SUMMARY.md文件

**重点**
* 可以使用book.json自定义书的风格,比如去掉分享按钮,更改搜索方式,目录可以收缩等等
* 安装插件 使用网上格式(具体网上很多),注意仔细检查内容
* 下面是我遇到的坑,很恐怖</br>
**book.json存放在最上层目录中也就是book的第一层**</br>
**当你需要重新安装或者删除插件时,你要先把你生成的本地化的书删除,才能重新安装**

**#一个超级巨大的坑: "这个符号一定要对....**
### 我的book.json
**要使用,删除注释**

```javascript
{
"title" : "siaoxiorliao的学习笔记",
"author" : "siaoxiorliao",
"description" : "学习笔记",
"output.name": "site",
"gitbook": "3.2.2",
"language": "zh-hans",
"root": ".",
"links": {
"sidebar": {
"Home": "https://www.gitbook.com/@siaoxiorliao1"
}
},

"plugins": [
"splitter",//侧边栏可以收缩

"-search",
"search-pro", //支持中文搜索

"-sharing",//禁用分享

"anchors",//标题上增加github图标

"simple-page-toc",//自动生成本页的目录结构:在需要生成目录的地方加上 <!-- toc -->


"anchor-navigation-ex",

"sectionx",//将页面分块显示:you should start with h3 if you use heading

"toggle-chapters",//使左侧的章节目录可以折叠

"prism",
"-highlight",//使用代码高亮,需要拷贝文件到插件目录,css我使用的在**下方**

"tbfed-pagefooter",//底部显示信息

"github",//右上角显示github图标

"-search",
"search-pro",

"anchor-navigation-ex",//添加Toc到侧边悬浮导航以及回到顶部按钮。
/*
            # h1
            ## h2
            ### h3
            必须要以 h1 开始递进，直接写 h2 不会被提取
*/

"todo",
/*
            添加todo功能
            - [ ] write some articles
            - [x] drink a cup of tea
*/

"github-buttons"//添加项目在 github 上的 star，watch，fork情况

],
"pluginsConfig": {

"search-pro": {
"cutWordLib": "nodejieba",
"defineWord" : ["Gitbook Use"]
},

"simple-page-toc": {
"maxDepth": 3,
"skipFirstH1": true
},


"anchor-navigation-ex": {
            "isRewritePageTitle": true,
            "isShowTocTitleIcon": true,
            "tocLevel1Icon": "fa fa-hand-o-right",
            "tocLevel2Icon": "fa fa-hand-o-right",
            "tocLevel3Icon": "fa fa-hand-o-right"
},


"sectionx": {
          "tag": "b"
},

/*
"prism": {
            "css": [
                "prism-themes/xonokai.css"
            ]
},
*/

"tbfed-pagefooter": {
"copyright":"Copyright &copy siaoxiorliao 2017",
"modify_label": "该文件修订时间：",
"modify_format": "YYYY-MM-DD HH:mm:ss"
}, 

"github": {
"url": "https://github.com/siaoxiorliao"
},

"github-buttons": {
            "repo": "siaoxiorliao/uifoundationnote",
            "types": [
                "star",
                "watch",
                "fork"
            ],
            "size": "small"
},

"search-pro": {
"cutWordLib": "nodejieba",
"defineWord" : ["Gitbook Use"]
},

"anchor-navigation-ex": {
            "isRewritePageTitle": true,
            "isShowTocTitleIcon": true,
            "tocLevel1Icon": "fa fa-hand-o-right",
            "tocLevel2Icon": "fa fa-hand-o-right",
            "tocLevel3Icon": "fa fa-hand-o-right"
}

  }
}
```

### website.css
加入
```javascript
h1 , h2{
    border-bottom: 1px solid #EFEAEA;
}
```


### xonokai.css
```javascript
/**
* xonokai theme for JavaScript, CSS and HTML
* based on: https://github.com/MoOx/sass-prism-theme-base by Maxime Thirouin ~ MoOx --> http://moox.fr/ , which is Loosely based on Monokai textmate theme by http://www.monokai.nl/
* license: MIT; http://moox.mit-license.org/
*/
code[class*="language-"],
pre[class*="language-"] {
    -moz-tab-size: 2;
    -o-tab-size: 2;
    tab-size: 2;
    -webkit-hyphens: none;
    -moz-hyphens: none;
    -ms-hyphens: none;
    hyphens: none;
    white-space: pre;
    white-space: pre-wrap;
    word-wrap: normal;
    font-family: Menlo, Monaco, "Courier New", monospace;
    font-size: 14px;
    color: #76d9e6;
    text-shadow: none;
}
pre[class*="language-"],
:not(pre)>code[class*="language-"] {
    background: #2a2a2a;
}
pre[class*="language-"] {
    padding: 15px;
    border-radius: 4px;
    border: 1px solid #e1e1e8;
    overflow: auto;
}

pre[class*="language-"] {
    position: relative;
}
pre[class*="language-"] code {
    white-space: pre;
    display: block;
}

:not(pre)>code[class*="language-"] {
    padding: 0.15em 0.2em 0.05em;
    border-radius: .3em;
    border: 0.13em solid #7a6652;
    box-shadow: 1px 1px 0.3em -0.1em #000 inset;
}
.token.namespace {
    opacity: .7;
}
.token.comment,
.token.prolog,
.token.doctype,
.token.cdata {
    color: #6f705e;
}
.token.operator,
.token.boolean,
.token.number {
    color: #a77afe;
}
.token.attr-name,
.token.string {
    color: #e6d06c;
}
.token.entity,
.token.url,
.language-css .token.string,
.style .token.string {
    color: #e6d06c;
}
.token.selector,
.token.inserted {
    color: #a6e22d;
}
.token.atrule,
.token.attr-value,
.token.keyword,
.token.important,
.token.deleted {
    color: #ef3b7d;
}
.token.regex,
.token.statement {
    color: #76d9e6;
}
.token.placeholder,
.token.variable {
    color: #fff;
}
.token.important,
.token.statement,
.token.bold {
    font-weight: bold;
}
.token.punctuation {
    color: #bebec5;
}
.token.entity {
    cursor: help;
}
.token.italic {
    font-style: italic;
}

code.language-markup {
    color: #f9f9f9;
}
code.language-markup .token.tag {
    color: #ef3b7d;
}
code.language-markup .token.attr-name {
    color: #a6e22d;
}
code.language-markup .token.attr-value {
    color: #e6d06c;
}
code.language-markup .token.style,
code.language-markup .token.script {
    color: #76d9e6;
}
code.language-markup .token.script .token.keyword {
    color: #76d9e6;
}

/* Line highlight plugin */
pre[class*="language-"][data-line] {
    position: relative;
    padding: 1em 0 1em 3em;
}
pre[data-line] .line-highlight {
    position: absolute;
    left: 0;
    right: 0;
    padding: 0;
    margin-top: 1em;
    background: rgba(255, 255, 255, 0.08);
    pointer-events: none;
    line-height: inherit;
    white-space: pre;
}
pre[data-line] .line-highlight:before,
pre[data-line] .line-highlight[data-end]:after {
    content: attr(data-start);
    position: absolute;
    top: .4em;
    left: .6em;
    min-width: 1em;
    padding: 0.2em 0.5em;
    background-color: rgba(255, 255, 255, 0.4);
    color: black;
    font: bold 65%/1 sans-serif;
    height: 1em;
    line-height: 1em;
    text-align: center;
    border-radius: 999px;
    text-shadow: none;
    box-shadow: 0 1px 1px rgba(255, 255, 255, 0.7);
}
pre[data-line] .line-highlight[data-end]:after {
    content: attr(data-end);
    top: auto;
    bottom: .4em;
}
```

