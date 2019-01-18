<template>
  <section class="container">
    <div class="sell">
      <vs-row 
        vs-align="center" 
        vs-type="flex" 
        vs-offset="5" 
        vs-justify="center" 
        vs-w="1">
        <vs-select
          v-model="select"
          placeholder="Select"
          class="selectExample"
          label="Select token"
        >
          <vs-select-item
            v-for="token in tokens.filter(token => token.zeroex_official === 1)"
            :key="token.name"
            :value="token.symbol"
            :text="token.name"
            :danger="true"
          />
        </vs-select>
        Price
        <vs-input-number 
          v-model="price" 
          min="0"/>
        Amount
        <vs-input-number 
          v-model="offerAmount" 
          min="0"/>
        <vs-button 
          type="filled" 
          @click="openLoading">Sell for Ξ {{ offerAmount }}</vs-button>
      </vs-row>
    </div>
  </section>
</template>
<script>
export default {
  async asyncData({ $axios }) {
    const tokens = await $axios.$get('https://api.radarrelay.com/v2/tokens')
    return { tokens }
  },
  name: 'Sell',
  components: {},
  data: function() {
    return {
      tokensExample: [
        { text: 'ETH', value: 0 },
        { text: 'ZRX', value: 1 },
        { text: 'REQ', value: 2 }
      ],
      select: 0,
      offerAmount: 0,
      price: 0
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
