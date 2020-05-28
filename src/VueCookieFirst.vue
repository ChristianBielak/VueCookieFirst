<template>
  <div class="v-cookiefirst" v-if="CookieFirst && !isAccepted">
    <div class="v-cookiefirst-banner" :style="bannerStyle.banner">
      <div class="v-cookiefirst-content">
        <div class="v-cookiefirst-text-wrapper">
          <p>
            <strong>{{ title }}</strong>
          </p>
          <p v-html="text"></p>
        </div>
        <div class="v-cookiefirst-button-wrapper">
          <button
            @click="acceptAllCategories"
            :style="bannerStyle.acceptButton"
            class="v-cookiefirst-button"
          >{{acceptButton}}</button>
          <button
            @click="declineAllCategories"
            :style="bannerStyle.declineButton"
            class="v-cookiefirst-button"
          >{{declineButton}}</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VueCookieFirst",
  props: {
    apiKey: {
      type: String,
      required: true
    },
    trans: {
      type: Object,
      default: () => ({
        de: {
          title: "Wir Verwenden Cookies",
          text:
            "Wir können diese zur Analyse unserer Besucherdaten platzieren, um unsere Website zu verbessern, personalisierte Inhalte anzuzeigen und Ihnen ein großartiges Website-Erlebnis zu bieten. Für weitere Informationen zu den von uns verwendeten Cookies öffnen Sie die Einstellungen.",
          acceptButton: "akzeptieren",
          declineButton: "ablehnen"
        },
        en: {
          title: "We are using Cookies",
          text:
            "This website uses cookies to ensure you get the best experience on our website. By using the website, you agree.",
          acceptButton: "accept",
          declineButton: "decline"
        }
      })
    },
    bannerStyle: {
      type: Object,
      default: () => ({
        banner: {
          backgroundColor: "#fff",
          color: "#000"
        },
        acceptButton: {
          backgroundColor: "#fff",
          color: "#000",
          borderColor: "#000",
          borderRadius: 0,
          borderWidth: "1px",
          borderStyle: "solid"
        },
        declineButton: {
          backgroundColor: "#fff",
          color: "#000",
          borderColor: "#000",
          borderRadius: 0,
          borderWidth: "1px",
          borderStyle: "solid"
        }
      })
    }
  },
  data() {
    return {
      checkCookieFirst: null,
      scriptIsLoaded: false,
      CookieFirst: {},
      isAccepted: false
    };
  },
  created() {
    this.loadCookieFirst();
  },
  mounted() {
    this.checkCookieFirst = setInterval(() => {
      if (window.CookieFirst) {
        this.scriptIsLoaded = true;
        this.CookieFirst = window.CookieFirst;
        clearInterval(this.checkCookieFirst);
        if (this.CookieFirst.consent) {
          this.isAccepted = true;
        }
      }
    }, 10);
  },
  methods: {
    loadCookieFirst() {
      let script = document.createElement("script");
      script.setAttribute("src", "https://app.cookiefirst.com/loader/init.js");
      script.setAttribute("data-cookiefirst-key", this.apiKey);
      script.async = true;
      script.defer = true;

      document.head.appendChild(script);
    },
    acceptAllCategories() {
      this.CookieFirst.acceptAllCategories();
      this.isAccepted = true;
    },
    declineAllCategories() {
      this.CookieFirst.declineAllCategories();
      this.isAccepted = true;
    }
  },
  computed: {
    currentLang() {
      return document.documentElement.lang;
    },
    title() {
      return this.trans[this.currentLang].title;
    },
    text() {
      return this.trans[this.currentLang].text;
    },
    acceptButton() {
      return this.trans[this.currentLang].acceptButton;
    },
    declineButton() {
      return this.trans[this.currentLang].declineButton;
    }
  }
};
</script>

<style lang="scss">
.cookiefirst-root {
  display: none !important;
}

.v-cookiefirst {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 999999900;
  padding-top: 24px;
  overflow: hidden;

  .v-cookiefirst-banner {
    box-shadow: 0 0 24px rgba(0, 0, 0, 0.25);
    padding: 19px 15px;

    .v-cookiefirst-content {
      display: flex;
      align-items: center;
      flex-wrap: wrap;

      .v-cookiefirst-text-wrapper {
        flex-grow: 1;
        text-align: left;
      }

      .v-cookiefirst-button-wrapper {
        display: flex;
        justify-content: center;
        flex-grow: 1;
        @media screen and (min-width: 1023px) {
          justify-content: flex-end;
        }

        .v-cookiefirst-button {
          font-size: 16px;
          background: transparent;
          cursor: pointer;
          margin-right: 20px;
          padding: 4px 12px;

          &:last-of-type {
            margin-right: 0;
          }
        }
      }
    }
  }
}
</style>
