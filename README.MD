## 优先改

## 已修改
1. （已修改）测距画完以后，页面图形不会出来，移动一下地图才出来。清空图绘制元素也一样的BUG   拖动地图  才消失
2. （已修改）测距会请求http://localhost:5000/image/marker.png.不合理
3. （已修改）测距设置的颜色跟显示的颜色不一致
4. （已修改）不设置鼠标滑过的样式。鼠标滑过，元素消失了。应该默认不变才对(初始化加了透明度0.9   鼠标滑过时估计设置为了1.0。不设置滑过样式，api就不要动就好，不要有任何设置)
5. （已修改）vector.getSource().addFeatures。图标添加第一个没问题，执行第二次，标记第二个点就标记不出来
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
6. （已修改）缺少demo或API：矩形、圆形、不规则图形的手动框选，框选完获取地图上的点位数组，然后点位高亮。框选图形的层级要比点位低，不能遮住点位。取消框选，点位样式还原。
7 （已修改）地图轨迹播放功能封装，要存储点位数组的信息到properties里，每个点位必然包含lonlat。
形式一：图标自动在轨迹上移动；形式二：每个图标依次高亮
8.  （已修改）api中图片用import img from './marker.png'方式引用
9. （已修改）要实现markerVector.setLayerStyle('layerStyle', layerStyleOption);功能
10. （已修改）未设置样式的情况下   刚刚的写法（第9点）  图片能出来了  但出现Uncaught TypeError: Cannot set property 'text' of undefined 错误at RKStyleFun.initSingleStyle (RKStyleFun.js:268)
11. （已修改）drawOption传递了两次，太重复。startDraw的drawOption建议非必填。RKMeasureTool的drawOption中的type建议能生效
12. （已修改）new RKFeatures(pointList)新建feature时，将点位数据存储在properties中。可以通过getProperties()取出
13. （已修改）setLayerStyle  addLayerEventListener只能执行一次  其他图层再设置就失效了   这个问题很严重  优先排查下
14. （已修改）框选的画笔selectLayers如果没传，默认框选所有的
15. （已修改）聚合图层使用setLayerStyle，会导致数字的出现
16. （已修改）setLayerStyle能否传递函数
17. （已修改）显示隐藏地图弹窗的demo（有api，但最好加demo)
18. （已修改）地图切换的api以及demo
19. （已修改）框选实现右键取消框选画笔状态。遇到了一个问题  用户开始准备框选  鼠标变成了画笔  突然不想画了   右键取消画笔
20. （已修改）设置了vector.addLayerEventListener('movein'）  但没设置movein样式。会导致图标消失，期望默认使用normal样式  不要变
21. （已修改）轨迹demo要执行start()，然后执行startManualTrack()才有效。如何一开始就进行就能手动播放轨迹？
22. （已修改）轨迹demo中的白色动球圆是哪里设置，如何设置一个自定义图片。
23. （已修改）doAction:不要必填
24. （已修改）vector.setLayerStyle('layerStyle', (data)=>{    设置了color: '#fff'没生效
25. （已修改）聚合图层用resourceVectorSet[type].vector.getSource().clear();清空后，点位只是隐藏，没有移除，再显示图层，点位又出来了。vector.getLayerSourse().clear();
26. （已修改）轨迹点位初始化和高亮默认imageAnchor: [0.5, 0.5]
27. （已修改）trackTool.startAutoTrack多次执行，导致动球反复渲染，多个叠加。希望startAutoTrack和endAutoTrack能够清空动球。
28. （已修改）执行一次  nextTrack，但doAction触发了两次
29. (已修改)点击轨迹上的点，能够高亮，并触发事件，并触发手动播放模式，并将当前播放的index设置为这个点
30. (已修改)所有的api的feature都要参照第8、14条：要存储点位数组的信息到properties里。目前轨迹doAction获取到的feature没有properties信息
31. (已修改)提供清除轨迹的api——drawTool.clear();
32. (已修改)停止轨迹手动播放API——drawTool.endManualTrack();
33. (已修改)drawTool.drawCircle(option);确认一下半径的单位是什么。如果不是米，要改为米 —— 测量工具下测量验证后是以米为单位。
34. (已修改)轨迹自动播放，需要暂停和继续的功能 —— drawTool.pauseAutoTrack(); 第一次为暂停，第二次为继续，依次类推。
35. (已修改)多个点位  地图居中 -- rkMap.fitViewZoom(pointList); var pointList = [{ id: '1', lonLat: [118.15, 24.51], text: '1'}, {id: '2', lonLat: [118.14, 24.5],text: '2'}
36. (已修改)vecLayer1.getSource().addRkFeatures([{
        id: '11',
        lonLat: [118.171, 24.511],
        text: '11'
    }, {
        id: '21',
        lonLat: [118.161, 24.52],
        text: '21'
    }
    ]).setCenter(); -- 修改为vecLayer.addRkFeatures([{},{}]).updateView();
    
37. (已修改)删除startManualTrack 、endManualTrack  ；startAutoTrack自动停止手动播放；prevTrack、nextTrack自动停止自动播放；doAction等其他播放设置放在setTrack中；
38. (已修改)缺少轨迹自动播放结束的回调函数
39. (已修改)mapServiceObj的projection和RKMap的projection区别 ——mapServiceObj的projection是view的
-- mapServiceObj 的 projection 是用于通过url加载的数据源source的投影坐标,会影响到source的显示。
-- RKMap 的projection 设置的是view(视图)的projection。 其作用之一是，当绘制点，线，面的时候，其坐标需要符合projection投影的坐标要求。否则就会显示不出来或者
显示到其他较远的地方。
40. (已修改)不设置projection，updateView失效
41. (已修改)获取某点根据X,Y像素的偏移后的经纬度 - rkMap.setCenter(coor, offset) ,offset = [ - width/2 ,- heigth /2]
42. 点击后出现两个重叠图标
43. vector.addLayerEventListener('moveout'）没触发
44. vecLayer.setFeatureSelected(feature); 和vecLayer.cancelFeatureSelected(feature)添加demo。支持两种参数feature以及ID
--使用方法,传入feature的id 或 feature本身。 vecLayer.setFeatureSelected(id)
45. 发现之前函数名称命名错误：原来函数为vecLayer.getLayerSourse() ,现在改为vecLayer.getLayerSource().原本的getLayerSourse()当前版本也还可以使用。
以后可能会删除这个错误命名的函数。
46. 手动\自动画一条线，高亮线条周边X米范围的点位  -- 使用方法：参考drawTool.html 
var drawTool = new ol.ropeok.RKDrawFeature(rkMap, drawOption, drawCallBack) 中的第二个参数：drawOption = {
        drawSelect:true [表示执行选中绘制图形内的元素],  selectedRadium:50 [用于指定绘制线段时候，线段周围50米的范围内为选中范围，单位米], ...}
drawTool.setSelectedRadium(60) 设置selecedRadium的半径。
47. 图标选中后，点击地图其他地方，不要还原样式。图层提供一个方法，手动调用，然后还原。
48. 点击矢量图层，高亮，点击地图空白地方，高亮图标会一闪一闪
49. 点击的高亮，要能够通过vector.cancelFeatureSelected(currentActivePoint);取消高亮
50. 矢量图层点击高亮后 ，清空图层，其他图标消失，但高亮图标还在。点击地图其他地方高亮消失，然后报错
51. 聚合图层存在类似2、3问题，而且点击地图空白地方，图标会消失，期望不消失
52. （已修改）弹窗隐藏的方法是哪个
53. （已修改）clusterLayer.html  initClusterSelectedFn第一个参数不是一个数组，但demo却去判断length
![Image text](https://github.com/ShuangMuChengLi/rk-map/blob/master/clickFeature.png)
54. （已修改）移除以下代码，使用addEventListener、removeEventListener代替
````
var rkInteraction = new ol.ropeok.RKInteraction(rkMap, {"deselectFlag": true});
rkInteraction.enableMoveSelectInteraction([vecLayer]);
rkInteraction.enableSelectInteraction([vecLayer]);
rkInteraction.enableRightSelectInteraction([vecLayer]);
````

55. （已修改）事件名：select movein moveout rightdown

    样式：selectedStyle moveinStyle moveoutStyle rightdownStyle
56. （已修改）ol.ropeok.RKFeatures、new ol.ropeok.RKVectorLayer建议就一个参数
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
57. （已修改）图层和feature加样式的方法名：setStyle
58. （已修改）demo要简洁些，明了的告诉读者，api怎么调用。不要过度封装
59. （已修改）移除ol.ropeok.RKFeatureStyle。前端不用调用这个方法，只传递一个对象，api内部自行调用。
60. （已修改）jsdemo/demo/drawFeatureByData.html打开默认报错，添加按钮不知道什么功能   点了没效果
61. （已修改）RKDrawFeatureByData  设置了drawLayer为什么还要设置vecSources  建议只传drawLayer
62. （已修改）vecSources建议命名为 vectorSource，drawLayer建议命名为layer。所有key名，统一，便于前端记忆
63. （已修改）图标不设置样式，默认不显示或者显示小圆点。不要依赖图片marker.png
64. （已修改）feature和vector不应该耦合在一起
65. （已修改）图标点击后，出现两个叠加的图标
66. （已修改）缺少弹窗的demo。showPopupById和showPopupModelById合并。showPopupById不行   可以删了。因为调用第二次  会找不到那个ID
67. （已修改）绘制圆形、矩形、任意图形，框选范围内的点位，并高亮
68. （已修改）绘制轨迹，车辆在轨迹上移动，画方圆五百米的圆，框选并高亮范围内点位。

## 优化
1. showMovePopup()文档不全，没有参数传递
2. source的api整合到vector中


## 笔记
1. vecLayer.setFeatureSelected(feature); 和vecLayer.cancelFeatureSelected(feature)
