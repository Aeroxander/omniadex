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
          v-model="select1"
          placeholder="Select"
          class="selectExample"
          label="Select token"
        >
          <vs-select-item
            v-for="item in tokensExample"
            :key="item.index"
            :value="item.value"
            :text="item.text"
            :danger="true"
          />
        </vs-select>

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
      offerAmount: 5
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
