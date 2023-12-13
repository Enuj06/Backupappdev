<template>
    <div>
      <Logo /><br>
      <AdminNav />
  
      <h1>Announcements</h1>
  
      <!-- Form for creating new Announcement -->
      <form @submit.prevent="createAnnouncement" class="announcement-form">
        <label>Title:</label>
        <input v-model="newAnnouncement.title" required>
  
        <label>Content:</label>
        <textarea v-model="newAnnouncement.content" required></textarea>
  
        <label>Upload Image:</label>
        <input @change="onFileChange" type="file" accept="image/*" required>
  
        <button type="submit">Create Announcement</button>
      </form>
  
      <h3>Announcements Table</h3>
  
      <!-- Display Announcements in a wider table -->
      <table>
        <thead>
          <tr>
            <th>Title</th>
            <th>Content</th>
            <th>Image</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="announcement in announcements" :key="announcement.id">
            <td>{{ announcement.title }}</td>
            <td>{{ announcement.content }}</td>
            <td>
              <img :src="announcement.image_url" alt="Announcement Image" />
            </td>
            <td>
              <button @click="editAnnouncement(announcement)" class="action-button">Edit</button>
              <button @click="deleteAnnouncement(announcement.id)" class="action-button">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  
  <script>
  import axios from 'axios';
  import Logo from '@/components/Logo.vue';
  import AdminNav from '@/components/AdminNav.vue';
  
  export default {
    name: 'Announcements',
    components: {
      Logo,
      AdminNav,
    },
    data() {
      return {
        announcements: [],
        newAnnouncement: {
          title: '',
          content: '',
          image: null,
        },
      };
    },
    mounted() {
      this.fetchAnnouncements();
    },
    methods: {
      onFileChange(event) {
        this.newAnnouncement.image = event.target.files[0];
      },
      async fetchAnnouncements() {
        try {
          const response = await axios.get('/fetch-announcements');
          this.announcements = response.data;
        } catch (error) {
          console.error('Error fetching announcements:', error);
        }
      },
      async createAnnouncement() {
        try {
          const formData = new FormData();
          formData.append('title', this.newAnnouncement.title);
          formData.append('content', this.newAnnouncement.content);
          formData.append('image', this.newAnnouncement.image);
  
          const response = await axios.post('/create-announcement', formData);
  
          if (response.status === 200) {
            this.fetchAnnouncements();
            this.newAnnouncement.title = '';
            this.newAnnouncement.content = '';
            this.newAnnouncement.image = null;
          } else {
            console.error('Failed to add announcement');
          }
        } catch (error) {
          console.error('Error creating announcement:', error);
        }
      },
    },
  };
  </script>
<style scoped>
h1,
h3 {
  text-align: center;
}

table,
th,
td {
  border: 1px solid black;
  margin-left: auto;
  margin-right: auto;
  border-collapse: collapse;
  width: 800px; /* Adjust the width as needed */
  text-align: center;
  font-size: 20px;
}

.announcement-form {
  max-width: 800px; /* Adjust the width as needed */
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

label {
  display: block;
  margin-bottom: 8px;
}

input,
textarea,
button {
  width: 100%;
  padding: 12px;
  border: 1px solid #f58be7;
  box-sizing: border-box;
  margin-bottom: 16px;
  resize: vertical;
}

button {
  background-color: #4caf50;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

.action-button {
  background-color: #4caf50;
  color: white;
  padding: 6px 10px; /* Adjust the padding as needed */
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px; /* Adjust the font size as needed */
  margin-right: 5px; /* Add margin between buttons if necessary */
}

.action-button:hover {
  background-color: #45a049;
}

</style>    