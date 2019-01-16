<template>
  <section class="container">
    <div class="about">
      <vs-divider color="primary">
        <h1>Our Team</h1>
      </vs-divider>
      <vs-row 
        vs-align="center" 
        vs-type="flex" 
        vs-justify="space-around" 
        vs-w="12">
        <vs-col
          vs-type="flex"
          vs-justify="center"
          vs-align="center"
          vs-lg="2.7"
          vs-sm="8"
          vs-xs="12"
          vs-w="2.5"
        >
          <vs-card>
            <div slot="header">
              <h3>Full-Stack Developer</h3>
            </div>
            <br>
            <div>
              <span>
                <vs-avatar
                  size="180px"
                  src="https://media.licdn.com/dms/image/C5603AQG2VXGcTcbk7w/profile-displayphoto-shrink_200_200/0?e=1551916800&v=beta&t=wzK5Vqaybey5Hxl2ZHP5tEdRlSSSR4pN0kIjW22GBRQ"
                />
                <br>Alexander Klus is a passionate about cutting edge to disrupt
                industries with decentralization and A.I
              </span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  type="gradient" 
                  color="primary">
                  <img
                    src="https://discordapp.com/assets/efc9b24f5779403afd12f5fc8594497f.svg"
                    alt="Twitter"
                  >
                </vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
        <vs-col
          vs-type="flex"
          vs-justify="center"
          vs-align="center"
          vs-lg="2.7"
          vs-sm="8"
          vs-xs="12"
          vs-w="2.5"
        >
          <vs-card>
            <div slot="header">
              <h3>Product Developer</h3>
            </div>
            <div>
              <span>
                <vs-avatar
                  size="180px"
                  src="https://cdn.discordapp.com/avatars/340082737354506261/bb0bd21265e97ce2109a06e7d5cd2cbe.png"
                />
                <br>This is Kelvin, he doesn't code (haha) and is a product
                developer.
              </span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  type="gradient" 
                  color="primary">
                  <img
                    src="https://discordapp.com/assets/efc9b24f5779403afd12f5fc8594497f.svg"
                    alt="Twitter"
                  >
                </vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
        <vs-col
          vs-type="flex"
          vs-justify="center"
          vs-align="center"
          vs-lg="2.7"
          vs-sm="8"
          vs-xs="12"
          vs-w="2.5"
        >
          <vs-card>
            <div slot="header">
              <h3>Smart-Contract Developer</h3>
            </div>
            <div>
              <span>
                <vs-avatar
                  size="180px"
                  src="https://avatars1.githubusercontent.com/u/4705997?s=460&v=4"
                />
                <br>Anson Lau is based in Hong Kong and is a smart contract
                programmer
              </span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  type="gradient" 
                  color="primary">
                  <img
                    src="https://discordapp.com/assets/efc9b24f5779403afd12f5fc8594497f.svg"
                    alt="Twitter"
                  >
                </vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
        <vs-col
          vs-type="flex"
          vs-justify="center"
          vs-align="center"
          vs-lg="2.7"
          vs-sm="8"
          vs-xs="12"
          vs-w="2.5"
        >
          <vs-card>
            <div slot="header">
              <h3>Full-Stack Developer</h3>
            </div>
            <div>
              <span>
                <vs-avatar
                  size="180px"
                  src="https://media.licdn.com/dms/image/C5603AQG2VXGcTcbk7w/profile-displayphoto-shrink_200_200/0?e=1551916800&v=beta&t=wzK5Vqaybey5Hxl2ZHP5tEdRlSSSR4pN0kIjW22GBRQ"
                />
                <br>Alexander Klus is a passionate about cutting edge to disrupt
                apps with decentralization and A.I
              </span>
            </div>
            <div slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button 
                  type="gradient" 
                  color="primary">
                  <img
                    src="https://discordapp.com/assets/efc9b24f5779403afd12f5fc8594497f.svg"
                    alt="Twitter"
                  >
                </vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
      </vs-row>
      <br>
      <h1>About us</h1>
      <vs-divider 
        class="text-font" 
        color="success">
        Our mission is to bring products come to life by
        <br>creating a platform
        to fund undeveloped ideas.
        <br>This platform will help show the
        <br>popularity and demand of an
        idea,
        <br>inspire the creators to take action on it
        <br>and the users to have a
        product come to life they love.
      </vs-divider>
    </div>
  </section>
</template>
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
