<template>
  <q-page class="row items-center justify-evenly">
    <div class="q-pa-md q-gutter-sm">
      <q-btn
        @click="connect"
        color="white"
        text-color="black"
        label="Connect"
      />
      <q-btn @click="getBalance" color="primary" label="getBalance" />
      <q-btn @click="withdraw" color="secondary" label="Withraw" />
      <br /><br />
      <q-form @click="onSubmit" class="q-gutter-md">
        <q-input filled type="number" v-model="amount" label="Montant" />

        <div>
          <q-btn label="Fund" type="submit" color="primary" />
        </div>
      </q-form>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'IndexPage',
  setup() {
    const amount = ref(null);

    return {
      amount,

      onSubmit() {
        console.log('yo', amount.value);
      },

      withdraw() {
        console.log('withdraw');
      },

      connect() {
        console.log('connect');
      },

      getBalance() {
        console.log('get Balance');
      },
    };
  },
});
</script>
