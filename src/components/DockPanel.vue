<template>
  <div class="dock-container">
    <div class="dock-panel">
      <button v-for="(app, index) in apps" :key="index" @click="openApp(app.url)">
        {{ app.name }}
      </button>
    </div>
    <div class="iframe-container">
      <iframe v-if="currentUrl" :src="currentUrl" frameborder="0"></iframe>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      apps: [],
      currentUrl: ''
    };
  },
  created() {
    this.loadApps();
  },
  methods: {
    async loadApps() {
      try {
        const response = await fetch('apps.json');
        const data = await response.json();
        this.apps = data;
      } catch (error) {
        console.error('Ошибка загрузки списка приложений:', error);
      }
    },
    openApp(url) {
      this.currentUrl = url;
    }
  }
};
</script>

<style scoped>
.dock-container {
  display: flex;
  height: 100vh;
}
.dock-panel {
  width: 200px;
  background-color: #f8f8f8;
  padding: 10px;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
}
.dock-panel button {
  display: block;
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  text-align: left;
}
.dock-panel button:hover {
  background-color: #45a049;
}
.iframe-container {
  flex-grow: 1;
  background-color: #fff;
}
.iframe-container iframe {
  width: 100%;
  height: 100%;
  border: none;
}
</style>