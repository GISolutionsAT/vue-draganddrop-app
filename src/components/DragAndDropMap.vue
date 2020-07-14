<template>
  <div id="draganddrop-map-container" />
</template>

<script>
import geojsonData from '../assets/data/defis.json';
import 'ol/ol.css';
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import Stamen from 'ol/source/Stamen';
import VectorLayer from 'ol/layer/Vector';
import VectorSource from 'ol/source/Vector';
import { Circle as CircleStyle, Fill, Stroke, Style } from 'ol/style';
import {GPX, GeoJSON, IGC, KML, TopoJSON} from 'ol/format';
import {defaults as defaultInteractions, DragAndDrop} from 'ol/interaction';


// Style function for point features
    let pointFeatureStyle = function(){
      return new Style({
        image: new CircleStyle({
          radius: 3,
          stroke: new Stroke({
            color: '#fff',
            width: 1.5
          }),
          fill: new Fill({
            color: '#007f54'
          })
        })
      })
    };
export default {
  name: 'OpenlayersDraganddropMap',
  data (){
    return {
      dataList: geojsonData
    }
  },
  mounted(){
    const me = this;
    // Drag and Drop
    const dragAndDropInteraction = new DragAndDrop({
      formatConstructors: [
        GPX,
        GeoJSON,
        IGC,
        KML,
        TopoJSON
      ]
    });

    // Define Vector Source
    const vectorSource = new VectorSource({
      features: new GeoJSON().readFeatures(geojsonData, {
        dataProjection: 'EPSG:4326'
      })
    });
    vectorSource.getFeatures().forEach((feature) => {
      feature.getGeometry().transform('EPSG:4326', 'EPSG:3857');
    });

    // Create Vector Layer
    const vectorPointLayer = new VectorLayer({
      source: vectorSource,
      style: pointFeatureStyle
    });


    // Create Tile Layer
    const stamenTileLayer = new TileLayer({
        source: new Stamen({
          layer: 'toner'
        })
    });

    // Main Map
    me.map = new Map({
      target: 'draganddrop-map-container',
      interactions: defaultInteractions().extend([dragAndDropInteraction]),
      layers: [stamenTileLayer, vectorPointLayer],
      view: new View({
        center: [1822497, 6141544],
        zoom: 12
      })
    });

    // Drag and Drop Interaction
    dragAndDropInteraction.on('addfeatures', function(event) {
      var vectorSource = new VectorSource({
        features: event.features
      });
      me.map.addLayer(new VectorLayer({
        source: vectorSource
      }));
      me.map.getView().fit(vectorSource.getExtent());
    });

    const displayFeatureInfo = function(pixel) {
      var features = [];
      me.map.forEachFeatureAtPixel(pixel, function(feature) {
        features.push(feature);
      });
      if (features.length > 0) {
        var info = [];
        var i, ii;
        for (i = 0, ii = features.length; i < ii; ++i) {
          info.push(features[i].get('name'));
        }
        // document.getElementById('info').innerHTML = info.join(', ') || '&nbsp';
      } else {
        // document.getElementById('info').innerHTML = '&nbsp;';
      }
    };

      me.map.on('pointermove', function(evt) {
          if (evt.dragging) {
            return;
          }
          var pixel = me.map.getEventPixel(evt.originalEvent);
          displayFeatureInfo(pixel);
      });

      me.map.on('click', function(evt) {
        displayFeatureInfo(evt.pixel);
      });

  }
}
</script>

<style scoped>
#draganddrop-map-container {
  height: 600px;
  width: 100%;
}
</style>
