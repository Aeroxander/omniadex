<template>
  <section class="container">
    <vs-divider color="success">
      <h1>NFT Categories</h1>
    </vs-divider>
    <vs-row 
      vs-align="center" 
      vs-type="flex" 
      vs-justify="space-around" 
      vs-w="12">
      <vs-col 
        type="flex" 
        vs-justify="center" 
        vs-align="center" 
        vs-w="3">
        <vs-card 
          actionable 
          class="cardx">
          <div slot="header">
            <h3>Fundraising</h3>
          </div>
          <div slot="media">
            <vs-icon 
              size="300px"
              icon="business"
            />
          </div>
          <div>
            <span class="text-font">Setup a donation up for your company, cause or creation and give NFT rewards back.</span>
          </div>
          <div slot="footer">
            <vs-row vs-justify="flex-end">
              <vs-button 
                color="rgb(11, 189, 135)" 
                type="filled">Setup Donation</vs-button>
            </vs-row>
          </div>
        </vs-card>
      </vs-col>
      <vs-col 
        type="flex" 
        vs-justify="center" 
        vs-align="center" 
        vs-w="3">
        <vs-card 
          actionable 
          class="cardx">
          <div slot="header">
            <h3>Ticket or License</h3>
          </div>
          <div slot="media">
            <vs-icon 
              size="300px"
              icon="confirmation_number"
            />
          </div>
          <div>
            <span class="text-font">Sell NFT tickets or licenses and validate them with our Javascript library</span>
          </div>
          <div slot="footer">
            <vs-row vs-justify="flex-end">
              <vs-button 
                color="rgb(11, 189, 135)"
                type="filled">Setup Ticket or License</vs-button>
            </vs-row>
          </div>
        </vs-card>
      </vs-col>
      <vs-col 
        type="flex" 
        vs-justify="center" 
        vs-align="center"
        vs-w="3">
        <vs-card 
          actionable 
          class="cardx">
          <div slot="header">
            <h3>Sell your own NFT</h3>
          </div>
          <div slot="media">
            <vs-icon 
              size="300px"
              icon="local_atm"
            />
          </div>
          <div>
            <span class="text-font">Sell an already existing NFT</span>
          </div>
          <div slot="footer">
            <nuxt-link to="/sell">
              <vs-row vs-justify="flex-end">
                <vs-button
                  color="rgb(11, 189, 135)" 
                  type="filled">Setup NFT Sale</vs-button>
              </vs-row>
            </nuxt-link>
          </div>
        </vs-card>
      </vs-col>
    </vs-row>
  </section>
</template>

<script>
export default {
  name: 'Setup',
  components: {}
}
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Nunito:300,400,600');

h1,
p,
.text-font {
  font-family: 'Nunito', Helvetica, sans-serif;
}
</style>
