h5 develop example

## H5开发笔记
这两天H5静态页面开发过程中的问题总结。
##### 1.viewport - 移动开发必须的配置
```
//内容宽为设备宽度，初始化缩放倍数为1（不缩放）
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
```


#### 2.rem和px
(1)px是相对于显示器屏幕分辨率而言的相对长度单位。

(2)rem是相对根元素的font-size大小的相对单位，可以做到只修改根元素font-size大小就成比例地调整所有字体大小。

适配：
- 利用媒体查询设置断点来控制HTML的font-size
```
html {         
    font-size:16px;  
}  
@media (max-width:414px) 
{ 
    html {         
        font-size: 18px; 
    }  
} 
@media (max-width:375px) { 
    html { 
        font-size: 20px;
    }
}
```
- 根据屏幕大小动态设置html的font-size  
```
var deviceWidth=document.documentElement.clientWidth;  
var rootFontSize = deviceWidth / 6.4; 
document.documentElement.style.fontSize=rootFontSize + 'px';
```
#### 3.同行元素上下位移偏差问题
手动设置元素居中，要不然浏览器会随机渲染，必须给它一个渲染规则。
#### 4.元素设置display:inline-block,自动产生间距
解决:给父元素设置font-size:0； letter-spacing: -4px; 子元素再另外设置font-size和letter-spacing。
#### 5.自适应布局
- 父元素设置display:flex ,子元素flex属性值设置比列。

- 利用百分百（%）布局

#### 6.调试换分辨率设备自动重布局 
```
(function (doc, win) {     
    resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',     
    recalc = function () {      
        var clientWidth=doc.documentElement.clientWidth;         
        if (!clientWidth) return;   
            //动态计算根元素的font-size
            doc.documentElement.style.fontSize=(clientWidth / 6.4) + 'px';     
        };      
        if (!doc.addEventListener) return; 
        
        //监听设备变化
        win.addEventListener(resizeEvt, recalc, false);  
        //监听浏览窗口变化
        doc.addEventListener('DOMContentLoaded', recalc, false);     
})(document, window); 
//orientationchange：设备更换事件 
//onresize：接收reset事件时触发的EventHandler 
//DOMContentLoaded: 浏览器窗口大小发生变化事件
```
#### 7.总结
H5页面的开发，因为设备视口大小不一致，在适配上应该足够细致、严谨。除了自适应的布局之外，更应该注意细节的处理。
