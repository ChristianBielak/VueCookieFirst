# vue-cookie-first

A simple Component for using Cookiefirst Custom Banner.

## Installation

```
yarn add vue-cookie-first
or
npm install vue-cookie-first
```

## Usage

### Register the Component

```
import VueCookieFirst from "vue-cookie-first";

export default {
  name: "Demo",
  components: {
    VueCookieFirst
  }
};

```

### Use the Component

```
<vue-cookie-first api-key="XXXXX-XXXXXX-XXXXX-XXXXX-XXXXXXXX"></vue-cookie-first>
```

## Custom styling

```

<template>
    <vue-cookie-first api-key="XXXXX-XXXXXX-XXXXX-XXXXX-XXXXXXXX" :banner-style="cookieFirstStyle"></vue-cookie-first>
</template>

<script>
import VueCookieFirst from 'vue-cookie-first';
export default {
    name: "Demo",
    components: {
        VueCookieFirst
    },
    data() {
    return {
      cookieFirstStyle: {
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
      }
    };
  }
}
</script>
```

## Custom Text

```
<template>
    <vue-cookie-first api-key="XXXXX-XXXXXX-XXXXX-XXXXX-XXXXXXXX" :trans="cookieFirstTrans"></vue-cookie-first>
</template>

<script>
import VueCookieFirst from 'vue-cookie-first';
export default {
    name: "Demo",
    components: {
        VueCookieFirst
    },
    data() {
    return {
      cookieFirstTrans: {
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
      }
    };
  }
}
</script>
```
