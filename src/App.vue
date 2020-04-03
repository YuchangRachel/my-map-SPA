<template>
<div class="container">

<div class="jumbotron">
<h4 class="display-4">
My Location
</h4>
<p class="lead">Enter your latitude and longitude to display your location on a Google Map</p>
<hr>
<form>
<div class="form-row">
<div class="col">
<input type="text" v-model="latitude" name="lat" id="lat" placeholder="Latitude" class="form-control">
</div>
<div class="col">
<input type="text" v-model="longitude" name="lon" id="lon" placeholder="Longitude" class="form-control">
</div>
<div class="col"">
	<button type="button" @click.stop.prevent="getUserGeo()"><i class="far fa-compass" style="padding: 5px 5px;font-size:15px;color:white"></i></button>
</div>
</div>
<div class="form-row">
<div class="col">
<input type="submit" v-on:click.stop.prevent="showLocation(latitude, longitude)" value="Show My Location" class="btn btn-primary mt-4" >
</div>
</div>
</form>
</div>

<my-map v-show="showMap" v-bind:lat="latitude" v-bind:lon="longitude"></my-map>

</div>
</template>

<script>
const API_KEY = "AIzaSyD_SgaoLq-gKz1aTCAEkq1lNibu7C-L4yw";

const mapComponent = {
	template:
		`<div>
		<img width="560" height="315" :src="map">
		</div>`,
	props: ['lat', 'lon'],
	data: function(){
		return {
			map: "",
		}
	},
	methods: {
		showMap(){
			if (!this.lat || !this.lon){
				this.map = "";
				alert("Enter all valid input");
				return;   /*not continue not show map */
			}
			this.map = `https://maps.googleapis.com/maps/api/staticmap?center=${this.lat},${this.lon}&zoom=14&size=512x512&markers=color:red|${this.lat},${this.lon}|&key=${API_KEY}`;

		}
	}
}

export default {
	name: 'app',

	data () {
		return {
			latitude: "",
			longitude: "",
			showMap: false,
		}
	},

	methods: {
		showLocation: function (latitude, longitude) {
				this.showMap = true;
				this.$children[0].showMap();

		},

		getUserGeo: function(){
			if(navigator.geolocation){
				 navigator.geolocation.getCurrentPosition(this.autoCoordsEntered, this.error);
			}
			else {
				alert('Geolocation is not supported by your browser');
			}
		},
		error: function(){
			alert('Uable to retrieve your location');
		},
		autoCoordsEntered(position) {
			this.latitude= position.coords.latitude;
			this.longitude = position.coords.longitude;
		 }
	},
	components: {
		'my-map': mapComponent
	}
}
</script>

<style scoped>
image {
	text-align: center;
}
button {
	background-color: #4CAF50;
	border-radius: 8px;
}

</style>
