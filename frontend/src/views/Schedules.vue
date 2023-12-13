<template>
    <div>
      <Logo /><br>
      <AdminNav />
      <div class="container">
        <h1 class="page-title">Schedules Table</h1>
  
        <!-- Form for creating a new schedule -->
        <form @submit.prevent="createSchedule" class="schedule-form">
          <label for="appointmentDate">Appointment Date:</label>
          <input v-model="appointmentDate" type="date" id="appointmentDate" name="appointmentDate" required>
  
          <label for="startTime">Start Time:</label>
          <input v-model="startTime" type="time" id="startTime" name="startTime" required>
  
          <label for="endTime">End Time:</label>
          <input v-model="endTime" type="time" id="endTime" name="endTime" required>
  
          <label for="status">Status:</label>
          <select v-model="status" id="status" name="status" required>
            <option value="available">Available</option>
            <option value="booked">Booked</option>
          </select>
  
          <button type="submit" class="btn btn-primary">Schedule Appointment</button>
        </form>
  
        <!-- Table to display schedules -->
        <table class="schedule-table">
          <thead>
            <tr>
              <th>Appointment Date</th>
              <th>Start Time</th>
              <th>End Time</th>
              <th>Status</th>
              <th>Edit</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="schedule in schedules" :key="schedule.id">
              <td>{{ schedule.appointment_date }}</td>
              <td>{{ schedule.start_time }}</td>
              <td>{{ schedule.end_time }}</td>
              <td>{{ schedule.status }}</td>
              <td><!-- Add your edit button or link here --></td>
              <td>
                <button @click="deleteSchedule(schedule.id)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </template>

<script>
import AdminNav from '@/components/AdminNav.vue';
import Logo from '@/components/Logo.vue';
import axios from 'axios';

export default {
  name: 'Schedules',
  components: {
    Logo,
    AdminNav,
  },
  data() {
    return {
      appointmentDate: '',
      startTime: '',
      endTime: '',
      status: 'available',
      schedules: [],
    };
  },
  created() {
    this.getSchedules();
  },
  methods: {
    async getSchedules() {
      try {
        const response = await axios.get('/schedule'); // Update the endpoint as needed
        this.schedules = response.data;
      } catch (error) {
        console.error('Error fetching schedules:', error);
      }
    },
    async createSchedule() {
      try {
        const response = await axios.post('/schedule/create', {
          appointment_date: this.appointmentDate,
          start_time: this.startTime,
          end_time: this.endTime,
          status: this.status,
        });

        if (response.data.msg === 'okay') {
          // Handle successful creation, e.g., show a success message or redirect
          console.log('Schedule created successfully');
          // Optionally, you can reset form fields or update the schedule list
          this.appointmentDate = '';
          this.startTime = '';
          this.endTime = '';
          this.status = 'available';
          this.getSchedules();
        } else {
          console.error('Failed to create schedule');
        }
      } catch (error) {
        console.error('Error creating schedule:', error);
      }
    },
    async deleteSchedule(scheduleId) {
      // ... (existing method)
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.page-title {
  text-align: center;
}

.schedule-form {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.schedule-form label {
  display: block;
  margin-bottom: 8px;
}

.schedule-form input,
.schedule-form select {
  width: 100%;
  padding: 8px;
  margin-bottom: 16px;
  box-sizing: border-box;
}

.schedule-form button {
  background-color: #4caf50;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.schedule-form button:hover {
  background-color: #45a049;
}

.schedule-table {
  margin-top: 20px;
  width: 100%;
  border-collapse: collapse;
}

.schedule-table th,
.schedule-table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.schedule-table th {
  background-color: #4caf50;
  color: white;
}
</style>