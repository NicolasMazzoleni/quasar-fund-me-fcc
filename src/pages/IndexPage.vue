<template>
  <q-page class="row items-center justify-evenly">
    <div class="q-pa-md q-gutter-sm">
      <q-btn @click="connect" color="secondary" label="Connect Wallet" />
      <q-btn @click="getBalance" color="accent" label="Contract Balance" />
      <q-btn @click="withdraw" color="negative" label="Withdraw" />
      <br /><br />
      <q-form class="q-gutter-md">
        <q-input filled type="number" v-model="amount" label="Amount" />

        <div>
          <q-btn label="Fund Contract" @click="fund" color="primary" />
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
import { defineComponent, ref } from 'vue'
import { useQuasar } from 'quasar'
import { ethers } from 'ethers'
import { fundMeAbi, fundMeContractAddress } from '../helpers/hardhatConfig'

export default defineComponent({
  name: 'IndexPage',
  setup() {
    const amount = ref(null)
    const metamaskLoader = ref(false)
    const $q = useQuasar()

    return {
      amount,
      metamaskLoader,

      async connect() {
        const { ethereum } = window
        if (ethereum !== undefined) {
          try {
            metamaskLoader.value = true
            const response = await ethereum.request({
              method: 'eth_requestAccounts',
            })

            if (response.length) {
              metamaskLoader.value = false
              $q.notify({
                message: 'Wallet connected !',
                icon: 'announcement',
                color: 'green',
              })
            }
          } catch (e) {
            metamaskLoader.value = false
            $q.notify({
              message: e.message,
              icon: 'warning',
              color: 'red',
            })
          }
        }

        if (ethereum === undefined) {
          $q.notify({
            message: 'Please install Metamask.',
            icon: 'warning',
            color: 'primary',
          })
        }
      },

      async fund() {
        try {
          if (!amount.value) {
            $q.notify({
              message: 'Select an amount of ETH.',
              icon: 'warning',
              color: 'primary',
            })

            return
          }

          if (amount.value) {
            const provider = new ethers.providers.Web3Provider(window.ethereum)
            const signer = provider.getSigner()
            const contract = new ethers.Contract(
              fundMeContractAddress,
              fundMeAbi,
              signer
            )

            const transactionResponse = await contract.fund({
              value: ethers.utils.parseEther(amount.value),
            })

            provider.once(transactionResponse.hash, () => {
              $q.notify({
                message: 'Contract funded !',
                icon: 'warning',
                color: 'primary',
              })
            })
          }
        } catch (e) {
          $q.notify({
            message: e.message,
            icon: 'warning',
            color: 'red',
          })
        }
      },

      async getBalance() {
        const { ethereum } = window
        if (ethereum !== undefined) {
          try {
            const provider = new ethers.providers.Web3Provider(ethereum)
            const balance = await provider.getBalance(fundMeContractAddress)
            $q.notify({
              message:
                'The current contract balance is ' +
                ethers.utils.formatEther(balance),
              icon: 'warning',
              color: 'primary',
            })
          } catch (e) {
            console.log('e ', e)
          }
        }
      },

      async withdraw() {
        const { ethereum } = window
        if (ethereum !== undefined) {
          try {
            const provider = new ethers.providers.Web3Provider(ethereum)
            const signer = provider.getSigner()
            const contract = new ethers.Contract(
              fundMeContractAddress,
              fundMeAbi,
              signer
            )

            const transactionResponse = await contract.withdraw()
            provider.once(transactionResponse.hash, () => {
              $q.notify({
                message: 'Withdraw completed.',
                icon: 'warning',
                color: 'primary',
              })
            })
          } catch (e) {
            console.log('e ', e)
          }
        }
      },
    }
  },
})
</script>
