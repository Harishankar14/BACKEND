<template>
  <div>
    <h1>Submit What You Want to Learn</h1>
    <form @submit.prevent="submitAccept">
      <label for="wantToLearn">What do you want to learn?</label>
      <select id="wantToLearn" v-model="wantToLearn">
        <option disabled value="">Select an option</option>
        <option value="Coding">Coding</option>
        <option value="Painting">Painting</option>
        <option value="Cooking">Cooking</option>
      </select>

      <label for="canTeach">What can you teach?</label>
      <select id="canTeach" v-model="canTeach">
        <option disabled value="">Select an option</option>
        <option value="Math">Math</option>
        <option value="Science">Science</option>
        <option value="Art">Art</option>
      </select>

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
      wantToLearn: "", // Selected "What do you want to learn?" value
      canTeach: "", // Selected "What can you teach?" value
      message: null, // Feedback message
      loading: false, // Loading state
    };
  },
  methods: {
    async submitAccept() {
      // Validate inputs
      if (!this.wantToLearn || !this.canTeach) {
        this.message = "Both fields are required.";
        return;
      }

      this.loading = true; // Set loading state
      this.message = null; // Clear any previous messages

      try {
        const response = await axios.post("http://127.0.0.1:5000/accept", {
          item: `Wants to learn ${this.wantToLearn} and can teach ${this.canTeach}`,
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
