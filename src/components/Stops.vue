<template>
  <div class="stops container">
    <Alert v-if="alert" v-bind:message="alert"/>
    <h1 class="page-header">Manage Stops</h1>
    <input class="form-control" placeholder="Enter Stop's Name" v-model="filterInput">
    <br>
    <table class="table table-striped">
      <thead>
      <tr>
        <th>Latitude</th>
        <th>Longitude</th>
        <th>Eta Stop</th>
        <th>Longitude Stop</th>
        <th>Status</th>
        <th>Stop Number</th>
        <th>Name</th>
        <th>Updated Date</th>
        <th>Created Date</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="stop in filterBy(stops, filterInput)">
        <td>{{stop.lat}}</td>
        <td>{{stop.longi}}</td>
        <td>{{stop.eta_stop}}</td>
        <td>{{stop.long_stop}}</td>
        <td>{{stop.status}}</td>
        <td>{{stop.num_stop}}</td>
        <td>{{stop.name}}</td>
        <td>{{stop.updatedAt}}</td>
        <td>{{stop.createdAt}}</td>
        <td>
          <router-link class="btn btn-default" v-bind:to="'/stop/'+stop._id">View</router-link>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import Alert from './Alert';
  import axios from 'axios';

  export default {
    name: 'stops',
    data() {
      return {
        stops: [],
        alert: '',
        filterInput: ''
      }
    },
    methods: {
      fetchStops() {
        //   this.$http.get('http://localhost/stops/public/api/stops')
        //   //this.$http.jsonp('http://localhost/stops/public/api/stops')
        //     .then(function(response){
        //       this.stops = JSON.parse(JSON.stringify(response.body));
        //     });

        axios.defaults.headers.get['Content-Type'] = 'application/json;charset=utf-8';
        axios.defaults.headers.get['Access-Control-Allow-Origin'] = '*';
        axios.get('http://ec2-18-188-110-179.us-east-2.compute.amazonaws.com:3000/stops')
          .then(resp => {
            let result = resp.data;
            console.log(result);
          });
      },
      filterBy(list, value) {
        value = value.charAt(0).toUpperCase() + value.slice(1);
        return list.filter(function (stop) {
          return stop.name.indexOf(value) > -1;
        });
      }
      /*     {
              headers:{
                  'Access-Control-Allow-Origin': '*',
                  'Access-Control-Allow-Methods': 'POST, GET, PUT, OPTIONS, DELETE',
                  'Access-Control-Allow-Headers': 'Access-Control-Allow-Methods, Access-Control-Allow-Origin, Origin, Accept, Content-Type'
          }} */
    },
    created: function () {
      if (this.$route.query.alert) {
        this.alert = this.$route.query.alert;
      }
      this.fetchStops();
    }
    ,
    updated: function () {
      this.fetchStops();
    }
    ,
    components: {
      Alert
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
