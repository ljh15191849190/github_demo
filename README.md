# github_demo


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>检索各单位泊位信息</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.2.1/css/ol.css" type="text/css">
    <script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
    <script src="https://cdn.polyfill.io/v2/polyfill.min.js?features=fetch,requestAnimationFrame,Element.prototype.classList,URL"></script>
    <script src="https://cdn.jsdelivr.net/gh/openlayers/openlayers.github.io@master/en/v6.2.1/build/ol.js"></script>
    <style>
        #map{
            width: 100%;
            height: 500px;
        }
    </style>
</head>
<body>
    <div id="map"></div>

<script type="text/javascript">

    //总泊位数组
     var coordinatesPolygona = new Array();
    //用于测试的一些数据，可以先测试看看好不好用
     var coordinates = [[51.6082, 24.1246],
                [52.4383, 22.9673],
                [54.8306, 22.6825],
                [54.9454, 23.3488],
                [51.6082, 24.1246]]
     var coordinatesPolygon = pushCoordinates(coordinates);
     coordinatesPolygona[0] = coordinatesPolygon;

     coordinates =  [[122.051605773926, 30.6479315948486],
                [122.051605773926, 30.6499896240234],
                [122.051436279297, 30.6499896240234],
                [122.051436279297, 30.6479315948486],
                [122.051605773926, 30.6479315948486]];
     coordinatesPolygon =  pushCoordinates(coordinates);
     coordinatesPolygona[1] = coordinatesPolygon;  

        //普通图层
    var tileLayer = new ol.layer.Tile({
        source:new ol.source.OSM()
    });

    var source = new ol.source.Vector();



    //多边形此处注意一定要是[坐标数组]
    var plygon = new ol.geom.Polygon(coordinatesPolygona);

    //多边形要素类
    var feature = new ol.Feature({
        geometry: plygon,//plygon代表多边形

    });

    /*此处为重要，理解feature和source的关系，也就很简单了
        feature就是我们的画图层
    */
    source.addFeature(feature);


    //矢量图层
    var vector = new ol.layer.Vector({
        source: source,
        style: new ol.style.Style({
            fill: new ol.style.Fill({
                color: 'pink'
            }),

            stroke: new ol.style.Stroke({
                color: 'red',
                width: 5
            }),
            image: new ol.style.Circle({
                radius: 7,
                fill: new ol.style.Fill({
                    color: 'red'
                })
            })
        })
    });

    //我这里没有对map加载进行处理，所以在二次加载时会出现覆盖现象。
    //本意是进页面就加载，没有按钮。所以有需要的可以处理一下
    var map = new ol.Map({
        layers: [tileLayer, vector],

        view:new ol.View({
            center:[54.52, 24.38],
            zoom: 6,
            projection: "EPSG:4326"

        }),
        target: "map"

    });


//画框前置方法
 function pushCoordinates(coordinates){
    //声明一个新的数组
    var coordinatesPolygon = new Array();

    //循环遍历将经纬度转到"EPSG:4326"投影坐标系下

    for (var i = 0; i < coordinates.length; i++) {
        //坐标转换
        var pointTransform = ol.proj.fromLonLat([coordinates[i][0], coordinates[i][1]], "EPSG:4326");
        //形成多边形数组
        coordinatesPolygon.push(pointTransform);

    }

    return coordinatesPolygon;
    
}

// map.on('pointermove', function(evt) {
//     console.log(evt)
//   if (!evt.dragging) {
   
//   }
// });

map.on('click', function(evt) {
    console.log(evt.coordinate)
    map.getLayers().getFeatures(evt.pixel)
//     var polygonLayer=new ol.layer.Vector({
//         source:source,
//         /*图形绘制好时最终呈现的样式,显示在地图上的最终图形*/
//         style: new ol.style.Style({
//             fill: new ol.style.Fill({
//                 color: 'blue'
//             }),
//             stroke: new ol.style.Stroke({
//                 color: 'red',
//                 width: 2
//             }),
//             image: new ol.style.Circle({
//                 radius: 7,
//                 fill: new ol.style.Fill({
//                     color: 'red'
//                 })
//             })
//         })
//     })
//   map.addLayer(polygonLayer);
});


     
</script>
</body>
</html>
