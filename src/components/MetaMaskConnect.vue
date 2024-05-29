<template>
  <div>
    <button @click="connectMetaMask">Подключить MetaMask</button>
    <div v-if="account">
      Адрес кошелька: {{ account }}
    </div>
  </div>
</template>

<script>
import Web3 from 'web3';

export default {
  data() {
    return {
      account: null,
      web3: null,
    };
  },
  methods: {
    async connectMetaMask() {
      if (window.ethereum) {
        try {
          await window.ethereum.request({ method: 'eth_requestAccounts' });
          this.web3 = new Web3(window.ethereum);
          const accounts = await this.web3.eth.getAccounts();
          this.account = accounts[0];
        } catch (error) {
          console.error('Ошибка подключения MetaMask', error);
        }
      } else {
        console.error('MetaMask не установлен');
      }
    },
  },
};
</script>

<style scoped>
button {
  padding: 10px 20px;
  background-color: #f6851b;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #e2761b;
}
</style>