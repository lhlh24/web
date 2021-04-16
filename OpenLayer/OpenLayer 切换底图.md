### OpenLayer切换天地图底图

#### 1.HTML代码

```HTML
<el-button type="primary" @click="change_img">影像</el-button>
<el-button type="primary" @click="change_vec">街道</el-button>
<el-button type="primary" @click="change_ter">地形</el-button>
```

#### 2.js代码

```JavaScript
// 影像
change_img (){
    let img = new ol.layer.Tile({
        source: new ol.source.XYZ({
            url:  'http://t3.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=key'
        })
    });
    let cia = new ol.layer.Tiler({
        source: new XYZ({
            url:  "http://t0.tianditu.com/DataServer?T=cia_w&x={x}&y={y}&l={z}&tk=key"
        })
    });
    this.map.addLayer(img);
    this.map.addLayer(cia);
},
// 街道
change_vec (){
    let img = new ol.layer.Tile({
        source: new ol.source.XYZ({
            url:  'http://t3.tianditu.com/DataServer?T=cva_w&x={x}&y={y}&l={z}&tk=key'
        })
    });
    let cia = new ol.layer.Tiler({
        source: new XYZ({
            url:  "http://t4.tianditu.com/DataServer?T=vec_w&x={x}&y={y}&l={z}&tk=key"
        })
    });
    this.map.addLayer(img);
    this.map.addLayer(cia);
},
// 地形
change_ter (){
    let img = new ol.layer.Tile({
        source: new ol.source.XYZ({
            url:  'http://t4.tianditu.com/DataServer?T=ter_w&x={x}&y={y}&l={z}&tk=key'
        })
    });
    let cia = new ol.layer.Tiler({
        source: new XYZ({
            url:  "http://t4.tianditu.com/DataServer?T=cva_w&x={x}&y={y}&l={z}&tk=key"
        })
    });
    this.map.addLayer(img);
    this.map.addLayer(cia);
},
```

