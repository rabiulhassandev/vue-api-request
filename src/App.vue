<template>
  <h1 class="text-center text-white py-2 pb-3">API Request</h1>

  <div class="container pb-5">
    <div class="row">
      <div class="col-md-6">
        <div class="card border-0 bg-white">
          <h4 class="text-dark text-center py-3">User List (From: jsonplaceholder online)</h4>
          <div class="row p-2">
            <!-- Users -->
            <div class="col-12 d-flex justify-content-center mb-4" v-if="!dataObj.users.isLoading">
              <Loader />
            </div>
            <div class="col-md-6" v-for="user in dataObj.users" :key="user.id" v-else>
              <div class="card mb-3 border-primary">
                <div class="card-body">
                  <h6 class="card-title text-primary">{{ user.name }}</h6>
                  <p class="card-text">{{ user.email }}</p>
                  <p class="card-text"><small class="text-muted">{{ user.phone }}</small></p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="col-md-6">
        <div class="card border-0 px-2">
          <h4 class="text-dark text-center py-3">Destination List (From: local JSON)</h4>
          <div class="d-flex justify-content-center mb-5" v-if="!dataObj.destinations.isLoading">
            <Loader />
          </div>
          <div class="table-responsive" v-else>
            <table class="table table-bordered table-striped">
              <thead>
                <tr class="table-primary">
                  <th>Name</th>
                  <th>Days</th>
                  <th>Price</th>
                </tr>
              </thead>
              <tbody>
                <!-- Destinations -->
                <tr v-for="destination in dataObj.destinations" :key="destination.id">
                  <td>{{ destination.name }}, {{ destination.country }}</td>
                  <td>{{ destination.days }}</td>
                  <td>{{ destination.price }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>


<script setup>
import axios from 'axios';
import { ref, reactive, onMounted } from 'vue';

let loadingUsers = ref(false);
let loadingDestinations = ref(false);
const dataObj = reactive({
  users: [],
  destinations: []
})


onMounted(() => {
  // load users
  loadUsers();
  // load destinations
  loadDestinations();
})


// load users
function loadUsers() {
  dataObj.users.isLoading = false;
  fetch('https://jsonplaceholder.typicode.com/users')
    .then((response) => response.json())
    .then((data) => {
      Promise.resolve(setTimeout(() => {
        dataObj.users = data;
        dataObj.users.isLoading = true;
      }, 1000))
    })
}

// load destinations
function loadDestinations() {
  dataObj.destinations.isLoading = false;

  axios.get('http://localhost:3000/destinations')
    .then((response) => {
      Promise.resolve(setTimeout(() => {
        dataObj.destinations = response.data;
        dataObj.destinations.isLoading = true;
      }, 3000))
    })
}

</script>