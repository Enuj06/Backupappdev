<template>
    <div>
      <Logo /><br>
      <AdminNav />
  
      <h1>Frequently Asked Questions</h1>
  
      <!-- Form for creating new FAQ -->
      <form @submit.prevent="createFAQ">
        <label>Question:</label>
        <input v-model="newQuestion" required>
  
        <label>Answer:</label>
        <textarea v-model="newAnswer" required></textarea>
  
        <button type="submit">Create FAQ</button>
      </form>
  
      <h3>Table</h3>
  
      <!-- Display FAQs in a wider table -->
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Question</th>
            <th>Answer</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="faq in info" :key="faq.id">
            <td>{{ faq.id }}</td>
            <td>{{ faq.question }}</td>
            <td>{{ faq.answer }}</td>
            <td>
              <button @click="editFAQ(faq)">Edit</button>
              <button @click="deleteFAQ(faq.id)">Delete</button>
            </td>
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
    name: 'FaqsAdmin',
    components: {
      AdminNav,
      Logo,
    },
    data() {
      return {
        info: [],
        newQuestion: '',
        newAnswer: '',
      };
    },
    created() {
      this.getInfo();
    },
    methods: {
      async getInfo() {
        try {
          const inf = await axios.get('/getFAQ');
          this.info = inf.data;
        } catch (error) {
          console.log(error);
        }
      },
  
      async createFAQ() {
        try {
          const response = await axios.post('/create-faq', {
            question: this.newQuestion,
            answer: this.newAnswer,
          });
  
          if (response.data.msg === 'okay') {
            // Refresh the FAQs after creating a new one
            this.getInfo();
            // Clear the form fields
            this.newQuestion = '';
            this.newAnswer = '';
          } else {
            console.error('Failed to create FAQ');
          }
        } catch (error) {
          console.error('Error creating FAQ:', error);
        }
      },
  
      editFAQ(faq) {
        this.newQuestion = faq.question;
        this.newAnswer = faq.answer;
  
        // Now, you can navigate to an edit page or show a modal
        this.$router.push({ name: 'EditFAQView', params: { id: faq.id } });
      },
  
      async deleteFAQ(id) {
        try {
          const response = await axios.get('/delete-faq/' + id);
  
          if (response.data.msg === 'okay') {
            this.getInfo();
          } else {
            console.error('Failed to delete FAQ');
          }
        } catch (error) {
          console.error('Error deleting FAQ:', error);
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
  
  form {
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
  textarea {
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
  </style>
  