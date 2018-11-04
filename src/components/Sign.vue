<template>
  <div class="sign">

    <div id="triggers">
      <button @click="trigger('light')" :disabled="isLoading">Light</button>
      <button @click="trigger('noise')" :disabled="isLoading">Noise</button>
      <button @click="trigger('motion')" :disabled="isLoading">Motion</button>
    </div>

    <div id="signs">
      <img v-if="ok" alt="Durchfahrt erlaubt" src="https://www.die-auto-welt.de/wp-content/uploads/2017/05/30-zone-ende.gif">
      
      <img v-if="!ok" alt="Durchfahrt verboten" src="https://www.die-auto-welt.de/wp-content/uploads/2017/05/verbot-fahrzeuge.gif">
      <img v-if="!ok" alt="Anlieger frei" src="https://www.die-auto-welt.de/wp-content/uploads/2017/05/anlieger-frei.png">
    </div>

  </div>
</template>

<script>
export default {
  name: "Sign",

  data() {
    return {
      isLoading: false,
      ok: true
    };
  },

  methods: {
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
          setTimeout(this.checkStatus, 5000);
        });
    }
  },

  created() {
    setTimeout(this.checkStatus, 5000);
  }
};
</script>

<style scoped>
#signs {
  text-align: center;
}

#signs img {
  width: 50vw;
}
</style>
