<template>
  <div
    class="window"
    :style="{
      top: pos.y + 'px',
      left: pos.x + 'px',
      width: size.w + 'px',
      height: size.h + 'px',
      zIndex: z
    }"
    @mousedown="bringToFront"
  >
    <div class="title-bar" @mousedown="startDrag">
      <span>{{ title }}</span>
      <button class="close" @click="$emit('close')">×</button>
    </div>
    <div class="content">
      <component :is="typeComponent" />
    </div>
    <div class="resize-handle" @mousedown.stop="startResize"></div>
  </div>
</template>

<script>
import Notes from "./types/Notes.vue";
import Calculator from "./types/Calculator.vue";
import Browser from "./types/Browser.vue";

export default {
  props: ["title", "z", "type"],
  components: { Notes, Calculator, Browser },
  data() {
    return {
      pos: { x: 100 + Math.random() * 200, y: 100 + Math.random() * 100 },
      size: { w: 300, h: 200 },
      dragging: false,
      resizing: false,
      offset: { x: 0, y: 0 },
      originalSize: { w: 0, h: 0 }
    };
  },
  computed: {
    typeComponent() {
      return this.type || "Notes";
    }
  },
  methods: {
    startDrag(e) {
      this.dragging = true;
      this.offset = {
        x: e.clientX - this.pos.x,
        y: e.clientY - this.pos.y
      };
      document.addEventListener("mousemove", this.onDrag);
      document.addEventListener("mouseup", this.endDrag);
    },
    onDrag(e) {
      if (this.dragging) {
        this.pos.x = e.clientX - this.offset.x;
        this.pos.y = e.clientY - this.offset.y;
      }
    },
    endDrag() {
      this.dragging = false;
      this.resizing = false;
      document.removeEventListener("mousemove", this.onDrag);
      document.removeEventListener("mousemove", this.onResize);
      document.removeEventListener("mouseup", this.endDrag);
    },
    startResize(e) {
      this.resizing = true;
      this.offset = { x: e.clientX, y: e.clientY };
      this.originalSize = { ...this.size };
      document.addEventListener("mousemove", this.onResize);
      document.addEventListener("mouseup", this.endDrag);
    },
    onResize(e) {
      if (this.resizing) {
        this.size.w = Math.max(200, this.originalSize.w + (e.clientX - this.offset.x));
        this.size.h = Math.max(100, this.originalSize.h + (e.clientY - this.offset.y));
      }
    },
    bringToFront() {
      this.$emit("bringToFront");
    }
  }
};
</script>

<style scoped>
.window {
  position: absolute;
  background: white;
  border: 1px solid #aaa;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  border-radius: 6px;
  overflow: hidden;
}
.title-bar {
  height: 30px;
  background: #eee;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  cursor: grab;
  font-weight: bold;
}
.close {
  border: none;
  background: transparent;
  font-size: 18px;
  cursor: pointer;
}
.content {
  padding: 10px;
  overflow: auto;
  height: calc(100% - 30px);
}
.resize-handle {
  position: absolute;
  right: 0;
  bottom: 0;
  width: 12px;
  height: 12px;
  background: transparent;
  cursor: nwse-resize;
}
</style>
