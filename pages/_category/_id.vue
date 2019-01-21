<template>
  <section class="container">
    <div class="offer">
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
              <h3>{{ token.name }}</h3>
            </div>
            <div slot="media">
              <img :src="'https://0xproject.com/images/token_icons/' + token.symbol + '.png'">
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
          <vs-divider color="primary">{{ params }}</vs-divider>
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
              :subtitle="order.signedOrder.makerAddress" 
              title="Owner ID"/>
            <vs-list-item title="Duration">
              <vs-chip>
                <vs-avatar
                  text-color="success"
                  badge="1"
                  badge-color="rgb(140, 23, 164)"
                  icon="access_time"
                />
                <countdown :time="order.signedOrder.expirationTimeSeconds">
                  <template
                    slot-scope="props"
                  >Ends in：{{ props.hours }} hours, {{ props.minutes }} minutes, {{ props.seconds }} seconds.</template>
                </countdown>
              </vs-chip>
            </vs-list-item>
            <vs-list-header title="Bids"/>
            <vs-list-item
              v-for="bid in openBids"
              :key="bid"
              :title="bid.price"
              :subtitle="'Bid on ' + bid.time"
            >
              <vs-chip 
                closable
                color="#24c1a0" 
                close-icon="close" 
                @click="remove(bid)">{{ bid.bidder }}</vs-chip>
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
              @click="offerBid(offerAmount)">Bid for Ξ {{ offerAmount }}</vs-button>
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
import Vue from 'vue'
import VueCountdown from '@chenfengyuan/vue-countdown'
Vue.component(VueCountdown.name, VueCountdown)

export default {
  /*

  asyncData({ params, $axios }) {
    return $axios
    do the awiat stuff here
      .$get('https://api.radarrelay.com/v2/markets/zrx-weth/book')
      .then(res => {
        return {
          order:
            res.bids[res.bids.findIndex(item => item.orderHash === params.id)],
          params: params.id
        }
      })
  },
  */
  async asyncData({ $axios, params }) {
    const orders = await $axios.$get(
      'https://api.radarrelay.com/v2/markets/' + params.category + '/book'
    )
    const tokens = await $axios.$get('https://api.radarrelay.com/v2/tokens')
    var order =
      orders.bids[orders.bids.findIndex(bid => bid.orderHash == params.id)]
    const token =
      tokens[tokens.findIndex(token => token.address == order.baseTokenAddress)]
    console.log(typeof order.signedOrder.expirationTimeSeconds)
    console.log(typeof order.signedOrder.expirationTimeSeconds)
    console.log(order.signedOrder.expirationTimeSeconds)
    return {
      order: order,
      params: params.id,
      token: token
    }
  },
  async validate({ $axios, params }) {
    console.log(params)
    const orders = await $axios.$get(
      'https://api.radarrelay.com/v2/markets/' + params.category + '/book'
    )
    const order =
      orders.bids[orders.bids.findIndex(bid => bid.orderHash == params.id)]
    return order
  },
  name: 'Offer',
  data: function() {
    return {
      iteration: {},
      recurring: false,
      period: ['Daily', 'Weekly', 'Monthly'],
      offerAmount: 0,
      activePrompt: false,
      bid: 'false',
      openBids: []
    }
  },
  methods: {
    openLoading() {
      this.$vs.loading()
      setTimeout(() => {
        this.$vs.loading.close()
      }, 2000)
    },
    offerBid(offerAmount) {
      /*this.openBids += {
        id: 1,
        bidder: 'YOU!!!',
        amount: offerAmount,
        time: Date.now()
      }*/
      var now = new Date().toLocaleString()
      if (this.openBids.findIndex(openBid => openBid == 'youraddress') === -1) {
        this.openBids.splice(
          this.openBids.findIndex(openBid => openBid == 'youraddress'),
          1,
          {
            bidder: 'youraddress',
            price: offerAmount,
            time: now
          }
        )
        this.openBids.filter(openBid => openBid.bidder == 'youraddress')
      } else {
        this.openBids.push({
          bidder: 'youraddress',
          price: offerAmount,
          time: now
        })
      }
    },
    remove(bid) {
      this.openBids.splice(
        this.openBids.findIndex(openBid => openBid == bid),
        1
      )
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
