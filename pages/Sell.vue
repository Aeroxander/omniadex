<template>
  <section class="container">
    <div class="sell">
      <br>
      <h1>Sell NFT</h1>
      <br>
      <vs-row vs-justify="center">
        <vs-select
          v-model="select"
          placeholder="Select"
          class="selectExample"
          label="Select token"
        >
          <!--
            <vs-select-item
              v-for="token in tokens.filter(token => token.zeroex_official === 1)"
              :key="token.name"
              :value="token.symbol"
              :text="token.name"
              :danger="true"
            />
          -->
          <vs-select-item
            v-for="token in walletNFT.assets"
            :key="token.name"
            :value="token.token_id"
            :text="token.name"
            :danger="true"
          />
        </vs-select>
      </vs-row>
      <br>
      <vs-row vs-justify="center">
        <vs-input 
          v-model="tokenName"
          placeholder="Name"/>
      </vs-row>
      <br>
      <vs-row 
        vs-justify="center">
        <vs-col vs-w="3">
          <vs-textarea 
            v-model="tokenDescription" 
            label="Description" />
        </vs-col>
      </vs-row>
      <vs-row vs-justify="center">
        <picture-input 
          ref="pictureInput"
          :custom-strings="{
            upload: '<h1>Bummer!</h1>',
            drag: 'Drag or upload an image here 😺'
          }" 
          width="300" 
          height="300" 
          margin="16" 
          accept="image/jpeg,image/png" 
          size="10"
          @change="onChange"/>
      </vs-row>
      <br>
      <vs-row vs-justify="center">
        <vs-input-number 
          v-model="price"
          min="0"/>
      </vs-row>
      <vs-row vs-justify="center">
        <vs-button 
          type="filled" 
          @click="openLoading">Sell for Ξ {{ price }}</vs-button>
      </vs-row>
      <br>
    </div>
  </section>
</template>
<script>
import PictureInput from '@/components/PictureInput/PictureInput'

export default {
  async asyncData({ $axios }) {
    const tokens = await $axios.$get('https://api.radarrelay.com/v2/tokens')
    const walletNFT = await $axios.$get(
      'https://api.opensea.io/api/v1/assets?owner=0x0239769a1adf4def9f07da824b80b9c4fcb59593&order_by=current_price&order_direction=asc'
    )
    console.log(walletNFT.assets)
    return { tokens, walletNFT }
  },
  name: 'Sell',
  components: { PictureInput },
  data: function() {
    return {
      tokenName: '',
      tokenDescription: '',
      select: 0,
      price: 1
    }
  },
  methods: {
    openLoading() {
      this.$vs.loading()
      setTimeout(() => {
        this.$vs.loading.close()
      }, 2000)
    }
    /*,
    step(type) {
      var number = this.price
      if (number == 0) return 1

      var numberDup = number
      number = number.toString()
      console.log(this.price.length)
      console.log('numberString: ' + number.slice(0, -1))
      console.log(number[number.length - 1] + 1)
      return
      parseInt(numberDup) -
        (number.slice(0, -1) + number[number.length - 1] + 1)
    }
    */
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Nunito:300,400,600');

h1,
p,
.text-font {
  font-family: 'Nunito', Helvetica, sans-serif;
  text-align: center;
}

.subTitle {
  text-shadow: 0 5px 10px rgba(0, 0, 0, 0.33);
  position: relative;
}

.sell {
  background: #fff;
  border-radius: 6px;
  -webkit-box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.5);
  box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.5);
  margin-bottom: 15px;
  overflow: hidden;
  margin-top: 0;
  max-width: 800px;
  margin: 0 auto;
  text-align: initial;
}
</style>
