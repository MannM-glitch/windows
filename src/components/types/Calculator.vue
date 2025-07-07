<template>
  <div>
    <input v-model="display" readonly />
    <div class="buttons">
      <button v-for="btn in buttons" @click="press(btn)">{{ btn }}</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      display: "",
      buttons: ["7", "8", "9", "+", "4", "5", "6", "-", "1", "2", "3", "*", "0", "C", "=", "/"]
    };
  },
  methods: {
    press(btn) {
      if (btn === "C") this.display = "";
      else if (btn === "=") {
        try {
          this.display = eval(this.display).toString();
        } catch {
          this.display = "Error";
        }
      } else {
        this.display += btn;
      }
    }
  }
};
</script>

<style scoped>
input {
  width: 100%;
  height: 30px;
  font-size: 16px;
  margin-bottom: 10px;
}
.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 4px;
}
button {
  padding: 8px;
}
</style>
