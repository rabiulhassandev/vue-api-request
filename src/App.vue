<template>
  <h1 class="text-center">API Request</h1>

  <div class="container">
    <div class="row">
      <div class="col-md-4" v-for="user in userObj.users" :key="user.id">
        <div class="card mb-3">
          <div class="card-body">
            <h5 class="card-title">{{ user.name }}</h5>
            <p class="card-text">{{ user.email }}</p>
            <p class="card-text"><small class="text-muted">{{ user.phone }}</small></p>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>


<script setup>
  import axios from 'axios';
  import { reactive, onMounted } from 'vue';

  const userObj = reactive({
    users: []
  })


  onMounted(() => {

    // get data by Fetch API
    fetch('https://jsonplaceholder.typicode.com/users')
    .then((response) => response.json())
    .then((data) => {
      userObj.users = data
    })

    // get data by Axios
    // axios.get('https://jsonplaceholder.typicode.com/users')
    // .then((response) => {
    //   userObj.users = response.data;
    // })



})

</script>