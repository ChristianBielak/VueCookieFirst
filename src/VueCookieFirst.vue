<template>
  <div class="v-cookiefirst" v-if="scriptIsLoaded && !isAccepted">
    <div class="v-cookiefirst-banner" :style="bannerStyle.banner">
      <div class="v-cookiefirst-content">
        <div class="v-cookiefirst-text-wrapper">
          <p class="v-cookiefirst-title">
            <strong>{{ title }}</strong>
          </p>
          <p v-html="text"></p>
        </div>
        <div class="v-cookiefirst-checkbox-wrapper">
          <p class="v-cookiefirst-subtitle">
            <strong>
              {{ subtitle }}
            </strong>
          </p>
          <div class="v-cookiefirst-checkboxes">
            <div class="v-cookiefirst-checkbox">
              <label class="label--checkbox">
                Necessary
                <input
                  type="checkbox"
                  class="checkbox"
                  :checked="consent.necessary"
                  v-model="consent.necessary"
                  disabled
                />
                <span class="custom-checkbox"></span>
              </label>
            </div>
            <div class="v-cookiefirst-checkbox">
              <label class="label--checkbox">
                Performance
                <input
                  type="checkbox"
                  class="checkbox"
                  :checked="consent.performance"
                  v-model="consent.performance"
                />
                <span class="custom-checkbox"></span>
              </label>
            </div>
            <div class="v-cookiefirst-checkbox">
              <label class="label--checkbox">
                Functional
                <input
                  type="checkbox"
                  class="checkbox"
                  :checked="consent.functional"
                  v-model="consent.functional"
                />
                <span class="custom-checkbox"></span>
              </label>
            </div>
            <div class="v-cookiefirst-checkbox">
              <label class="label--checkbox">
                Advertising
                <input
                  type="checkbox"
                  class="checkbox"
                  :checked="consent.advertising"
                  v-model="consent.advertising"
                />
                <span class="custom-checkbox"></span>
              </label>
            </div>
          </div>
        </div>
        <div class="v-cookiefirst-button-wrapper">
          <button
            @click="acceptSelectedCategories()"
            :style="bannerStyle.declineButton"
            class="v-cookiefirst-button"
          >
            {{ acceptSelectedButton }}
          </button>
          <button
            @click="acceptAllCategories()"
            :style="bannerStyle.acceptButton"
            class="v-cookiefirst-button"
          >
            {{ acceptAllButton }}
          </button>
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
      required: true,
    },
    trans: {
      type: Object,
      default: () => ({
        de: {
          title: "Wir respektieren Ihre Privatsphäre",
          text:
            "Wir und bestimmte Dritte verwenden Cookies. Einzelheiten zu den Arten von Cookies, ihrem Zweck und den beteiligten Stellen finden Sie in unserem Cookie-Hinweis. Bitte willigen Sie in die Verwendung von Cookies ein, indem Sie auf „Alle zulassen und fortsetzen“ klicken, um die bestmögliche Performance auf unserer Webseite zu haben.",
          subtitle: "Cookie Präferenzen verwalten",
          acceptAllButton: "Alle zulassen und fortsetzen",
          acceptSelectedButton: "Meine Auswahl bestätigen",
        },
        en: {
          title: "We are using Cookies",
          text:
            "This website uses cookies to ensure you get the best experience on our website. By using the website, you agree.",
          subtitle: "Manage cookie settings",
          acceptAllButton: "Accept all",
          acceptSelectedButton: "Accept selected",
        },
      }),
    },
    bannerStyle: {
      type: Object,
      default: () => ({
        banner: {
          backgroundColor: "#fff",
          color: "#000",
        },
        acceptButton: {
          backgroundColor: "#fff",
          color: "#000",
          borderColor: "#000",
          borderRadius: 0,
          borderWidth: "1px",
          borderStyle: "solid",
        },
        declineButton: {
          backgroundColor: "#fff",
          color: "#000",
          borderColor: "#000",
          borderRadius: 0,
          borderWidth: "1px",
          borderStyle: "solid",
        },
      }),
    },
  },
  data() {
    return {
      checkCookieFirst: null,
      scriptIsLoaded: false,
      CookieFirst: {},
      isAccepted: false,
      consent: {
        necessary: true, // necessary category can't be disabled
        performance: false,
        functional: false,
        advertising: false,
      },
    };
  },
  created() {
    this.loadCookieFirst();
  },
  mounted() {
    window.addEventListener("cf_init", this.onLoad);
  },
  methods: {
    loadCookieFirst() {
      let script = document.createElement("script");
      script.setAttribute("src", "https://app.cookiefirst.com/loader/init.js");
      script.setAttribute("data-cookiefirst-key", this.apiKey);
      script.async = true;
      script.defer = true;

      document.body.appendChild(script);
    },
    onLoad() {
      this.scriptIsLoaded = true;
      this.CookieFirst = window.CookieFirst;
      clearInterval(this.checkCookieFirst);
      if (this.CookieFirst.consent) {
        this.isAccepted = true;
        console.log(this.CookieFirst.consent);
      }
    },
    acceptAllCategories() {
      this.CookieFirst.acceptAllCategories();
      this.isAccepted = true;
    },
    acceptSelectedCategories() {
      this.CookieFirst.updateConsent(this.consent);
      this.isAccepted = true;
    },
  },
  computed: {
    currentLang() {
      if (document.documentElement.lang) {
        return document.documentElement.lang;
      } else {
        return "en";
      }
    },
    title() {
      return this.trans[this.currentLang].title;
    },
    text() {
      return this.trans[this.currentLang].text;
    },
    subtitle() {
      return this.trans[this.currentLang].subtitle;
    },
    acceptAllButton() {
      return this.trans[this.currentLang].acceptAllButton;
    },
    acceptSelectedButton() {
      return this.trans[this.currentLang].acceptSelectedButton;
    },
  },
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
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: flex-end;
  justify-content: center;

  p {
    margin: 0;
  }

  .v-cookiefirst-title {
    margin-bottom: 25px;
    font-size: 20px;
  }

  .v-cookiefirst-subtitle {
    margin-bottom: 25px;
  }

  .v-cookiefirst-banner {
    padding: 19px 15px;
    @media screen and (min-width: 1023px) {
      width: 75vw;
    }

    .v-cookiefirst-content {
      display: flex;
      flex-wrap: wrap;

      .v-cookiefirst-text-wrapper {
        text-align: left;
        margin-bottom: 40px;
      }

      .v-cookiefirst-button-wrapper {
        width: 100%;
        display: flex;
        justify-content: center;
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
      .v-cookiefirst-checkbox-wrapper {
        display: flex;
        flex-direction: column;
        width: 100%;
        text-align: left;
        margin-bottom: 30px;
        .v-cookiefirst-checkboxes {
          display: flex;
          flex-direction: column;
          @media screen and (min-width: 1023px) {
            flex-direction: row;
          }

          .v-cookiefirst-checkbox {
            display: flex;
            align-items: center;
            margin-bottom: 15px;

            @media screen and (min-width: 1023px) {
              margin-bottom: 0;
              margin-right: 50px;
            }

            .label--checkbox {
              position: relative;
              font-family: Arial, sans-serif;
              line-height: 135%;
              cursor: pointer;
              left: 0;
              display: inline-block;
              width: 50%;

              @media screen and (min-width: 480px) {
                width: 31%;
              }

              @media screen and (min-width: 850px) {
                width: 18%;
              }

              @media screen and (min-width: 1023px) {
                width: unset;
              }
            }

            .checkbox {
              position: relative;
              top: -2px;
              margin-right: 1.5rem !important;
              cursor: pointer;
              visibility: hidden;

              @supports (-ms-accelerator: true) {
                visibility: visible;
              }

              @supports (-ms-ime-align: auto) {
                visibility: visible;
              }

              @media all and (-ms-high-contrast: none),
                (-ms-high-contrast: active) {
                visibility: visible;
              }
            }

            .custom-checkbox {
              &:before {
                transition: all 0.3s ease-in-out;
                content: "";
                position: absolute;
                right: 0;
                top: -15%;
                z-index: 1;
                width: 1.5rem;
                height: 1.5rem;
                border: 2px solid #f2f2f2;
                visibility: visible;
              }

              &:after {
                content: "";
                visibility: visible;
                position: absolute;
                top: -15%;
                right: 0;
                width: 1.5rem;
                height: 1.5rem;
                background: #fff;
                cursor: pointer;
              }
            }

            .checkbox {
              &:checked {
                ~ .custom-checkbox {
                  &:before {
                    transform: rotate(-45deg);
                    height: 0.8rem;
                    top: 0.1rem;
                    border-color: gray;
                    border-top-style: none;
                    border-right-style: none;
                    visibility: visible;
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
</style>
