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
      <q-inner-loading :showing="metamaskLoader">
        <q-circular-progress
          indeterminate
          show-value
          size="90px"
          color="orange"
          track-color="transparent"
        >
          <q-avatar>
            <img :src="require('../assets/metamask-logo.png')" />
          </q-avatar>
        </q-circular-progress>
      </q-inner-loading>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue';
import { useQuasar } from 'quasar';

export default defineComponent({
  name: 'IndexPage',
  setup() {
    const amount = ref(null);
    const metamaskLoader = ref(false);
    const $q = useQuasar();

    return {
      amount,
      metamaskLoader,

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
            metamaskLoader.value = true;
            const response = await web3js.request({
              method: 'eth_requestAccounts',
            });

            if (response.length) {
              metamaskLoader.value = false;
              $q.notify({
                message: 'Wallet connected !',
                icon: 'announcement',
                color: 'green',
              });
            }
          } catch (e) {
            metamaskLoader.value = false;
            $q.notify({
              message: e.message,
              icon: 'warning',
              color: 'red',
            });
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
