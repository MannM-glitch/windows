<template>
  <textarea v-model="text" placeholder="Type your notes..." />
</template>

<script>
export default {
  data() {
    return { text: "" };
  }
};
</script>

<style scoped>
textarea {
  width: 100%;
  height: 100%;
  resize: none;
  border: none;
  outline: none;
  padding: 10px;
  font-family: sans-serif;
}
</style>
