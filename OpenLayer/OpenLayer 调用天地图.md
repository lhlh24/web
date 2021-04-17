## OpenLayer调用天地图

### 1.HTML

```html
<!--HTML-->
<template>
    <div id="Map">
        
    </div>
</template>
```

### 2.JavaScript

```JavaScript
/// JS	
	let layers = [
        new ol.layer.Tile({
            source: new ol.source.XYZ({
                url: 'http://t0.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=key',
            }),
        new ol.layer.Tile({
        	source: new ol.source.XYZ({
        		url: 'http://t0.tianditu.com/DataServer?T=cia_w&x={x}&y={y}&l={z}&tk=key',
        	})
		}),
	];
	this.map = new ol.Map({
        target: "Map",
        controls: defaultControls({
            zoom: true
        }).extend([]),
        layers: layers,
        view: new ol.View({
            center: [116.397755, 40.193179],
            zoom: 10,
            maxZoom: 18,
            projection: 'EPSG:4326',
        })
    });
```

### 3.CSS

```css
<!--CSS-->
#Map{
    height: 600px;
    width: 100%;
}
```

