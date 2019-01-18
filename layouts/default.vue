<template>
  <div id="app">
    <div>
      <vs-navbar 
        v-model="activeItem" 
        class="nabarx">
        <vs-navbar-title>
          <img
            src="https://cdn.discordapp.com/attachments/529971696380149770/530530296446189578/Screen_Shot_2019-01-03_at_23.37.36.png"
            alt="Brand logo"
            height="50px"
          >
        </vs-navbar-title>

        <vs-spacer/>
        <vs-navbar-item index="0">
          <nuxt-link to="/sell">
            <vs-button
              color="#3dd495"
              gradient-color-secondary="rgb(130, 207, 23)"
              type="gradient"
            >Sell Token</vs-button>
          </nuxt-link>
        </vs-navbar-item>
        <vs-navbar-item index="1">
          <nuxt-link to="/">Marketplace</nuxt-link>
        </vs-navbar-item>
        <vs-navbar-item index="2">
          <nuxt-link to="/about">About</nuxt-link>
        </vs-navbar-item>
        <vs-navbar-item index="3">
          <nuxt-link to="/settings">Settings</nuxt-link>
        </vs-navbar-item>
      </vs-navbar>
    </div>
    <nuxt/>
  </div>
</template>
<script>
import SideBar from '@/components/SideBar'

export default {
  components: {
    SideBar
  },
  data: () => ({
    activeItem: 0
  })
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.nuxt-link-exact-active {
  color: #42b983;
}
</style>
