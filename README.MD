## 优先改
1. （已修改）测距画完以后，页面图形不会出来，移动一下地图才出来。清空图绘制元素也一样的BUG   拖动地图  才消失
2. 测距会请求http://localhost:5000/image/marker.png.不合理
3. 测距设置的颜色跟现实的颜色不一致
4.


## 优化
1. 图标不设置样式，默认不显示或者显示小圆点。不要依赖图片marker.png
2. feature和vector不应该耦合在一起
3. 图标点击后，出现两个叠加的图标
4. 缺少弹窗的demo。showPopupById和showPopupModelById合并。showPopupById不行   可以删了。因为调用第二次  会找不到那个ID
5. 绘制圆形、矩形、任意图形，框选范围内的点位，并高亮
6. 绘制轨迹，车辆在轨迹上移动，画方圆五百米的圆，框选并高亮范围内点位。
7. showMovePopup()文档不全，没有参数传递
8. 弹窗隐藏的方法是哪个
9. clusterLayer.html  initClusterSelectedFn第一个参数不是一个数组，但demo却去判断length
![Image text](https://github.com/ShuangMuChengLi/rk-map/blob/master/clickFeature.png)
10. 移除以下代码，使用addEventListener、removeEventListener代替
````
var rkInteraction = new ol.ropeok.RKInteraction(rkMap, {"deselectFlag": true});
rkInteraction.enableMoveSelectInteraction([vecLayer]);
rkInteraction.enableSelectInteraction([vecLayer]);
rkInteraction.enableRightSelectInteraction([vecLayer]);
````

11. 事件名：select movein moveout rightdown

    样式：selectedStyle moveinStyle moveoutStyle rightdownStyle
12. ol.ropeok.RKFeatures、new ol.ropeok.RKVectorLayer建议就一个参数
````
new ol.ropeok.RKFeatures(
        features,
        true,
        vecLayer);
````
````
// 矢量图层
const opt_layer = {
    vectorSource:vecSrc,
    styleOption: {},
    styleFunction: styleFunction
};
// 标签样式，若有设置styleFunction，则styleOption无效，未设置styleFunction时，则使用styleOption中的样式
var vecLayer = new ol.ropeok.RKVectorLayer(opt_layer);
````
13. 图层和feature加样式的方法名：setStyle
14. demo要简洁些，明了的告诉读者，api怎么调用。不要过度封装
15. 移除ol.ropeok.RKFeatureStyle。前端不用调用这个方法，只传递一个对象，api内部自行调用。
16. jsdemo/demo/drawFeatureByData.html打开默认报错，添加按钮不知道什么功能   点了没效果
17. RKDrawFeatureByData  设置了drawLayer为什么还要设置vecSources  建议只传drawLayer
18. vecSources建议命名为 vectorSource，drawLayer建议命名为layer。所有key名，统一，便于前端记忆


