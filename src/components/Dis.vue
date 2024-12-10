<script setup>
import { ref } from 'vue';

const acceptedItems = ref([]);
const skills = ref([]);
const newItem = ref("");  // Used to store the new item input for accepting

// Fetch accepted items from the backend
async function fetchAcceptedItems() {
  console.log('Fetching accepted items...');
  try {
    const response = await fetch('http://127.0.0.1:5000/get_accepted_items');
    if (!response.ok) throw new Error('Failed to fetch accepted items');
    const data = await response.json();
    acceptedItems.value = data;
    console.log('Updated acceptedItems:', acceptedItems.value);
  } catch (error) {
    console.error('Error fetching accepted items:', error);
  }
}

// Fetch skills from the backend
async function fetchSkills() {
  console.log('Fetching skills...');
  try {
    const response = await fetch('http://127.0.0.1:5000/get_skills');
    if (!response.ok) throw new Error('Failed to fetch skills');
    const data = await response.json();
    skills.value = data;
    console.log('Updated skills:', skills.value);
  } catch (error) {
    console.error('Error fetching skills:', error);
  }
}

// Handle item acceptance
async function acceptItem() {
  if (!newItem.value) {
    alert("Please provide an item to accept.");
    return;
  }
  
  try {
    const response = await fetch('http://127.0.0.1:5000/accept', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ item: newItem.value }),
    });

    if (!response.ok) throw new Error('Failed to accept item');
    
    const data = await response.json();
    console.log(data.message);

    // After accepting an item, fetch the updated list of accepted items
    fetchAcceptedItems();
    
    // Clear the input field after submitting
    newItem.value = "";
  } catch (error) {
    console.error('Error accepting item:', error);
  }
}
</script>

<template>
  <div class="profile-container">
    <div class="personal-details">
      <div class="picture">
        <img class="profile-pic" src="./../../assets/Profile/profile_pic.jpg" alt="Profile Picture" />
      </div>
      <div class="details">
        <p>Name: {{ name }}</p>
        <p>Email: {{ email }}</p>
        <button class="edit">Edit Profile</button>
      </div>
    </div>

    <div class="public-stats">
      <div class="activity">
        <button class="commision" id="but1" @click="fetchSkills">Requests</button>
        <button class="commision" id="but2" @click="fetchAcceptedItems">Accepted</button>
      </div>
      <button class="add">Add +</button>

      <div class="about">
        <div class="box">
          <h3>Skills</h3>
          <ul>
            <li v-for="(skill, index) in skills" :key="index">
              Known: {{ skill.skill_known }}, Can Teach: {{ skill.skill_can_teach }}
            </li>
          </ul>
        </div>

        <div class="box">
          <h3>Accepted Items</h3>
          <!-- Form to accept new item -->
          <input v-model="newItem" placeholder="Enter item to accept" />
          <button @click="acceptItem">Accept Item</button>

          <ul v-if="acceptedItems.length">
            <li v-for="item in acceptedItems" :key="item.id">
              {{ item.item }}
            </li>
          </ul>
          <p v-else>No accepted items found.</p>
        </div>
      </div>
    </div>
  </div>
</template>
