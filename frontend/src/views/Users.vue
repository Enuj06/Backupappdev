<template>
    <div>
      <Logo /><br>
      <AdminNav />
  
      <table class="user-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Status</th>
            <th>Role</th>
            <th>Created At</th>
            <!-- Add more columns as needed -->
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.id }}</td>
            <td>{{ user.name }}</td>
            <td>{{ user.status }}</td>
            <td>{{ user.role }}</td>
            <td>{{ user.created_at }}</td>
            <!-- Add more columns as needed -->
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import AdminNav from '@/components/AdminNav.vue';
  import Logo from '@/components/Logo.vue';
  
  export default {
    name: 'Users',
    components: {
      Logo,
      AdminNav,
    },
    data() {
      return {
        users: [],
      };
    },
    mounted() {
      // Fetch data when the component is mounted
      this.fetchData();
    },
    methods: {
        fetchData() {
            // Make a GET request to the getData endpoint
            axios.get('/getData', {
                headers: {
                'Authorization': 'Bearer ' + yourTokenVariable, // Replace with your actual token variable
                },
            })
            .then(response => {
                // Check if authentication was successful
                if (response.data && response.data.userData) {
                // Update the users data with the received data
                this.users = response.data;
                } else {
                console.error('Authentication failed:', response.data.error);
                // Handle authentication failure (e.g., redirect to login)
                }
            })
            .catch(error => {
                console.error('Error fetching data:', error);
                // Handle other errors (e.g., show an error message to the user)
            });
        },
    },
  };
  </script>
  
  <style scoped>
  .user-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }
  
  .user-table th, .user-table td {
    border: 1px solid #ddd;
    padding: 12px;
    text-align: left;
  }
  
  .user-table th {
    background-color: #f2f2f2;
  }
  </style>
  