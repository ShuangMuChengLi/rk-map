## 优先改
1. （已修改）测距画完以后，页面图形不会出来，移动一下地图才出来。清空图绘制元素也一样的BUG   拖动地图  才消失
2. （已修改）测距会请求http://localhost:5000/image/marker.png.不合理
3. （已修改）测距设置的颜色跟显示的颜色不一致
4. 点击后出现两个重叠图标
5. （已修改）不设置鼠标滑过的样式。鼠标滑过，元素消失了。应该默认不变才对(初始化加了透明度0.9   鼠标滑过时估计设置为了1.0。不设置滑过样式，api就不要动就好，不要有任何设置)
6. （已修改）vector.getSource().addFeatures。图标添加第一个没问题，执行第二次，标记第二个点就标记不出来
````
    getNewVector(){
      var srcOpt = {
      };
      // 数据容器
      var vecSrc = new RKVectorSource(srcOpt);

      // 图层样式
      const layerStyleOption = {
        imgSrc: require('./images/marker.png')
      };
      // 矢量图层
      const opt_layer = {
        opacity: 0.9,
        vectorSource: vecSrc,
        layerStyle: layerStyleOption
      };

      var vecLayer = new RKVectorLayer(opt_layer);
      rkMap.addLayer(vecLayer);
      return vecLayer;
    },
    addFeatures(vector, point){
      // 标签数据
      var rkFeature = new RKFeatures(
        [
          {
            lonLat: point,
          }
        ]);
      console.log(rkFeature.getFeatures())
      vector.getSource().addFeatures(rkFeature.getFeatures());
    },
````
7. （已修改）缺少demo或API：矩形、圆形、不规则图形的手动框选，框选完获取地图上的点位数组，然后点位高亮。框选图形的层级要比点位低，不能遮住点位。取消框选，点位样式还原。
8. 地图轨迹播放功能封装，要存储点位数组的信息到propertys里，每个点位必然包含lonlat。
形式一：图标自动在轨迹上移动；形式二：每个图标依次高亮
9.  （已修改）api中图片用import img from './marker.png'方式引用
10. （已修改）要实现markerVector.setLayerStyle('layerStyle', layerStyleOption);功能
11. （已修改）未设置样式的情况下   刚刚的写法（第9点）  图片能出来了  但出现Uncaught TypeError: Cannot set property 'text' of undefined 错误at RKStyleFun.initSingleStyle (RKStyleFun.js:268)
12. （已修改）drawOption传递了两次，太重复。startDraw的drawOption建议非必填。RKMeasureTool的drawOption中的type建议能生效
13. 图标选中后，点击地图其他地方，不要还原样式。图层提供一个方法，手动调用，然后还原。
14. （已修改）new RKFeatures(pointList)新建feature时，将点位数据存储在properties中。可以通过getProperties()取出
15. setLayerStyle  addLayerEventListener只能执行一次  其他图层再设置就失效了   这个问题很严重  优先排查下
16. 框选的画笔selectLayers如果没传，默认框选所有的
17. addVectorFeatures  addFeatures能不能统一addFeatures。是不是考虑用原生就可以
18. 聚合图层使用setLayerStyle，会导致数字的出现
19. setLayerStyle能否传递函数，待确认
20. 显示隐藏地图弹窗的demo
21. 地图切换的api

## 优化
1. （已修改）图标不设置样式，默认不显示或者显示小圆点。不要依赖图片marker.png
2. （已修改）feature和vector不应该耦合在一起
3. 图标点击后，出现两个叠加的图标
4. 缺少弹窗的demo。showPopupById和showPopupModelById合并。showPopupById不行   可以删了。因为调用第二次  会找不到那个ID
5. （已修改）绘制圆形、矩形、任意图形，框选范围内的点位，并高亮
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
13. （已修改）图层和feature加样式的方法名：setStyle
14. demo要简洁些，明了的告诉读者，api怎么调用。不要过度封装
15. （已修改）移除ol.ropeok.RKFeatureStyle。前端不用调用这个方法，只传递一个对象，api内部自行调用。
16. jsdemo/demo/drawFeatureByData.html打开默认报错，添加按钮不知道什么功能   点了没效果
17. RKDrawFeatureByData  设置了drawLayer为什么还要设置vecSources  建议只传drawLayer
18. vecSources建议命名为 vectorSource，drawLayer建议命名为layer。所有key名，统一，便于前端记忆



