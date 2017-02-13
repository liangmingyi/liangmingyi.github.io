title: WEB APP开发项目总结
date: 2013-05-07 15:13:37
tags: 项目总结
---

## 前人的经验
第一次做WEB APP，自然少不了看些前人总结的一些文章，补充下基本的知识，这里有两篇总结的比较好的文章，对viewpoint、meta、mediaquery、touch事件等基本知识。还有一篇是豆瓣的一位前端在开发豆瓣阅读时的总结，写的非常棒。

> [webapp开发要点](http://www.cnblogs.com/pifoo/archive/2011/05/28/webkit-webapp.html)
> [在移动浏览器中踩过的坑](http://huisecheng.com/blog/2013/01/ugly-tips-in-mobile-f2e-development/)
> [当前 WEB APP 开发的最佳实践](http://lyric.im/best-practice-for-web-app-development/)

## 弹性盒子模型
对一个父元素使用display: box其实就是弹性盒子模型的声明，毕竟属于CSS3的东西，使用的时候，需要附带私有前缀。就是诸如-moz-, -webkit-之类。

```
#father {
    -webkit-display: box;
    -moz-display: box; 
    display: box; 
}
```

假设父元素下有三个子元素，它们的样式如下

```
#first_boy { -webkit-box-flex: 2; -mox-box-flex: 2; box-flex: 2; }
#second_boy { -webkit-box-flex: 1; -mox-box-flex: 1; box-flex: 1; }
#three_boy { -webkit-box-flex: 1; -mox-box-flex: 1; box-flex: 1; }
```

上面的样式解释下，就是将父元素水平分成四等份，其中#first_boy占2份，second_boy和three_boy各占一份。

下面接着来介绍下在父元素上还有的几个非常厉害的属性

> box-orient: 用来确定子元素的方向。是横着排还是竖着走。可选的值有: horizontal | vertical | inline-axis | block-axis | inherit
> box-direction: 是用来确定子元素的排列顺序，可选值有: normal | reverse | inherit
> box-align与box-pack都是决定盒子内部剩余空间怎么使用的。在行为效果上就是表现为“对齐”。其中box-align决定了垂直方向上的空间利用，也就是垂直方向上的对齐表现。为了便于记忆，我们可以拿来和CSS2中的vertical-align隐射记忆，两者都有”align”，都是都是垂直方向的对齐；而剩下的box-pack就是水平方向的了。可选参数有: start | end | center | baseline | stretch
其中stretch为默认值，为拉伸，也就是父标签高度过高，其孩子元素的高度就多高，以后等高布局不用愁了。start表示顶边对齐，end为底部对齐，center为居中对齐，baseline表示基线（英文字母o,m,n等的底边位置线）对齐
> box-pack: 决定了父标签水平遗留空间的使用，其可选值有: start | end | center | justify，就大部分的行为表现来说分别对应text-align属性的值：left | right | center | justify


## 使用localStorage遇到的一些问题

> localStorage是同步的，会阻塞页面的渲染
> 对I/O有操作，读取数据消耗的时间随机
> 有大小限制，一般默认为5M，在内存吃紧时会丢失数据

## 总结一下在这次webapp开发过程中程序和设计上遇到的坑

> 设计上用了太多的图片，每一个标题文字都用的是图片，直接导致资源文件太多，并且为了适应多屏，图片会被各种拉伸，导致布局起来异常艰难
> 在手机浏览器上对position: fixed支持的不是很好，并且顶部的状态栏设计的是个不规则形状的图片，不能定高。在不同屏幕尺寸下等比拉升后，尤其在横屏时，高度会变得非常高，就一个状态栏就能占满整个屏幕
> 没有考虑到任何硬件加速的 CSS 3 属性的动画效果，所有的动画只能通过传统的DOM换图，导致性能卡死
因为主要是在智能终端上做音乐播放功能，完全没有必要使用SoundManager插件去兼容各种情况，应该直接用HTML5的audio API来播放音乐。用了这个插件在各个平台下会导致各种奇怪头疼的问题。

对于audio API，github上依旧有各种对audio api的详细的示例代码 [http://learnshare.github.io/labs/audio/audio-apis.html](http://learnshare.github.io/labs/audio/audio-apis.html)
项目地址：[https://github.com/LearnShare/LearnShare.github.io/tree/master/labs/audio](https://github.com/LearnShare/LearnShare.github.io/tree/master/labs/audio)


