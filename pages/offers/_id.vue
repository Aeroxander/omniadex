<template>
  <section class="container">
    <div class="offer">
      <vs-breadcrumb align="left">
        <li>
          <router-link to="/marketplace">Marketplace</router-link>
          <span class="vs-breadcrum--separator">/</span>
        </li>
        <li 
          aria-current="page" 
          class="active">Offer</li>
      </vs-breadcrumb>
      <vs-row 
        vs-align="center" 
        vs-type="flex" 
        vs-justify="center" 
        vs-w="12">
        <vs-col
          type="flex"
          vs-justify="center"
          vs-align="center"
          vs-lg="4"
          vs-sm="8"
          vs-xs="12"
          vs-w="4"
        >
          <vs-card>
            <div slot="header">
              <h3>Microsoft Office License</h3>
            </div>
            <div slot="media">
              <img src="https://0x.org/images/token_icons/ZRX.png">
            </div>
            <div>
              <span>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  type="gradient" 
                  color="danger" 
                  icon="favorite"/>
                <vs-button 
                  color="success" 
                  icon="share" 
                  @click="activePrompt = true"/>
                <vs-button 
                  color="primary" 
                  icon="turned_in_not"/>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
        <vs-col
          type="flex"
          vs-justify="flex-start"
          vs-align="flex-start"
          vs-lg="4"
          vs-sm="8"
          vs-xs="12"
          vs-w="4"
          vs-offset="1"
        >
          <vs-divider color="primary">{{ params.id }}</vs-divider>
          <vs-alert
            title="Software License"
            active="true"
            color="dark"
          >Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat</vs-alert>
          <vs-list>
            <vs-list-header title="Specifications"/>
            <vs-list-item 
              title="Token type" 
              subtitle="This token is an NFT"/>
            <vs-list-item 
              title="Lifetime" 
              subtitle="This NFT has a lifetime of 30 days"/>
            <vs-list-item 
              :subtitle="params.id" 
              title="Owner ID"/>
            <vs-list-item 
              title="Duration" 
              subtitle>
              <vs-chip>
                <vs-avatar
                  text-color="success"
                  badge="1"
                  badge-color="rgb(140, 23, 164)"
                  icon="access_time"
                />Ends in: 1d
              </vs-chip>
            </vs-list-item>
          </vs-list>
          <vs-divider color="primary"/>
          <vs-radio 
            v-model="bid" 
            vs-value="false">Buy</vs-radio>
          <vs-radio 
            v-model="bid" 
            vs-value="true">Bid</vs-radio>
          <span v-if="bid == 'false'">
            <br>
            <vs-button 
              type="filled" 
              @click="openLoading">
              Buy now for
              Ξ {{ order.price }}
            </vs-button>
          </span>
          <span v-else>
            <vs-input-number 
              v-model="offerAmount" 
              min="0"/>
            <vs-button 
              type="filled" 
              @click="openLoading">Bid for Ξ {{ offerAmount }}</vs-button>
          </span>
          <!--
          <li>
            <vs-radio v-model="radios1" vs-value="one-time">One-time</vs-radio>
          </li>
          <li>
            <vs-radio v-model="radios1" vs-value="daily">Daily</vs-radio>
          </li>
          -->
        </vs-col>
      </vs-row>
      <!--
    <vs-dropdown>
      <vs-button class="btn-drop" type="filled" icon="expand_more"></vs-button>
       <a href="#">Hola mundo</a> 
      <vs-dropdown-menu>
        <vs-dropdown-item
          v-on:click="test"
          v-for="freq in frequency"
          v-bind:key="freq"
          >{{ freq.text }}</vs-dropdown-item
        >
      </vs-dropdown-menu>
      </vs-dropdown>-->
      <vs-popup 
        :active.sync="activePrompt" 
        class="holamundo" 
        title="Lorem ipsum dolor sit amet"/>
    </div>
  </section>
</template>
<script>
import {
  assetDataUtils,
  BigNumber,
  ContractWrappers,
  generatePseudoRandomSalt,
  Order,
  orderHashUtils,
  signatureUtils,
  SignerType
} from '0x.js'

export default {
  /*
  validate({ params }) {
    return order.orderHash === params.id
  },
  */
  asyncData({ params, $axios }) {
    return $axios
      .$get('https://api.radarrelay.com/v2/markets/zrx-weth/book')
      .then(res => {
        return {
          order:
            res.bids[res.bids.findIndex(item => item.orderHash === params.id)],
          params: params.id
        }
      })
  },
  name: 'Offer',
  components: {},
  data: function() {
    return {
      iteration: {},
      recurring: false,
      period: ['Daily', 'Weekly', 'Monthly'],
      offerAmount: order.price,
      activePrompt: false,
      bid: 'false'
    }
  },
  methods: {
    openLoading() {
      this.$vs.loading()
      setTimeout(() => {
        this.$vs.loading.close()
      }, 2000)
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Nunito:300,400,600');

h1,
p,
.text-font {
  font-family: 'Nunito', Helvetica, sans-serif;
}

.subTitle {
  text-shadow: 0 5px 10px rgba(0, 0, 0, 0.33);
  position: relative;
}
</style>
