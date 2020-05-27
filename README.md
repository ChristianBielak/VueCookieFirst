# vue-cookie-first

A simple Component for using Cookiefirst Custom Banner.

## Installation

```
yarn install
or
npm install
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
