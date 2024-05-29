<template>
  <div>
    <textarea v-model="inputText" placeholder="Введите текст здесь"></textarea>
    <button @click="calculateSha256">Вычислить SHA-256</button>
    <div v-if="hash">
      SHA-256: {{ hash }}
    </div>
  </div>
</template>

<script>
import CryptoJS from 'crypto-js';

export default {
  data() {
    return {
      inputText: '',
      hash: null,
    };
  },
  methods: {
    calculateSha256() {
      this.hash = CryptoJS.SHA256(this.inputText).toString(CryptoJS.enc.Hex);
    },
  },
};
</script>

<style scoped>
textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
button {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
</style>