<template>
  <section class="container">
    <div class="donateHome">
      <vs-divider color="primary">
        <h1>Choose your pick</h1>
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
              <h3>For the visionaries</h3>
            </div>
            <div slot="media">
              <img
                src="https://static1.squarespace.com/static/53fba539e4b0a52701b62eb4/t/5854289e1b631b690caf3315/1481910437921/"
              >
            </div>
            <div>
              <span class="text-font">Innovate the world by raising money for your project</span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  color="primary" 
                  type="gradient">Become a creator</vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
        <vs-col 
          type="flex" 
          vs-justify="center" 
          vs-align="center" 
          vs-offset="-3" 
          vs-w="3">
          <vs-card 
            actionable 
            class="cardx">
            <div slot="header">
              <h3>For the believers</h3>
            </div>
            <div slot="media">
              <img
                src="https://static1.squarespace.com/static/53fba539e4b0a52701b62eb4/t/5854289e1b631b690caf3315/1481910437921/"
              >
            </div>
            <div>
              <span class="text-font">Help fund projects so innovators can work on their craft</span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  color="primary" 
                  type="gradient">Become a backer</vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
      </vs-row>
    </div>
  </section>
</template>

<script>
export default {
  name: 'DonateHome',
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
