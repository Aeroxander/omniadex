<template>
  <section class="container">
    <div class="home">
      <vs-divider color="primary">
        <h1>Marketplace</h1>
      </vs-divider>
      <vs-row 
        v-if="offers.records" 
        vs-align="center" 
        vs-type="flex" 
        vs-justify="space-around" 
        vs-w="12">
        <vs-col
          v-for="offer in offers.records"
          :key="offer"
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
            class="cardx">
            <div slot="header">
              <h3>{{ offer.metaData.title }}</h3>
            </div>
            <div slot="media">
              <img :src="offer.metaData.image">
            </div>
            <div>
              <span class="text-font">{{ offer.metaData.desc }}</span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  disabled 
                  color="primary" 
                  type="gradient">Ξ 5</vs-button>
                <vs-button
                  color="primary"
                  type="gradient"
                  @click="$router.push('/offer/' + offer.order.makerAddress)"
                >More details</vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
      </vs-row>
      <vs-prompt 
        :vs-active.sync="activePrompt" 
        @vs-accept="acceptAlert" 
        @vs-close="close">
        <div class="prompt">Current price Offer
          <vs-input 
            placeholder="Code" 
            vs-placeholder="Code"/>
        </div>
      </vs-prompt>
    </div>
  </section>
</template>
<script>
// import zeroExInstant from "https://instant.0x.org/instant.js";
import SideBar from '@/components/SideBar'

export default {
  name: 'Home',
  components: { SideBar },
  data: function() {
    return {
      activePrompt: false,
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
    acceptAlert() {
      //check if transaction is received, notify error if not
      this.$vs.notify({
        color: 'success',
        title: 'Accept Selected',
        text: 'Lorem ipsum dolor sit amet, consectetur'
      })
    },
    close() {
      this.$vs.notify({
        color: 'danger',
        title: 'Closed',
        text: 'You close a dialog!'
      })
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
.prompt {
  padding: 10px;
  padding-bottom: 0px;
}
.prompt .vs-input {
  width: 100%;
  margin-top: 10px;
}
</style>
