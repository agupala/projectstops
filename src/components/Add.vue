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
               <input type="text" class="form-control" placeholder="Longitude" v-model="stop.longi">
            </div>
         
            <div class="form-group">
                  <label>ETA Stop</label>
                  <input type="text" class="form-control" placeholder="Eta Stop" v-model="stop.eta_stop">
            </div>
            <div class="form-group">
                  <label>Longitude Stop</label>
                  <input type="text" class="form-control" placeholder="Longitude Stop" v-model="stop.long_stop">
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
         if(!this.stop.lat || !this.stop.longi || !this.stop.eta_stop || !this.stop.long_stop || !this.stop.num_stop || !this.stop.name){
           console.log('Please fill in all required fields');
           this.alert = 'Please fill in all required fields';
        } else {
           let newStop = {
               lat: this.stop.lat,
               longi: this.stop.longi,
               eta_stop: this.stop.eta_stop,
               long_stop: this.stop.long_stop,
               status: this.stop.status,
               num_stop: this.stop.num_stop,
               name: this.stop.name
           }
           this.$http.post('http://localhost/stops/public/api/stops/add', newStop)
            .then(function(response){
               this.$router.push({path:'/', query: {alert: 'Customer Added'}});
            });
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