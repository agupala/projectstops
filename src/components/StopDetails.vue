<template>
  <div class="details container">
     <router-link to="/">Back</router-link>
    <h1 class="page-header">{{stop.name}}
      <span class="pull-right">
        <router-link class="btn btn-primary" v-bind:to="'/edit/'+stop._id" >Edit</router-link>
        <button class="btn btn-danger" v-on:click="deleteStop(stop._id)">Delete</button>
      </span>
    </h1>
   <ul class="list-group">
      <li class="list-group-item">Latitud: {{stop.lat}}</li>
      <li class="list-group-item">Longitud: {{stop.longi}}</li>
      <li class="list-group-item">Tiempo de parada: {{stop.eta_stop}}</li>
      <li class="list-group-item">Radio parada: {{stop.long_stop}}</li>
      <li class="list-group-item">Status: {{stop.status}}</li>
      <li class="list-group-item">Numero parada: {{stop.num_stop}}</li>
      <li class="list-group-item">Modificado: {{stop.updatedAt}}</li>
      <li class="list-group-item">Creado: {{stop.createdAt}}</li>
   </ul>
  </div>
</template>

<script>
export default {
  name: 'stopdetails',
  data () {
    return {
      stop: ''
    }
  },
  methods: {
     fetchStop(id) {
      this.$http.get('http://localhost/stops/public/api/stop/'+id)
        .then(function(response){
          this.stop = JSON.parse(JSON.stringify(response.body));
        });
     },
     deleteStop(id) {
      //   console.log(id)
      this.$http.delete('http://localhost/stops/public/api/stops/delete/'+id)
      .then(function(response){
         this.$router.push({path: '/', query: {alert: 'Customer Deleted'}});
      });
     }
  },
  created: function(id) {
     this.fetchStop(this.$route.params.id)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>