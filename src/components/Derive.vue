<template>
  <div class="hello pt-2">
    <div class="card shadow mb-3 d-print-none" v-show="familySeed.slice(0, 1) === 'X'">
      <div class="card-header"><b>Please enter your Exarpy PIN</b></div>
      <div class="card-body pr-0 row">
        <div class="row pb-2 col-6 pr-0 col-lg-3" v-for="i in Object.keys(secret)" v-bind:key="i">
          <div class="col-3 pr-0 text-right"><b class="text-primary pt-1 d-block">{{ Number(i)+1 }}</b></div>
          <div class="col-9 pr-0"><input :class="{ 'is-valid': String(secret[i]).trim().match(/^[0-9]{1,3}$/g), 'is-invalid': !String(secret[i]).trim().match(/^[0-9]{1,3}$/g) }" maxlength="3" v-model="secret[i]" type="text" class="form-control form-control-sm text-secondary" placeholder="XXX (Please enter)" /></div>
        </div>
      </div>
      <div v-if="valid" class="mt-0 pt-0 card-body text-center">
          <button @click="calc()" class="btn btn-lg btn-primary py-2 px-5">Next (convert secret)</button>
      </div>
    </div>
    <div class="text-center" v-show="familySeed.slice(0, 1) !== 'X'">
      <b>Account address (public)</b>
      <br />
      <code class="generated font-weight-bold px-2 text-primary d-inline-block mb-2" v-clipboard:copy="'Account: ' + account">{{ account }}</code>
      <br />
      <b>Account secret (for your eyes only)</b>
      <br />
      <small>
        <b>Family Seed</b> (Common secret format)
        <br />
        <code class="generated px-2 text-danger d-inline-block mb-2" v-clipboard:copy="'Family Seed Secret (keep safe!): ' + familySeed">
          {{ familySeed }}
        </code>
      </small>
    </div>
    <div class="mt-4 text-center d-print-none" v-show="familySeed.slice(0, 1) !== 'X'">
      <button @click="print()" class="font-weight-bold btn btn btn-dark">Print this (unsafe!) <small>- Better write down &amp; test</small></button>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueClipboard from 'vue-clipboard2'
// import XrplKeyPairs from 'ripple-keypairs'
import { generateSeed, deriveKeypair, deriveAddress } from 'ripple-keypairs'

Vue.use(VueClipboard)

export default {
  name: 'Derive',
  methods: {
    print () {
      window.print()
    },
    calc () {
      const account = generateSeed({
        entropy: this.secret.map(s => String(s).trim()),
        algorithm: 'ed25519'
      })
      this.familySeed = account
      const keypair = deriveKeypair(account)
      const address = deriveAddress(keypair.publicKey)
      this.account = address
    }
  },
  watch: {},
  data () {
    return {
      account: 'XXXXXXXXXXXXXXXXXXXXXXXXXXX',
      secret: ['', '', '', '', '', '', '', '', '', '', '', '', '', '', '', ''],
      familySeed: 'XXXXXXXXXXXXXXXXXXXXXXXXXXX'
    }
  },
  computed: {
    valid () {
      if (this.secret.filter(s => String(s).trim().match(/^[0-9]{1,3}$/g)).length === 16) {
        return true
      }
      return false
    }
  }
}
</script>

<style scoped lang="scss">
  input[type='text'] {
    font-weight: bold;
  }
  code {
    cursor: pointer;
    letter-spacing: .1em;
    font-size: 1.5em;
    &:active {
      color: white !important;
      background-color: rgba(0, 0, 0, .8);
      outline: 0;
      box-shadow: 0 0 0 3px rgba(0, 123, 255, .5);
      border-radius: 4px;
      z-index: 10;
      position: relative;
      &:before {
        content: 'Copied! (Unsafe!)';
        background-color: rgba(200, 0, 0, .9);
        padding: 1px 4px;
        border-radius: 4px;
        display: block;
        position: absolute;
        color: white;
        font-weight: bold;
        border: 1px solid rgba(0, 0, 0, .5);
        font-size: 12px;
        margin-top: -28px;
        margin-left: -10px;
        letter-spacing: normal;
      }
    }
  }
</style>
