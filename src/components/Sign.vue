<template>
  <div class="sign">

    <div id="triggers" v-if="isDev">
      <button @click="rotate()">Rotate &minus;90&deg;</button>
      <button @click="trigger('light')" :disabled="isLoading">Light</button>
      <button @click="trigger('noise')" :disabled="isLoading">Noise</button>
      <button @click="trigger('motion')" :disabled="isLoading">Motion</button>
    </div>

    <div id="signs" :style="signStyle">
      <img v-if="ok" alt="Durchfahrt erlaubt" src="https://www.die-auto-welt.de/wp-content/uploads/2017/05/30-zone-ende.gif">
      
      <img v-if="!ok" alt="Durchfahrt verboten" src="https://www.die-auto-welt.de/wp-content/uploads/2017/05/verbot-fahrzeuge.gif">
      <!-- <img v-if="!ok" alt="Anlieger frei" src="https://www.die-auto-welt.de/wp-content/uploads/2017/05/anlieger-frei.png"> -->
    </div>

  </div>
</template>

<script>
const INTERVAL = 1 * 30 * 1000;

export default {
  name: "Sign",

  data() {
    return {
      isDev: false,
      isLoading: false,
      ok: true,

      signRotation: 0,

      signStyle: {
        transform: ""
      }
    };
  },

  methods: {
    rotate() {
      if (this.signRotation == 0 || this.signRotation > -270) {
        this.signRotation -= 90;
      } else {
        this.signRotation = 0;
      }
      this.signStyle.transform = "rotate(" + this.signRotation + "deg)";
    },

    trigger(action) {
      window.console.log("triggered action: " + action);
      this.isLoading = true;
      let data = { action: action };

      fetch("https://jsonplaceholder.typicode.com/posts", {
        method: "POST",
        headers: {
          "Content-Type": "application/json; charset=utf-8"
        },
        body: JSON.stringify(data)
      })
        .then(response => response.json())
        .then(json => {
          window.console.log("response", json);
          this.isLoading = false;
          this.ok = !this.ok;
        });
    },

    checkStatus() {
      fetch("https://jsonplaceholder.typicode.com/todos/101")
        .then(response => response.json())
        .then(json => {
          window.console.log("response", json);
          this.isLoading = false;
          this.ok = !this.ok;
          setTimeout(this.checkStatus, INTERVAL);
        });
    }
  },

  created() {
    setTimeout(this.checkStatus, INTERVAL);

    this.isDev = window.location.search.substr(1) == "dev";
  }
};
</script>

<style scoped>
#triggers {
  position: fixed;
  z-index: 1;
}

#signs {
  text-align: center;
}

#signs img {
  width: 100%;
  height: 100%;
}
</style>
