<template>
  <section class="container">
    <div class="fundraise">
      <br>
      <h1>Fundraise</h1>
      <br>
      <vs-row vs-justify="center">
        <vs-col 
          type="flex" 
          vs-justify="center" 
          vs-align="center" 
          vs-w="10">
          <vs-card>
            <div slot="header">
              <h3>
                <vs-input 
                  v-model="tokenName"
                  placeholder="Tier Title"/>
              </h3>
            </div>
            <div>
              <vs-textarea 
                v-model="tokenDescription"
                label="Reward Description"/>
              <vs-checkbox v-model="limit.active">Limit donators</vs-checkbox>
              <!-- eslint-disable -->
              <vs-input-number 
                v-if="limit.active"
                v-model="limit.amount"
                iconInc="person_add"
                iconDec="person_add_disabled"
                min="1"/>
              <!-- eslint-enable -->
              <vs-input-number 
                v-model="price"
                min="1"/> <!-- :min="Math.max(...sortedTiers.map(tier => tier.price))"/>-->
              <vs-row vs-justify="center">
                <vs-button 
                  type="filled"
                  color="#00b8ff"
                  disabled
                  @click="openLoading">Tier Price Ξ {{ price }}</vs-button>
              </vs-row>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  color="success" 
                  icon="add_circle"
                  @click="addTier"/>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
      </vs-row>
      <br>
    </div>
    <transition
      :css="false"
      @before-enter="beforeEnter"
      @enter="enter"
      @leave="leave">
      <vs-divider 
        v-if="tiers.length != 0"
        color="primary"
        style="margin: 35px 0;">
        <h1>Tiers</h1>
      </vs-divider>
    </transition>
    <vs-row 
      vs-type="flex"
      vs-justify="space-around">
      <transition-group 
        name="list-complete" 
        tag="span"
        style="width: 100%; display: contents;">
        <vs-col
          v-for="(tier, index) in tiers" 
          :key="tier"
          type="flex"
          vs-justify="center"
          vs-align="center"
          vs-lg="2.7"
          vs-sm="5"
          vs-xs="10"
          vs-w="2.5"
          class="list-complete-item">
          <vs-card>
            <div slot="header">
              <span v-if="tier.activeEdit === false">
                <h3>
                  {{ tier.name }}
                </h3>
              </span>
              <vs-input 
                v-else
                v-model="tier.name"
                placeholder="Tier Title"/>
            </div>
            <div>
              <span v-if="tier.activeEdit === false">
                <vs-list>
                  <vs-list-item 
                    :subtitle="tier.description" 
                    icon="short_text" 
                    title="Description"/>
                  <vs-list-item 
                    v-if="tier.limit.active" 
                    :subtitle="tier.limit.amount" 
                    icon="person" 
                    title="Donator Limit"/>
                  <vs-list-item 
                    :subtitle="'Ξ ' + tier.price" 
                    icon="attach_money" 
                    title="Price"/>
                </vs-list>
              </span>
              <span v-else>
                <vs-textarea 
                  v-model="tier.description" 
                  label="Description"/>
                <vs-checkbox v-model="tier.limit.active">Limit donators</vs-checkbox>
                <!-- eslint-disable -->
                <vs-input-number 
                  v-if="tier.limit.active"
                  v-model="tier.limit.amount"
                  iconInc="person_add"
                  iconDec="person_add_disabled"
                  min="1"/>
                <!-- eslint-enable -->
                <vs-input-number 
                  ref="input"
                  v-model="tier.price"
                  min="1"/><!-- :min="sortedTiers[index - 1] ? sortedTiers[index - 1].price : 0"/>-->
                <vs-row vs-justify="center">
                  <vs-button 
                    type="filled" 
                    color="#00b8ff"
                    disabled
                    @click="openLoading">Tier Price Ξ {{ tier.price }}</vs-button>
                </vs-row>
              </span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <span v-if="tier.activeEdit === false">
                  <vs-button
                    type="gradient" 
                    color="success" 
                    icon="edit"
                    @click="enableTierEdit(index)"/>
                  <vs-button 
                    type="gradient" 
                    color="danger" 
                    icon="remove_circle"
                    @click="removeTier(index)"/>
                </span>
                <vs-button
                  v-else
                  color="success" 
                  icon="check"
                  @click="editTier(tier, index)"/>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
      </transition-group>
    </vs-row>
  </section>
</template>
<script>
import Vue from 'vue'

let Velocity

if (process.browser) {
  Velocity = require('velocity-animate')
}

export default {
  async asyncData({ $axios }) {
    const tokens = await $axios.$get('https://api.radarrelay.com/v2/tokens')
    const walletNFT = await $axios.$get(
      'https://api.opensea.io/api/v1/assets?owner=0x0239769a1adf4def9f07da824b80b9c4fcb59593&order_by=current_price&order_direction=asc'
    )
    console.log(walletNFT.assets)
    return { tokens, walletNFT }
  },
  name: 'Fundraise',
  data: function() {
    return {
      tiers: [],
      tokenName: '',
      tokenDescription: '',
      select: 0,
      price: 1,
      limit: { active: false, amount: 1 }
    }
  },
  methods: {
    openLoading() {
      this.$vs.loading()
      setTimeout(() => {
        this.$vs.loading.close()
      }, 2000)
    },
    addTier() {
      if (this.tokenName != '') {
        this.tiers.push({
          name: this.tokenName,
          description: this.tokenDescription,
          price: this.price,
          limit: this.limit,
          activeEdit: false
        })
        this.tiers = this.sortedTiers(this.tiers)
        this.tokenName = ''
        this.tokenDescription = ''
        this.minPrice = this.price
        this.limit = { active: false, amount: 1 }
      } else {
        this.$vs.notify({
          text: 'Please Fill In Title',
          color: 'danger'
        })
      }
    },
    removeTier(index) {
      this.tiers.splice(index, 1)
    },
    enableTierEdit(index) {
      this.tiers[index].activeEdit = true
    },
    editTier(tier, index) {
      if (tier.name != '') {
        this.tiers[index].activeEdit = false
        this.tiers[index] = tier
        this.tiers = this.sortedTiers(this.tiers)
      } else {
        this.$vs.notify({
          text: 'Please Fill In Title',
          color: 'danger'
        })
      }
    },
    sortedTiers(tiers) {
      return tiers.sort((a, b) => a.price - b.price)
    },
    beforeEnter: function(el) {
      el.style.opacity = 0
    },
    enter: function(el, done) {
      var vm = this
      Velocity(
        el,
        { opacity: 1 },
        {
          duration: 1000,
          complete: function() {
            done()
          }
        }
      )
    },
    leave: function(el, done) {
      var vm = this
      Velocity(
        el,
        { opacity: 0 },
        {
          duration: 1000,
          complete: function() {
            done()
          }
        }
      )
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Nunito:300,400,600');

h4,
p,
.text-font {
  font-family: 'Nunito', Helvetica, sans-serif;
  text-align: initial;
}

h1 {
  font-family: 'Nunito', Helvetica, sans-serif;
  text-align: center;
}

.p {
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen,
    Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  -webkit-font-smoothing: antialiased;
}

* {
  font-family: OpenSans, arial;
}

.subTitle {
  text-shadow: 0 5px 10px rgba(0, 0, 0, 0.33);
  position: relative;
}

.fundraise {
  background: #fff;
  border-radius: 6px;
  -webkit-box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.5);
  box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.5);
  margin-bottom: 15px;
  overflow: hidden;
  margin-top: 0;
  max-width: 400px;
  margin: 0 auto;
  text-align: left !important;
}

.vs-checkbox--icon {
  font-size: initial !important;
}

.vs-list--item {
  text-align: left !important;
}

.list-complete-item {
  transition: all 1s;
  display: inline-block;
}

.list-complete-enter, .list-complete-leave-to
/* .list-complete-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
.list-complete-leave-active {
  position: absolute;
}
</style>
