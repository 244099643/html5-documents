Egret框架入门第一步 - 电影剪辑
===============

电影剪辑(MovieClip)是Flash程序员非常熟悉的东西了，历史悠久，虽然Flash历经多次版本变更，但电影剪辑迄今为止一直被广泛的使用着。Egret框架也支持MovieClip，您可以用Flash Pro软件去制作MovieClip，然后使用Egret的扩展工具，导出Egret可识别的格式（类似spritesheet，包括图片和描述文件）。

这里我们还是先使用samples里自带的资源，拷贝/egret/examples/assets/480目录下的两个文件：monkey.json和monkey.png，将这两个文件复制到HelloEgret项目的assets/480目录下面。

先分析一下monkey.json这个描述文件，是如下的结构：

![github](https://raw.githubusercontent.com/NeoGuo/html5-documents/master/egret/images/egret_monkey_json.png "json")

对应到Flash的时间轴上，你可以理解为"stand"是一个关键帧，此帧上又嵌套了一个电影剪辑"stand_inner"，stand_inner的时间轴是19个帧组成，每一帧上放了一个小图片。当你把主时间轴的播放头指向"stand"
，则stand_inner将会不断的重复播放。这样说，应该让您更容易理解Egret的MovieClip的行为方式。

来看看如何实现，首先还是先加载：

```
var loader:ns_egret.LoadingController = new ns_egret.LoadingController();
loader.addResource("monkey.json", ns_egret.ResourceLoader.DATA_TYPE_BINARY);//加载描述文件
loader.addResource("monkey.png", ns_egret.ResourceLoader.DATA_TYPE_IMAGE);//加载图片
loader.addEventListener(ns_egret.ResourceLoader.LOAD_COMPLETE, this.onSpriteSheetLoadComplete, this);//事件侦听加载完成
loader.load();//执行加载
```

然后将大图和描述数据作为参数，创建电影剪辑：

```
private onSpriteSheetLoadComplete():void {
    var data = ns_egret.ResourceLoader.create("monkey.json").data;//获取描述
    data = JSON.parse(data);//将JSON字符串转换为Object
    var texture = ns_egret.TextureCache.getInstance().getTexture("monkey.png");//获取大图
    var monkey = new ns_egret.MovieClip(data, texture);//创建电影剪辑
    var stage = ns_egret.MainContext.instance.stage;//获取Stage引用
    stage.addChild(monkey);//添加到显示列表
    monkey.setInterval(1);//设置动画每帧的时间间隔，单位是毫秒，数值越小，动作越快
    monkey.gotoAndPlay("attack");
}
```
> 通过调节传递给setInterval的参数，可以控制动画播放的速度，实现慢放和快进

完整代码：

```
class Demo3 {

    /**游戏启动后，会自动执行此方法*/
    public startGame():void {
        this.loadResource();
    }
    /**加载所需资源*/
    public loadResource():void {
        var loader:ns_egret.LoadingController = new ns_egret.LoadingController();
        loader.addResource("monkey.json", ns_egret.ResourceLoader.DATA_TYPE_BINARY);//加载描述文件
        loader.addResource("monkey.png", ns_egret.ResourceLoader.DATA_TYPE_IMAGE);//加载图片
        loader.addEventListener(ns_egret.ResourceLoader.LOAD_COMPLETE, this.onSpriteSheetLoadComplete, this);//事件侦听加载完成
        loader.load();//执行加载
    }
    /**精灵表加载完毕后即可使用*/
    private onSpriteSheetLoadComplete():void {
        var data = ns_egret.ResourceLoader.create("monkey.json").data;//获取描述
        data = JSON.parse(data);//将JSON字符串转换为Object
        var texture = ns_egret.TextureCache.getInstance().getTexture("monkey.png");//获取大图
        var monkey = new ns_egret.MovieClip(data, texture);//创建电影剪辑
        var stage = ns_egret.MainContext.instance.stage;//获取Stage引用
        stage.addChild(monkey);//添加到显示列表
        monkey.setInterval(1);//设置动画每帧的时间间隔，单位是毫秒，数值越小，动作越快
        monkey.gotoAndPlay("attack");
    }
}

//create app
var app = new Demo3();
```

将Demo3作为入口类，重新编译项目，即可看到一只猴子的动画：

![github](https://raw.githubusercontent.com/NeoGuo/html5-documents/master/egret/images/movieclip_monkey.jpg "monkey")

电影剪辑的常用方法：

```
monkey.gotoAndPlay("stand");//播放指定动画
monkey.gotoAndStop("stand");//播放并暂停指定动画
monkey.stop();//暂停播放
monkey.getCurrentFrameIndex();//获取当前播放头的位置
monkey.getTotalFrame();//获取总帧数
monkey.getIsPlaying();//判断当前动画是否正在播放
```

- - -

[上一篇:纹理和位图](https://github.com/NeoGuo/html5-documents/blob/master/egret/02-bitmap.md)
| [下一篇:文本](https://github.com/NeoGuo/html5-documents/blob/master/egret/04-text.md)
