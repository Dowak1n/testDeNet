<template>
  <div>
    <input v-model="address" placeholder="Введите адрес кошелька" />
    <button @click="checkBalance">Проверить баланс</button>
    <div v-if="balance !== null">
      Баланс: {{ balance }} токенов
    </div>
    <div v-if="error" style="color: red;">
      Ошибка: {{ error }}
    </div>
  </div>
</template>

<script>
import Web3 from 'web3';

export default {
  data() {
    return {
      address: '',
      balance: null,
      error: null,
      web3: null,
      network: 'mainnet',
      tokenContractAddress: '0x6B175474E89094C44Da98b954EedeAC495271d0F',
      tokenABI: [
        {
          constant: true,
          inputs: [
            {
              name: "_owner",
              type: "address",
            },
          ],
          name: "balanceOf",
          outputs: [
            {
              name: "balance",
              type: "uint256",
            },
          ],
          payable: false,
          stateMutability: "view",
          type: "function",
        },
      ],
    };
  },
  created() {
    this.initWeb3();
  },
  methods: {
    initWeb3() {
      const infuraProjectId = 'efe582c947e941b99aef11728bc4028e';
      let infuraUrl = '';

      if (this.network === 'mainnet') {
        infuraUrl = `https://mainnet.infura.io/v3/${infuraProjectId}`;
      } else if (this.network === 'kovan') {
        infuraUrl = `https://kovan.infura.io/v3/${infuraProjectId}`;
      } else if (this.network === 'polygon') {
        infuraUrl = `https://polygon-mainnet.infura.io/v3/${infuraProjectId}`;
      } else {
        this.error = 'Unsupported network';
        return;
      }

      this.web3 = new Web3(new Web3.providers.HttpProvider(infuraUrl));
    },
    async checkBalance() {
      this.error = null;
      this.balance = null;

      if (this.web3 && this.address) {
        if (!this.web3.utils.isAddress(this.address)) {
          this.error = 'Введите корректный адрес кошелька.';
          return;
        }

        try {
          const contract = new this.web3.eth.Contract(this.tokenABI, this.tokenContractAddress);
          const result = await contract.methods.balanceOf(this.address).call();
          this.balance = this.web3.utils.fromWei(result, 'ether');
        } catch (error) {
          this.error = 'Ошибка получения баланса: ' + error.message;
          console.error('Ошибка получения баланса:', error);
        }
      } else {
        this.error = 'Введите корректный адрес кошелька.';
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