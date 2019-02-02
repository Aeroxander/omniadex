<template><section class="container"/></template>

<script>
export default {
  name: 'Instant',
  props: {
    open: {
      type: Boolean,
      default: false
    }
  },
  head() {
    return {
      script: [
        { src: 'https://instant.0x.org/instant.js', async: true, defer: true },
        {
          innerHTML:
            'zeroExInstant.render({ orderSource: "https://api.radarrelay.com/0x/v2/",}, "section");',
          type: 'text/javascript'
        }
      ],
      __dangerouslyDisableSanitizers: ['script']
    }
  }
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
