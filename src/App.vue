<template>
  <main>
    <MetaMaskConnect />
    <Sha256Calculator />
    <DockPanel />
    <AesEncryptor />
    <AesDecryptor />
    <BalanceChecker />
    <TransactionSender />
    <Erc20TransactionSender />
  </main>
</template>

<script>
import DockPanel from './components/DockPanel.vue';
import MetaMaskConnect from './components/MetaMaskConnect.vue';
import Sha256Calculator from './components/Sha256Calculator.vue';
import AesEncryptor from './components/AesEncryptor.vue';
import AesDecryptor from './components/AesDecryptor.vue';
import BalanceChecker from './components/BalanceChecker.vue';
import TransactionSender from './components/TransactionSender.vue';
import Erc20TransactionSender from './components/Erc20TransactionSender.vue';

export default {
  components: {
    MetaMaskConnect,
    Sha256Calculator,
    DockPanel,
    AesEncryptor,
    AesDecryptor,
    BalanceChecker,
    TransactionSender,
    Erc20TransactionSender,
  },
};
</script>

<style scoped>

</style>
