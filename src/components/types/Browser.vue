<template>
  <div>
    <input v-model="url" @keyup.enter="visit" placeholder="Enter URL (e.g. https://example.com)" />
    <iframe v-if="safeUrl" :src="safeUrl" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      url: "",
      safeUrl: ""
    };
  },
  methods: {
    visit() {
      if (this.url.startsWith("http")) {
        this.safeUrl = this.url;
      } else {
        this.safeUrl = "https://" + this.url;
      }
    }
  }
};
</script>

<style scoped>
input {
  width: 100%;
  margin-bottom: 4px;
  padding: 5px;
}
iframe {
  width: 100%;
  height: 100%;
  border: none;
}
</style>
