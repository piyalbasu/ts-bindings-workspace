<script setup lang="ts">
import { ref } from 'vue';
import { isAllowed, setAllowed, getUserInfo } from "@stellar/freighter-api";

const signedInText = ref("");
const isBtnDisabled = ref(false);

  async function getPk() {
    const { publicKey } = await getUserInfo();
    return publicKey;
  }

  async function setLoggedIn() {
    const publicKey = await getPk();

    signedInText.value = `Signed in as ${publicKey}`;
    isBtnDisabled.value = true;
  }



  const handleClick = async () => {
    if (await isAllowed()) {
      if (await getPk()) {
        setLoggedIn();
      } else {
        signedInText.value =  "Freighter is locked.<br>Sign in & refresh the page."
      }
  } else {
      await setAllowed();
      await setLoggedIn();
    };
    
  }
</script>

<template>
  <div id="freighter-wrap" class="wrap" aria-live="polite">
  <div class="ellipsis">
    <div v-if="signedInText">
      {{ signedInText }}
    </div>
    <div v-else>
      <button data-connect aria-controls="freighter-wrap" :disabled="isBtnDisabled" @click="handleClick">Connect</button>
    </div>
  </div>
</div>
</template>