<template>
  <div class="add container">
     <Alert v-if="alert" v-bind:message="alert" />
      <h1 class="page-header">Add Stop</h1>
      <form v-on:submit="addStop">
         <div class="well">
            <h4>Stop Info</h4>
            <div class="form-group">
               <button type="submit" class="btn btn-success">Map</button>
            </div>
            <div class="form-group">
               <label>Latitude</label>
               <input type="text" class="form-control" placeholder="Latitude" v-model="stop.lat">
            </div>
            <div class="form-group">
               <label>Longitude</label>
               <input type="text" class="form-control" placeholder="Longitude" v-model="stop.long">
            </div>

            <div class="form-group">
                  <label>Status</label>
                  <input type="text" class="form-control" placeholder="status" v-model="stop.status">
            </div>
            <div class="form-group">
                  <label>Stop Number</label>
                  <input type="text" class="form-control" placeholder="stop number" v-model="stop.num_stop">
            </div>
            <div class="form-group">
                  <label>Name</label>
                  <input type="text" class="form-control" placeholder="Name" v-model="stop.name">
            </div>
         </div>

         <button type="submit" class="btn btn-primary">Submit</button>
      </form>
   </div>
    
</template>

<script>

import Alert from './Alert'
import axios from 'axios'
const BASE_URL = 'http://ec2-18-188-110-179.us-east-2.compute.amazonaws.com:3000/'

export default {
  name: 'add',
  data () {
    return {
      stop: {},
      alert: ''
    }
  },
  methods: {
     addStop(e) {
         console.log(123); //console test
         if(!this.stop.lat || !this.stop.long || !this.stop.status || !this.stop.num_stop || !this.stop.name){
           this.alert = 'Please fill in all required fields';
         } else if(isNaN(this.stop.lat && this.stop.long)) {
            this.alert = 'Please enter a valid number';
         } else if(typeof(this.stop.status !== 'string')) {
            this.alert = 'Please enter true or false';
        } else {
           //this.$http.post('http://localhost/stops/public/api/stops/add', newStop)
            axios({
               method: 'post', //edit es put
               url:  `${BASE_URL}stops`,
               headers: {}, 
               data: {
                  lat: this.stop.lat,
                  long: this.stop.long,
                  eta_stop: this.stop.eta_stop,
                  long_stop: this.stop.long_stop,
                  status: this.stop.status,
                  num_stop: this.stop.num_stop,
                  name: this.stop.name
               }
            });
            this.$router.push({path:'/', query: {alert: 'Stop Added'}});
         e.preventDefault();
        }
        e.preventDefault();
     }
  },
  components: {
     Alert
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>