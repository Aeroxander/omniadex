<template>
  <div class="parentx-static">
    <vs-sidebar 
      :parent="$refs.parentSidebar"
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
              v-for="pair in pairs.filter(pair => pair.id.includes('WETH'))"
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
          type="border"/>
      </div>
    </vs-sidebar>
  </div>
</template>

<script>
export default {
  asyncData({ $axios }) {
    return $axios.$get('https://api.radarrelay.com/v2/markets').then(res => {
      return { pairs: res }
    })
  },
  name: 'SideBar',
  props: {
    open: {
      type: Boolean,
      default: false
    }
  },
  data: () => ({})
}
</script>

<style>
h4,
a {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  font-size: xx-small;
}
.header-sidebar {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 100%;
}
.header-sidebar h4 {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
}
.header-sidebar h4 > button {
  margin-left: 10px;
}
.footer-sidebar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}
.footer-sidebar > button {
  border: 0px solid rgba(0, 0, 0, 0) !important;
  border-left: 1px solid rgba(0, 0, 0, 0.07) !important;
  border-radius: 0px !important;
}
</style>
