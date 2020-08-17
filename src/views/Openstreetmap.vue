<template>
	<div class="openstreetmap">
		<l-map style="height: 550px" :zoom="zoom" :center="center" @click="onMapClick">
			<l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
			<l-marker v-for="(location, index) in locations" :lat-lng="location.position" v-bind:key="index">
				<l-popup :content="location.name" ></l-popup>
			</l-marker>

			<l-marker ref="marker" :lat-lng="[newLt, newLng]" :icon="icon">
				<l-popup ref="popup" :content="newLoc" :options="{ autoClose: false, closeOnClick: false }"></l-popup>
			</l-marker>


		</l-map>
	</div>
</template>
<style type="text/css">
.leaflet-popup-close-button {
	display: none; 
}
</style>
<script>
	import {LMap, LTileLayer, LMarker, LPopup} from 'vue2-leaflet';
	import L from 'leaflet'
	import 'leaflet/dist/leaflet.css'

	delete L.Icon.Default.prototype._getIconUrl
	L.Icon.Default.imagePath = ''
	L.Icon.Default.mergeOptions({
		iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
		iconUrl: require('leaflet/dist/images/marker-icon.png'),
		shadowUrl: require('leaflet/dist/images/marker-shadow.png')
	})

	export default {
		name: 'Openstreetmap',
		components: {
			LMap,
			LTileLayer,
			LMarker,
			LPopup,
		},
		data: () => ({
			zoom:10.5,
			center: L.latLng(-7.797068, 110.370529),
			url:'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
			attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
			'<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ' +
			'Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
			id: 'mapbox/streets-v11',

			newLoc: '',
			newLt : 0, 
			newLng : 0, 
			
			locations: [
			{ name: 'Yogyakarta',position: [-7.803180,110.362472] },
			{ name: 'Bantul',position: [-7.889228,110.330543] },
			{ name: 'Kulon Progo',position: [ -7.8593006,110.158195] },
			{ name: 'Gunung Kidul',position: [-7.965057,110.601768] },
			{ name: 'Sleman',position: [ -7.691939,110.340499] },
			{ name: 'Purworejo',position: [ -7.718476,110.008163] },
			{ name: 'Kutoarjo',position: [ -7.725280,109.910659] },
			{ name: 'Klaten',position: [ -7.679690,110.624771] }
			],
			icon: L.icon({iconUrl: "null",}),
		}),
		mounted ()  {
			this.$nextTick(() => {
				this.$refs.marker.mapObject.openPopup();
			});
			this.newLoc = this.locations[0].name
			this.newLt = this.locations[0].position[0]
			this.newLng = this.locations[0].position[1]
		},
		methods: {
			onMapClick(e) {
				this.newLt = e.latlng.lat
				this.newLng = e.latlng.lng
				this.newLoc = 'Location on click: '+(e.latlng.lat).toFixed(6)+','+(e.latlng.lng).toFixed(6)
			}
		}

	}
</script>