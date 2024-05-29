<template>
  <div>
    <button @click="connectWallet">Подключить кошелек</button>
    <div v-if="account">
      Подключенный аккаунт: {{ account }}
      <input v-model="recipient" placeholder="Адрес получателя" />
      <input v-model="amount" type="number" placeholder="Количество ETH" />
      <button @click="sendTransaction">Отправить транзакцию</button>
    </div>
    <div v-if="transactionHash">
      Хеш транзакции: {{ transactionHash }}
    </div>
  </div>
</template>

<script>
import Web3 from 'web3';

export default {
  data() {
    return {
      web3: null,
      account: null,
      recipient: '',
      amount: 0,
      transactionHash: ''
    };
  },
  methods: {
    async connectWallet() {
      if (window.ethereum) {
        try {
          await window.ethereum.request({ method: 'eth_requestAccounts' });
          this.web3 = new Web3(window.ethereum);
          const accounts = await this.web3.eth.getAccounts();
          this.account = accounts[0];
        } catch (error) {
          console.error('Ошибка подключения кошелька:', error);
        }
      } else {
        alert('Пожалуйста, установите Metamask!');
      }
    },
    async sendTransaction() {
      if (!this.web3) {
        alert('Пожалуйста, подключите кошелек!');
        return;
      }

      if (!this.account || !this.recipient || !this.amount) {
        alert('Пожалуйста, введите все необходимые данные!');
        return;
      }

      try {
        const transactionParameters = {
          to: this.recipient,
          value: this.web3.utils.toHex(this.web3.utils.toWei(this.amount.toString(), 'ether')),
          from: this.account
        };

        const txHash = await this.web3.eth.sendTransaction(transactionParameters);
        this.transactionHash = txHash.transactionHash;
      } catch (error) {
        console.error('Ошибка отправки транзакции:', error);
      }
    }
  }
};
</script>

<style scoped>
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