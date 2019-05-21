<template>
  <div class="edit container">
   <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Edit Stop</h1>
    <form v-on:submit="updateStop">
      <div class="well">
         <h4>Stop Info</h4>
         <div class="form-group">
               <label>Latitud</label>
               <input type="text" class="form-control" placeholder="Latitud" v-model="stop.lat">
               <label>Longitud</label>
               <input type="text" class="form-control" placeholder="Longitud" v-model="stop.long">
               <label>Eta Stop</label>
               <input type="text" class="form-control" placeholder="Eta stop" v-model="stop.eta_stop">
               <label>Longitude Stop</label>
               <input type="text" class="form-control" placeholder="Longitude stop" v-model="stop.long_stop">
               <label>Status</label>
               <input type="text" class="form-control" placeholder="Status" v-model="stop.status">
               <label>Stop Number</label>
               <input type="text" class="form-control" placeholder="Stop number" v-model="stop.num_stop">
               <label>Name</label>
               <input type="text" class="form-control" placeholder="Name" v-model="stop.name">
               <label>Date updated</label>
               <input type="text" class="form-control" placeholder="Date updated" v-model="stop.updatedAt">
               <label>Date Created</label>
               <input type="text" class="form-control" placeholder="Date created" v-model="stop.createdAt">
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
        alert:''
        }
    },
    methods: {
        fetchStop(id){
/*           this.$http.get('http://localhost/stops/public/api/stop/'+id)
          .then(function(response){
            this.stop = JSON.parse(JSON.stringify(response.body));
            }); */
        axios.get(`${BASE_URL}stops/`+id)
          .then(resp => {
            //let result = resp.data;
            this.stop = JSON.parse(JSON.stringify(resp.data));
            // console.log(result);
        });
        },
        updateStop(e){
         if(!this.stop.lat || !this.stop.long || !this.stop.eta_stop || !this.stop.long_stop || !this.stop.num_stop || !this.stop.name){
           this.alert = 'Please fill in all required fields';
         } else {

          //  this.$http.put('http://localhost/stops/public/api/stops/update/'+this.$route.params.id, updateStop)
          //   .then(function(response){
          //      this.$router.push({path:'/', query: {alert: 'Stop Updated'}});
          //   });
            axios({
              method: 'put', //edit es put
              url: `${BASE_URL}stops/${this.$route.params.id}`,
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
            }).catch((error) => {
        // Error
        if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            // console.log(error.response.data);
            // console.log(error.response.status);
            // console.log(error.response.headers);
            this.$router.push({path: `/stops/${this.$route.params.id}`, query: {alert: 'Error en Update'}});
        } else if (error.request) {
            this.$router.push({path: `/stops/${this.$route.params.id}`, query: {alert: 'Error en Update'}});
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            console.log(error.request);
        } else {
            // Something happened in setting up the request that triggered an Error
            //console.log('Error', error.message);
            this.$router.push({path: `/stops/${this.$route.params.id}`, query: {alert: 'Error en Update'}});
        }})
          this.$router.push({path:'/', query: {alert: 'Stop Updated'}});
          e.preventDefault();
         }
         e.preventDefault();
         }
    },
    created: function(){
        this.fetchStop(this.$route.params.id);
    },
    components: {
        Alert
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>