﻿# netnrmd

> Markdown Combinatorial editor | 组合编辑器

> markdown语法解析基于remarkable，编辑与解析分离

> 调用任意markdown解析器都能完美的运行

> <https://md.netnr.com>

## Install 安装

```
//font-awesome （可以修改样式，用图片代替）
<link href="https://lib.baomitu.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet">

//jquery
<script src="https://lib.baomitu.com/jquery/1.12.4/jquery.min.js"></script>

//remarkable 默认解析器（自定义render方法，可不引入）
<script src="https://lib.baomitu.com/remarkable/1.7.1/remarkable.js"></script>

//netnrmd 样式
<link href="netnrmd.css" rel="stylesheet" />
//netnrmd
<script src="netnrmd.min.js"></script>

//highlight 代码高亮（可选）
<link href="https://lib.baomitu.com/highlight.js/9.12.0/styles/github.min.css" rel="stylesheet">
<script src="https://lib.baomitu.com/highlight.js/9.12.0/highlight.min.js"></script>
```


## Usage 使用

```js
var nmd = new netnrmd('#txt');
console.log(nmd);
console.log($('#txt').data('netnrmd'));

//nmd.obj	参数
//nmd.md	默认remarkable解析器对象
```

## Documentation 文档

> [remarkable demo](https://jonschlinkert.github.io/remarkable/demo/)

### Options 选项

```js
var nmd = new netnrmd('#txt', {
    fullscreen: false,		//全屏
    splitscreen: true,		//分屏
    height: 300,			//高度
    defer: 300,				//延迟解析（毫秒）
    prefixicon: 'fa fa-',	//矢量图标前缀，font-awesome
    prefixkey: 'Ctrl+',		//按键支持

    //解析器，默认使用remarkable，需要引入remarkable
    //指定解析方式，返回解析后的HTML
    render: function (md) {
        console.log(md)
        return md;
    },

    //自定义工具栏
    items: [
        {
            title: '表情',		//title
            icon: 'smile-o',	//icon
            key: 'E',			//keyboard
            cmd: 'emoji'		//cmd (default icon) 
        },
        {
            title: '粗体',
            icon: 'bold',
            key: 'B'
        }
    ],

    //Before rendering the callback, add an expression Icon
    //渲染前回调，添加一个表情图标
    viewbefore: function () {
		console.log(this);

        this.items.splice(0, 0, {
            title: '表情/emoji',
            icon: 'smile-o',
            key: 'E',
            cmd: 'emoji'
        });
    },

    //Markdown editor changes when callback, custom parsing, add: emoji: parsing
    //编辑器变动时回调，自定义解析，添加 :emoji: 解析
    input: function () {
        //markdown to html
		//获取markdown解析为html
        var htm = this.md.render(this.getmd());

        //:emoji:
        //emojiParse自己实现
        htm = emojiParse(htm);

        //赋值视图
        //set html
        this.sethtml(htm);

        //Prevent internal rendering		
        //阻止内部渲染 
        return false;
    },

    //Trigger command callback
    //触发命令回调
    cmdcallback: function (cmd) {
        if (cmd == "emoji") {
            $('#myModalEmoji').modal();
        }
    }
});

//default remarkable parse，默认解析方式
console.log(nmd.md.render('# netnrmd!'));
// => <h1>netnrmd!</h1>
```

### Function 方法

```js
var nmd = new netnrmd('#txt');
console.log(nmd);

//focus 焦点选中
nmd.focus();

//set height 设置高度
nmd.height(200);

//toggle Full Screen 切换全屏
nmd.toggleFullScreen();
nmd.toggleFullScreen(true);
nmd.toggleFullScreen(false);

//toggle Split Screen 切换分屏
nmd.toggleSplitScreen();
nmd.toggleSplitScreen(true);
nmd.toggleSplitScreen(false);

//toggle Preview 预览切换
nmd.togglePreview();
nmd.togglePreview(true);
nmd.togglePreview(false);

//set markdown 赋值
nmd.setmd(md);

//get markdown 取值
nmd.getmd();

//set html 赋值
nmd.sethtml(html);

//get html 取值
nmd.gethtml();

//clear markdown&html 清空
nmd.clear();

//render 渲染
nmd.render();


//插入内容 重要，控制文本域的操作
var ops = {
	cmd: cmdname,				//不为空即可
    txt: nmd.obj.textarea[0],	//文本域对象
    before: '**',				//插入的内容前缀
    defaultvalue: '加粗',		//未选中内容时，默认内容，会选中
    after: '**'					//插入的内容后缀
};
netnrmd.insertxt(ops);
//有选中内容XXX，输出：**XXX**
//未选择内容，输出：**加粗**
//before 到 after 之间的内容会选中
```
### Textarea Extend 文本域拓展 

```
var txtDom = $('#txt')[0];

//获取光标位置
netnrmd.getCursortPosition(txtDom);

//设置光标位置
netnrmd.getCursortPosition(txtDom, 3);

//获取选中文字
netnrmd.getSelectText(txtDom);

//选中特定范围的文本
netnrmd.setSelectText(txtDom, 1, 3);

//在光标后插入文本
netnrmd.insertAfterText(txtDom, "text");
```

## Authors 作者

- [netnr](https://www.netnr.com) [github/netnr](https://github.com/netnr) 
- Jon Schlinkert [github/jonschlinkert](https://github.com/jonschlinkert)
