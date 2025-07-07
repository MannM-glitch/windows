<template>
  <div id="desktop">
    <div class="buttons">
      <button @click="openWindow('Notes')">📝 New Note</button>
      <button @click="openWindow('Calculator')">🧮 Calculator</button>
      <button @click="openWindow('Browser')">🌐 Browser</button>
    </div>
    <AppWindow
      v-for="(win, index) in windows"
      :key="win.id"
      :title="win.title"
      :z="win.z"
      :type="win.type"
      @close="closeWindow(win.id)"
      @bringToFront="bringToFront(win.id)"
    />
  </div>
</template>

<script>
import AppWindow from "./components/AppWindow.vue";

export default {
  components: { AppWindow },
  data() {
    return {
      windows: [],
      nextId: 1,
      nextZ: 1
    };
  },
  methods: {
    openWindow(type) {
      this.windows.push({
        id: this.nextId++,
        title: type + " " + this.nextId,
        z: this.nextZ++,
        type
      });
    },
    closeWindow(id) {
      this.windows = this.windows.filter(w => w.id !== id);
    },
    bringToFront(id) {
      const win = this.windows.find(w => w.id === id);
      if (win) win.z = this.nextZ++;
    }
  }
};
</script>

<style>
#desktop {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: #dce0e4;
  overflow: hidden;
}
.buttons {
  position: fixed;
  top: 10px;
  left: 10px;
  z-index: 1000;
  display: flex;
  gap: 8px;
}
button {
  padding: 6px 12px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
}
</style>
