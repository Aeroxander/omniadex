<template>
  <section class="container">
    <!-- <div class="parentx-static">
      <vs-sidebar
        v-if="pairs"
        v-model="open"
        static-position
        default-index="1"
        color="primary"
        class="sidebarx"
        spacer
      >
        <div 
          slot="header" 
          class="header-sidebar">
          <vs-avatar 
            size="70px" 
            src="https://randomuser.me/api/portraits/men/85.jpg"/>

          <h4>0x55cA8aB54574D1Dcf1cF44Dbc7B69Bb6e6b2f075
            <vs-button 
              color="primary" 
              icon="more_horiz" 
              type="flat"/>
          </h4>
        </div>

        <vs-sidebar-item 
          index="1" 
          icon="question_answer">Dashboard</vs-sidebar-item>

        <vs-sidebar-group 
          :open="true" 
          title="Categories">
          <vs-sidebar-item 
            index="2" 
            icon="store">NFT's</vs-sidebar-item>
          <vs-sidebar-group title="EC-20">
            <vs-sidebar-group title="ETH">
              <vs-sidebar-item
                v-for="pair in pairs.filter(pair => !pair.id.includes('DAI'))"
                :key="pair.score"
                :index="'2.'+pair.index"
              >{{ pair.displayName }}</vs-sidebar-item>
            </vs-sidebar-group>
            <vs-sidebar-group title="DAI">
              <vs-sidebar-item
                v-for="pair in pairs.filter(pair => pair.id.includes('DAI'))"
                :key="pair.score"
                :index="'2.'+pair.index"
              >{{ pair.displayName }}</vs-sidebar-item>
            </vs-sidebar-group>
          </vs-sidebar-group>
          <vs-sidebar-item 
            index="2.3" 
            icon="style">Misc</vs-sidebar-item>
        </vs-sidebar-group>

        <vs-divider 
          icon="person" 
          position="left">User</vs-divider>

        <vs-sidebar-item 
          index="3" 
          icon="verified_user">Configurations</vs-sidebar-item>
        <vs-sidebar-item 
          index="4" 
          icon="account_box">Profile</vs-sidebar-item>
        <vs-sidebar-item index="5">Card</vs-sidebar-item>

        <div 
          slot="footer" 
          class="footer-sidebar">
          <vs-button 
            icon="reply" 
            color="danger" 
            type="flat">Log Out</vs-button>
          <vs-button 
            icon="settings" 
            color="primary" 
            type="flat"/>
        </div>
      </vs-sidebar>
    </div>-->
    <vs-divider color="primary">
      <h1>Marketplace</h1>
    </vs-divider>
    <vs-tabs vs-alignment="center">
      <vs-tab 
        vs-label="WETH" 
        vs-icon="pets" 
        @click="colorx = '#8B0000'">
        <vs-tabs vs-position="left">
          <vs-tab
            v-for="pair in pairs.filter(pair => !pair.id.includes('DAI'))"
            :key="pair.score"
            :vs-label="pair.displayName"
            @click="getOrders(pair.id)"
          >
            <vs-row 
              vs-align="center" 
              vs-type="flex" 
              vs-justify="space-around" 
              vs-w="12">
              <vs-col
                v-for="order in orders.bids"
                :key="order.price"
                type="flex"
                vs-justify="center"
                vs-align="center"
                vs-lg="2.7"
                vs-sm="8"
                vs-xs="12"
                vs-w="2.5"
              >
                <vs-card
                  actionable
                  class="cardx"
                >
                  <div slot="header">
                    <h3>{{ order.type }}</h3>
                  </div>
                  <div slot="media">
                    <img :src="'https://0xproject.com/images/token_icons/' + pair.id.split('-')[0] + '.png'">
                  </div>
                  <div>
                    <span class="text-font"> {{ pair.name }} </span>
                  </div>
                  <vs-button 
                    color="#0066CC"
                    type="line"
                    disabled>Ξ {{ order.price }}</vs-button>
                  <div slot="footer">
                    <vs-row vs-justify="flex-end">
                      <vs-button
                        color="success"
                        type="gradient"
                        icon="details"
                        @click="$router.push('/offers/' + order.orderHash)"
                      >Buy</vs-button>
                    </vs-row>
                  </div>
                </vs-card>
              </vs-col>
            </vs-row>
          </vs-tab>
        </vs-tabs>
      </vs-tab>
      <vs-tab 
        vs-label="DAI" 
        vs-icon="account_balance" 
        @click="colorx = '#FFA500'">
        <vs-tabs vs-position="left">
          <vs-tab
            v-for="pair in pairs.filter(pair => pair.id.includes('DAI'))"
            :key="pair.score"
            :vs-label="pair.displayName"
            @click="getOrders(pair.id)"
          >
            <vs-row 
              vs-align="center" 
              vs-type="flex" 
              vs-justify="space-around" 
              vs-w="12">
              <vs-col
                v-for="order in orders.bids"
                :key="order.price"
                type="flex"
                vs-justify="center"
                vs-align="center"
                vs-lg="2.7"
                vs-sm="8"
                vs-xs="12"
                vs-w="2.5"
              >
                <vs-card
                  actionable
                  class="cardx"
                >
                  <div slot="header">
                    <h3>{{ order.type }}</h3>
                  </div>
                  <div slot="media">
                    <img :src="'https://0xproject.com/images/token_icons/' + pair.id.split('-')[0] + '.png'">
                  </div>
                  <div>
                    <span class="text-font"> {{ pair.name }} </span>
                  </div>
                  <vs-button 
                    color="primary" 
                    type="line" >Ξ {{ order.price }}</vs-button>
                  <div slot="footer">
                    <vs-row vs-justify="flex-end">
                      <vs-button
                        color="success"
                        type="gradient"
                        icon="details"
                        @click="$router.push('/offers/' + order.orderHash)"
                      >Buy</vs-button>
                    </vs-row>
                  </div>
                </vs-card>
              </vs-col>
            </vs-row>
          </vs-tab>
        </vs-tabs>
      </vs-tab>
    </vs-tabs>
  </section>
</template>
<script>
import { HttpClient, OrderbookRequest, OrderConfigRequest } from '@0x/connect'
import TOKENS from '@/pages/tokens.js'

export default {
  async asyncData({ $axios }) {
    const bookRes = await $axios.$get(
      'https://api.radarrelay.com/v2/markets/zrx-weth/book'
    )
    const marketRes = await $axios.$get('https://api.radarrelay.com/v2/markets')
    return { orders: bookRes, pairs: marketRes }
  },
  name: 'Index',
  components: {},
  data: function() {
    return {
      open: false,
      market: null,
      offers: {
        total: 1,
        page: 0,
        perPage: 20,
        records: [
          {
            metaData: {
              title: 'First offer',
              desc:
                'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
              image: 'https://0xproject.com/images/token_icons/WETH.png'
            },
            order: {
              signature:
                '0x1b4b01392fd7afd2f9901ead9e99885a62f4d005fe735e0a19cf8f37c9aa8218895f7072f9ab92729ac35f2d478542ffdea62ad78d488e5ff0fbeef667d758f9e303',
              senderAddress: '0x0000000000000000000000000000000000000000',
              makerAddress: '0x4ae231df684d81de0e85582b0a99154445a3ea2f',
              takerAddress: '0x0000000000000000000000000000000000000000',
              makerFee: '0',
              takerFee: '0',
              makerAssetAmount: '1',
              takerAssetAmount: '100000000000000000',
              makerAssetData:
                '0x0257179200000000000000000000000061af2aa3f2ee1b35699d8986dab3f421b671269a0000000000000000000000000000000000000000000000000000000000000001',
              takerAssetData:
                '0xf47261b0000000000000000000000000d0a1e359811322d97991e03f863a0c30c2cf029c',
              salt:
                '54482334814118760009374173240692376400580602373172445140036590366157243598889',
              exchangeAddress: '0x35dd2932454449b14cee11a94d3674a936d5d7b2',
              feeRecipientAddress: '0x0000000000000000000000000000000000000000',
              expirationTimeSeconds: '1547398871'
            }
          }
        ]
      }
    }
  },
  methods: {
    async getOrderbook() {
      const relayerApiUrl = 'https://sra.bamboorelay.com/0x/v2/'
      const httpClient = new HttpClient(relayerApiUrl)
      const orderbookRequest = {
        baseAssetData:
          '0xf47261b0000000000000000000000000e41d2489571d322189246dafa5ebde1f4699f498',
        quoteAssetData:
          '0x02571792000000000000000000000000371b13d97f4bf77d724e78c16b7dc74099f40e840000000000000000000000000000000000000000000000000000000000000063'
      }
      const response = await httpClient.getOrderbookAsync(orderbookRequest, {
        networkId: 1
      })
      if (response.asks.total === 0) {
        throw new Error('No orders found on the SRA Endpoint')
      } else {
        console.log(response)
      }
    },
    async getOrders(id) {
      const bookRes = await this.$axios.$get(
        'https://api.radarrelay.com/v2/markets/' + id + '/book'
      )
      this.orders = bookRes
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Nunito:300,400,600');

h1,
h3 p,
.text-font {
  font-family: 'Nunito', Helvetica, sans-serif;
}

h3 {
  font-size: large;
}

.subTitle {
  text-shadow: 0 5px 10px rgba(0, 0, 0, 0.33);
  position: relative;
}

.parentx-static {
  overflow: hidden;
  height: 500px;
  position: relative;
}
</style>
