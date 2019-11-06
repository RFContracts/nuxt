<template>
  <v-layout>
    <v-flex xs4>
      <nuxt-link to="/">
        <Logo></Logo>
      </nuxt-link>
    </v-flex>
    <v-flex xs1 offset-xs7 d-flex justify-end>
      <v-menu open-delay="200" transition="fade-transition" open-on-hover offset-y>
        <template v-slot:activator="{ on }">
          <div class="socials">
            <a :href="linksKey.Facebook" class="menu-icon"> <svg width="20" height="20" class="icon" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg" data-svg="facebook"><path class="i" d="M11,10h2.6l0.4-3H11V5.3c0-0.9,0.2-1.5,1.5-1.5H14V1.1c-0.3,0-1-0.1-2.1-0.1C9.6,1,8,2.4,8,5v2H5.5v3H8v8h3V10z"></path></svg> </a>
            <a :href="linksKey.Whatsapp" class="menu-icon"> <svg width="20" height="20" class="icon" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg" data-svg="instagram"><path class="i" d="M13.55,1H6.46C3.45,1,1,3.44,1,6.44v7.12c0,3,2.45,5.44,5.46,5.44h7.08c3.02,0,5.46-2.44,5.46-5.44V6.44 C19.01,3.44,16.56,1,13.55,1z M17.5,14c0,1.93-1.57,3.5-3.5,3.5H6c-1.93,0-3.5-1.57-3.5-3.5V6c0-1.93,1.57-3.5,3.5-3.5h8 c1.93,0,3.5,1.57,3.5,3.5V14z"></path><circle cx="14.87" cy="5.26" r="1.09"></circle><path d="M10.03,5.45c-2.55,0-4.63,2.06-4.63,4.6c0,2.55,2.07,4.61,4.63,4.61c2.56,0,4.63-2.061,4.63-4.61 C14.65,7.51,12.58,5.45,10.03,5.45L10.03,5.45L10.03,5.45z M10.08,13c-1.66,0-3-1.34-3-2.99c0-1.65,1.34-2.99,3-2.99s3,1.34,3,2.99 C13.08,11.66,11.74,13,10.08,13L10.08,13L10.08,13z"></path></svg> </a>
          </div>
          <v-btn v-on="on" class="handle-menu">
            <ChatIcon></ChatIcon>
          </v-btn>
        </template>
        <v-list>
          <v-flex class="lang-link" @click="changeLang">En</v-flex>
          <v-flex
            v-for="(item, index) in this.links"
            :key="index"
            @click=""
          >
            <a v-if="item.name === 'Facebook'" :href="item.url" class="menu-icon fb-icon"></a>
            <a v-if="item.name === 'Whatsapp'" :href="item.url" class="menu-icon whatsapp-icon"></a>
            <a v-if="item.name === 'Email'" :href="item.url" class="menu-icon mail-icon"></a>
          </v-flex>
        </v-list>
      </v-menu>
    </v-flex>
  </v-layout>
</template>

<script>
  import {mapActions, mapGetters} from "vuex"

  const Logo = () => import("~/components/Logo")
  const ChatIcon = () => import("~/components/common/ChatIcon")

  export default {
    components: {
      Logo,
      ChatIcon
    },
    async mounted() {
      await this.GET_OBJECTS(this.lang)
    },
    computed: {
      ...mapGetters("header",
        [
          "linksKey",
          "links",
          "languages",
          "lang"
        ]
      )
    },
    methods: {
      ...mapActions("header", ["GET_OBJECTS", "SET_LANG"]),
      async changeLang() {
        let newLang = "";
        if (this.lang === "en") {
          newLang = "ru"
        } else {
          newLang = "en"
        }
        await this.SET_LANG(newLang)
        await this.GET_OBJECTS(newLang)
      }
    }
  }
</script>

<style scoped>
  .menu-container {
    position: relative;
  }

  .menu-item {
    font-family: "KlavikaBold-BoldOSF";
    font-size: 14px;
    text-decoration: none;
    text-transform: uppercase;
    position: relative;
    -moz-transition: all 1s ease-out;
    -o-transition: all 1s ease-out;
    -webkit-transition: all 1s ease-out;
  }

  .menu-item:before {
    content: " ";
    display: block;
    position: absolute;
    bottom: 2px;
    left: -50px;
    width: 40px;
    background-color: #575757;
    height: 2px;
    opacity: .8;
  }

  .menu-item:hover {
    color: black !important;
  }

  .handle-menu {
    background: transparent !important;
    box-shadow: none !important;
    padding-right: 0 !important;
  }

  .handle-menu:before {
    opacity: 0 !important;
  }

  .v-ripple-container {
    opacity: 0 !important;
    background: transparent !important;
    display: none !important;
  }

  .v-menu__content {
    box-shadow: none !important;
  }

  .v-list {
    background-color: transparent !important;
  }

  .menu-icon {
    width: 20px;
    height: 20px;
    display: block;
    margin: 20px auto;
    background-size: cover;
  }

  .lang-link {
    font-family: "KlavikaBold-BoldOSF";
    margin: 20px auto;
    color: #7d7d7d;
    text-align: center;
    cursor: pointer;
  }

  .fb-icon {
    background-image: url("/fb.png");
  }

  .whatsapp-icon {
    background-image: url("/whatsapp.png");
  }

  .mail-icon {
    background-image: url("/email.png");
  }

  .socials{
    display: block;
    width: 80px;
    height: 40px;
  }
  .socials > a{
    display: inline-block;
    color: rgba(0, 0, 0, 0.47);
    margin: 0 5px;
  }
  .socials > a > .icon{
    width: 30px;
    height: 30px;
  }
</style>
