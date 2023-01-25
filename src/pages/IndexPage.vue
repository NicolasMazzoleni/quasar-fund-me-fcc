<template>
  <q-page class="row items-center justify-evenly">
    <div class="q-pa-md q-gutter-sm">
      <q-btn @click="connect" color="secondary" label="Connect" />
      <q-btn @click="getBalance" color="accent" label="getBalance" />
      <q-btn @click="withdraw" color="negative" label="Withraw" />
      <br /><br />
      <q-form @click="onSubmit" class="q-gutter-md">
        <q-input filled type="number" v-model="amount" label="Montant" />

        <div>
          <q-btn label="Fund" type="submit" color="primary" />
        </div>
      </q-form>
      <q-inner-loading :showing="loader">
        <q-spinner-gears size="50px" color="orange-8" />
      </q-inner-loading>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'IndexPage',
  setup() {
    const amount = ref(null);
    const loader = ref(false);

    return {
      amount,
      loader,

      onSubmit() {
        console.log('yo', amount.value);
      },

      withdraw() {
        console.log('withdraw');
      },

      async connect() {
        const web3js = window.ethereum;
        if (web3js !== undefined) {
          try {
            loader.value = true;
            const response = await web3js.request({
              method: 'eth_requestAccounts',
            });

            if (response.length) {
              loader.value = false;
            }
          } catch (e) {
            loader.value = false;
          }
        }
      },

      getBalance() {
        console.log('get Balance');
      },
    };
  },
});
</script>
