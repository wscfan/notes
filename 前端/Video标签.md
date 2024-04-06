# Video标签

## 一、属性

### autoplay

声明该属性后，视频会尽快自动开始播放，不会停下来等待数据全部加载完成。

### controls

加上这个属性，浏览器会在视频底部提供一个控制面板，允许用户控制视频的播放，包括音量，跨帧，暂停/恢复播放。

### controlsList

控制面板上显示的组件。

允许的值有 `nodownload`、`nofullscreen`、`noremoteplayback`、`noplaybackrate`。

### crossorigin

是否使用CORS来获取相关视频。

允许的值有`anonymous`、`use-credentials`。

### disablepictureinpicture

防止浏览器显示画中画上下文菜单或在某些情况下自动请求画中画模式。该属性可以禁用 `video` 元素的画中画特性，右键菜单中的“画中画”选项会被禁用

### height

视频显示区域的高度，单位是像素。

### loop

指定后，会在视频播放结束的时候，自动返回视频开始的地方，继续播放。

### muted

指明在视频中音频的默认设置。设置后，音频会初始化为静音。默认值是 false, 意味着视频播放的时候音频也会播放。

### playsinline

指明视频将内联（inline）播放，即在元素的播放区域内。

### poster

海报帧图片 URL，用于在视频处于下载中的状态时显示。如果未指定该属性，则在视频第一帧可用之前不会显示任何内容，然后将视频的第一帧会作为海报（poster）帧来显示。

### preload

在播放视频之前，加载哪些内容。

允许的值有`none`、`metadata`、`auto`、`空字符串`。

### width

视频显示区域的宽度，单位是像素。



