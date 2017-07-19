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
"splitter",

"-search",
"search-pro", 

"-sharing",

"anchors",

"simple-page-toc",


"anchor-navigation-ex",

"sectionx",

"toggle-chapters",

"prism",
"-highlight",

"tbfed-pagefooter",

"github",

"-search",
"search-pro",

"anchor-navigation-ex",

"todo",

"github-buttons"

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

"prism": {
            "css": [
                "prism-themes/xonokai.css"
            ]
},

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

