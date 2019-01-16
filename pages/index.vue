<template>
  <section class="container">
    <div class="home">
      <vs-divider color="primary">
        <h1>Marketplace</h1>
      </vs-divider>
      <vs-row 
        v-if="offers" 
        vs-align="center" 
        vs-type="flex" 
        vs-justify="space-around" 
        vs-w="12">
        <vs-col
          v-for="offer in offers"
          :key="offer.name"
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
              <h3>{{ offer.title }}</h3>
            </div>
            <div slot="media">
              <img :src="offer.image">
            </div>
            <div>
              <span class="text-font">{{ offer.desc }}</span>
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
                  @click="$router.push('/offer/0x423hlds9f02fjpdas8')"
                >More details</vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
      </vs-row>
      <vs-prompt
        :vs-active.sync="activePrompt"
        @vs-accept="acceptAlert"
        @vs-close="close"
      >
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
        1: {
          title: 'First offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        2: {
          title: 'Second offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        3: {
          title: 'Third offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        4: {
          title: 'Fourth offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        5: {
          title: 'Fifth offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        6: {
          title: 'Sixth offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        7: {
          title: 'Seventh offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        8: {
          title: 'Eigth offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        9: {
          title: 'Ninth offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        },
        10: {
          title: 'Tenth offer',
          desc:
            'This is the description of the offer, this explains what the user wants in return but that could also be done in pictures later on.',
          image: 'https://0xproject.com/images/token_icons/WETH.png'
        }
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
