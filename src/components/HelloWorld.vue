<template>
  <div>
    
    
    <h1>Submit Your Skills</h1>
    <form @submit.prevent="submitSkills">
      <label for="skillKnown">What skill do you know?</label>
      <input
        type="text"
        id="skillKnown"
        v-model="skillKnown"
        placeholder="Enter a skill you know"
      />

      <label for="skillTeach">What skill can you teach?</label>
      <input
        type="text"
        id="skillTeach"
        v-model="skillTeach"
        placeholder="Enter a skill you can teach"
      />

      <button type="submit" :disabled="loading">Submit</button>
    </form>

    <div v-if="message">
      <p>{{ message }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      skillKnown: "",
      skillTeach: "",
      message: null,
      loading: false, // Add loading state
    };
  },
  methods: {
    async submitSkills() {
      // Validate inputs
      if (!this.skillKnown || !this.skillTeach) {
        this.message = "Both fields are required.";
        return;
      }

      this.loading = true; // Set loading state
      this.message = null; // Clear any previous messages

      try {
        const response = await axios.post("http://127.0.0.1:5000/submit_skills", {
          skill_known: this.skillKnown,
          skill_can_teach: this.skillTeach,
        });
        this.message = "Data submitted successfully!";
        console.log("Response:", response.data);
      } catch (error) {
        // Handle error response
        if (error.response && error.response.data && error.response.data.error) {
          this.message = error.response.data.error; // Show server error message
        } else {
          this.message = "Error submitting data. Please try again.";
        }
        console.error("Error:", error);
      } finally {
        this.loading = false; // Reset loading state
      }
    },
  },
};
</script>

<style>
/* Add some styling if needed */
</style>
