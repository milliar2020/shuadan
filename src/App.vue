<template>
  <div id="app">
    <div>Current location:{{province}}</div>
    <div>Current location:{{city}}</div>
    <div>Current location:{{district}}</div>
    <div>Current location:{{adress}}</div>
    <div>lat:{{lat}}</div>
    <div>lng:{{lng}}</div>
    <button v-on:click="getLocation" >Locate</button>
  </div>
</template>

<script>
import AMap from 'AMap'
import { location } from "./utils/locationutil";

export default {
  name: 'app',
  data () {
    return {
      city: '',
      province: '',
      district: '',
      adress: '',
      lat: 0,
      lng: 0
    }
  },
  methods: {
    getLocation() {
      let _that = this;
      let geolocation = location.initMap("map-container"); //定位
      AMap.event.addListener(geolocation, "complete", result => {
        _that.lat = result.position.lat;
        _that.lng = result.position.lng;
        _that.province = result.addressComponent.province;
        _that.city = result.addressComponent.city;
        _that.district = result.addressComponent.district;
        _that.adress = result.formattedAddress;
      });
    }
  },
  mounted () {
    this.getLocation(); // 调用获取地理位置
  },
  plusReady: function () {
    console.log(this.os.plus)
    console.log('plusReady')
    this.$geolocation.getCurrentPosition().then(function (position) {
      this.city = position.address.city
      this.$nativeUI.toast(JSON.stringify(position))
    }.bind(this))
  }
}
</script>
