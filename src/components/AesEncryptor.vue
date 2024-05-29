<template>
  <div>
    <textarea v-model="inputText" placeholder="Введите текст здесь"></textarea>
    <input v-model="key" placeholder="Введите ключ шифрования" />
    <button @click="encryptText">Зашифровать текст</button>
    <div v-if="encryptedText">
      Зашифрованный текст: {{ encryptedText }}
    </div>
  </div>
</template>

<script>
import CryptoJS from 'crypto-js';

export default {
  data() {
    return {
      inputText: '',
      key: '',
      encryptedText: null,
    };
  },
  methods: {
    encryptText() {
      if (this.inputText && this.key) {
        const encrypted = CryptoJS.AES.encrypt(this.inputText, this.key).toString();
        this.encryptedText = encrypted;
      } else {
        alert('Введите текст и ключ шифрования.');
      }
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
input {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
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