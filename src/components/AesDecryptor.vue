<template>
  <div>
    <textarea v-model="encryptedText" placeholder="Введите зашифрованный текст здесь"></textarea>
    <input v-model="key" placeholder="Введите ключ для расшифровки" />
    <button @click="decryptText">Расшифровать текст</button>
    <div v-if="decryptedText !== null">
      Расшифрованный текст: {{ decryptedText }}
    </div>
  </div>
</template>

<script>
import CryptoJS from 'crypto-js';

export default {
  data() {
    return {
      encryptedText: '',
      key: '',
      decryptedText: null,
    };
  },
  methods: {
    decryptText() {
      if (this.encryptedText && this.key) {
        try {
          const bytes = CryptoJS.AES.decrypt(this.encryptedText, this.key);
          const originalText = bytes.toString(CryptoJS.enc.Utf8);
          this.decryptedText = originalText ? originalText : 'Неверный ключ или текст';
        } catch (error) {
          this.decryptedText = 'Ошибка расшифровки';
        }
      } else {
        alert('Введите зашифрованный текст и ключ для расшифровки.');
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