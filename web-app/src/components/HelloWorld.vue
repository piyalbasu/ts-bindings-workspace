<script setup lang="ts">
import { ref } from 'vue';
import * as HelloContract from "bindings/hello-soroban-client";
import * as IncrementContract from "bindings/incrementor-client";


const greeter = new HelloContract.Contract({
  ...HelloContract.networks.testnet,
  rpcUrl: "https://soroban-testnet.stellar.org", // from https://soroban.stellar.org/docs/reference/rpc#public-rpc-providers
});

const incrementor = new IncrementContract.Contract({
  ...IncrementContract.networks.testnet,
  rpcUrl: "https://soroban-testnet.stellar.org", // from https://soroban.stellar.org/docs/reference/rpc-list#sdf-futurenet-and-testnet-only
});

let count = ref(0);
let isBtnDisabled = ref(false);


const increment = async () => {
  isBtnDisabled.value = true;
  const tx = await incrementor.increment();
  const r = await tx.signAndSend();

  const result = r.result;

  count.value = result;
  isBtnDisabled.value = false;
}
 

const { result } = await greeter.hello({ to: "Soroban" });

</script>

<template>
  <h1>{{ result.join(" ") }}</h1>

  <div class="card">
    <button type="button" :disabled="isBtnDisabled" @click="increment">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Install
    <a href="https://github.com/vuejs/language-tools" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}

button:disabled {
  cursor: wait;
}
</style>
