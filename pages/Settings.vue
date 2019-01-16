<template>
  <section class="container">
    <div class="settings">
      <vs-avatar 
        size="70px" 
        src="https://randomuser.me/api/portraits/men/85.jpg"/>0x55cA8aB54574D1Dcf1cF44Dbc7B69Bb6e6b2f075
      <vs-tabs>
        <vs-tab vs-label="My Wallet">
          <div>My wallet</div>
        </vs-tab>
        <vs-tab vs-label="Open Orders">
          <div>Open orders</div>
        </vs-tab>
        <vs-tab vs-label="Trade History">
          <div>Trade History</div>
        </vs-tab>
        <vs-tab 
          disabled 
          vs-label="Disabled">
          <div>Offers</div>
        </vs-tab>
      </vs-tabs>
    </div>
  </section>
</template>
<script>
export default {
  name: 'Settings',
  components: {},
  data: function() {
    return {}
  },
  methods: {}
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
