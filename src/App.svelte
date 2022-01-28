<script>
  import { provinces } from '../saprovinces.js'
  import { stadiums } from '../stadiums-geo.js'
  import { onMount } from 'svelte'
  import L from 'leaflet'
  console.log(stadiums)
  import Stadiums from './Stadiums.svelte'

  onMount(() => {
    // addMap()
  })

  function mapStyle(feature) {
    return {
      color: '#eeeeee',
      weight: 1,
      opacity: 0.65,
    }
  }

  var geojsonMarkerOptions = {
    radius: 2,
    fillColor: '#ff7800',
    color: '#000',
    weight: 1,
    opacity: 1,
    fillOpacity: 0.8,
  }

  function onEachFeature(feature, layer) {
    // console.log(feature)
    // let popupContent = feature.properties
    // layer.bindPopup(popupContent)
  }

  function addMap() {
    const map = L.map('map', {
      zoomControl: false,
      scrollWheelZoom: true,
      dragging: false,
      zoomSnap: 0.1,
      center: [0, 0],
      zoom: 2,
      zoomControl: false,
      attributionControl: false,
    }).setView([-28.1018753, 24.8843883], 6)

    L.tileLayer(
      'https://stamen-tiles-{s}.a.ssl.fastly.net/toner-lite/{z}/{x}/{y}{r}.{ext}',
      {
        attribution: '',
        subdomains: 'abcd',
        minZoom: 0,
        maxZoom: 20,
        ext: 'png',
      }
    ).addTo(map)

    let outline = L.geoJSON(provinces, {
      onEachFeature: onEachFeature,
      style: mapStyle,
    }).addTo(map)
    map.fitBounds(outline.getBounds(), { padding: [10, 10] })

    L.geoJSON(stadiums, {
      pointToLayer: function (feature, latlng) {
        return L.circleMarker(latlng, geojsonMarkerOptions)
      },
      onEachFeature: function (feature, layer) {
        let pu = feature.properties.stadium
        pu += '<br />' + feature.properties.code
        layer.bindPopup(pu)
      },
    }).addTo(map)
  }
</script>

<main>
  <!-- <div id="map" /> -->

  <Stadiums />
</main>

<style>
  #map {
    width: 96%;
    margin-left: auto;
    margin-right: auto;
    min-height: 500px;
    /* border: solid 1px gray; */
  }
</style>
