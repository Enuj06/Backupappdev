<template>
  <div>
    <Logo /><br>
    <Hamburger />
    <v-container class="navbar-container" fluid>
      <NavBar />
    </v-container>
    <v-container class="communicate-container" fluid>
      <v-row justify="center" align="center">
        <v-col xs="12" sm="10" md="8" lg="6">
          <v-card-text>
            <v-container class="faq-content">
              <!-- Chatbot response textarea -->
              <textarea
                v-model="chatbotResponse"
                cols="30"
                rows="10"
                placeholder="Chatbot response..."
                readonly
              ></textarea>

              <!-- User input textarea -->
              <textarea
                v-model="userMessage"
                cols="30"
                rows="5"
                placeholder="Input your thoughts here..."
              ></textarea>

              <!-- Submit button -->
              <v-btn type="submit" block class="mt-2">Submit</v-btn>
            </v-container>
          </v-card-text>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios';
import Hamburger from '@/components/Hamburger.vue';
import NavBar from '@/components/NavBar.vue';
import Logo from '@/components/Logo.vue';

export default {
  name: 'Communicate',
  components: {
    Hamburger,
    NavBar,
    Logo,
  },
  data() {
    return {
      tab: null,
      userMessage: '',
      chatbotResponse: '',
    };
  },
  methods: {
    async submitForm() {
      try {
        const response = await axios.post('/chatbotinteraction', {
          message: this.userMessage,
        });

        if (response.data && response.data.response) {
          this.chatbotResponse = response.data.response;
        } else {
          console.error('Invalid response format from server');
        }

        this.userMessage = '';
      } catch (error) {
        console.error('Error submitting form:', error);
      }
    },
  },
};
</script>

<style scoped>
.navbar-container {
  max-width: 800px;
  margin: 0 auto;
}

.communicate-container {
  max-width: 800px;
  margin: 0 auto;
}

.faq-content {
  height: 100%; /* Set the height to 100% */
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #f58be7;
  box-sizing: border-box;
  margin-bottom: 16px;
  resize: vertical;
}

/* Add more styling as needed */
</style>
