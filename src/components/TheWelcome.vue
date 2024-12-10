<template>
  <div class="container">
    <div class="tabs">
      <button class="tab-button" @click="selectTab('coding')">Coding</button>
      <button class="tab-button" @click="selectTab('design')">Design</button>
      <button class="tab-button" @click="selectTab('video')">Video Editing</button>
      <button class="tab-button" @click="selectTab('language')">Lanaskdoasdiasguage</button>
      <button class="tab-button" @click="selectTab('others')">Others</button>
    </div>

    <div class="content">
      <div v-if="selectedTab === 'coding'" class="item">
        <p>Wants to learn coding, offers graphic design.</p>
        <button class="accept-button" @click="accept('graphic design')">ACCEPT</button>
      </div>
      <div v-if="selectedTab === 'design'" class="item">
        <p>Wants to learn coding, offers video editing.</p>
        <button class="accept-button" @click="accept('video editing')">ACCEPT</button>
      </div>
      <!-- Add more items for other tabs as needed -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedTab: 'coding'
    };
  },
  methods: {
    selectTab(tab) {
      this.selectedTab = tab;
    },
    accept(offering) {
      alert(`You accepted the offering for ${offering}`);
    }
  }
};
</script>

<style scoped>
.container {
  background-color: black;
  color: white;
  padding: 20px;
  width: 500px;
  margin: auto;
  border-radius: 10px;
}

.tabs {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
}

.tab-button {
  background-color: transparent;
  border: 1px solid white;
  color: white;
  padding: 10px;
  cursor: pointer;
}

.tab-button:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.content {
  display: flex;
  justify-content: space-between;
}

.item {
  background-color: #333;
  padding: 20px;
  border-radius: 5px;
  width: 45%;
  text-align: center;
}

.accept-button {
  background-color: blue;
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
}

.accept-button:hover {
  background-color: darkblue;
}
</style>
