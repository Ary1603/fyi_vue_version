<template>
  <table class="table mt-5">
    <thead>
      <tr>
        <th scope="col">ID</th>
        <th scope="col">Name</th>
        <th scope="col">Email</th>
        <th scope="col">Website</th>
        <th scope="col">Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="user in users">
        <th scope="row">{{ user.id }}</th>
        <td>{{user.name}}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.website }}</td>
        <td><i id="icon-map" class="fa-solid fa-map-location-dot icon-table " @click="mapView(user.address.geo.lat, user.address.geo.lng)"></i> <i id="icon-delete" class="fa-solid fa-trash icon-table"></i></td>
      </tr>
      
    </tbody>
  </table>

  <GoogleMap  api-key="AIzaSyBxvspNzUmVsPmQO7D7vhvpVxq41aQxTas" style="width: 100%; height: 500px" :center="center" :zoom="4" v-model="center">
    <Marker :options="{ position: center }" />
  </GoogleMap>
</template>


<script>

import { defineComponent } from "vue";
import { GoogleMap, Marker } from "vue3-google-map";
export default defineComponent({
  components: { GoogleMap, Marker },
  
  data() {
    return {
      users: [],
      center: {lat: 24.886, lng: -70.268},
    };
  },
  created() {
    this.fetchUsers();
    this.mapView();
  },
  methods: {
    fetchUsers() {
      fetch("https://jsonplaceholder.typicode.com/users ", {
        method: "GET",
      })
        .then((response) => response.json())
        .then((data) => {
          this.users = data;
          console.log(data);
        })
        .catch((error) => console.error("Error en la peticion: " + error));
    },
    mapView(lat, lng) {
      if(lat && lng){
        this.center.lat = Number(lat)
        this.center.lng = Number(lng)
      }
      
      console.log(this.center)
    }
  },
});
</script>

<style>
.icon-table{
  margin: 0 8px;
}
#icon-map{
  color: #35d4c7;
}

#icon-delete{
  color: brown;
}
</style>



