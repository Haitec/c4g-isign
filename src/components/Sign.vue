<template>
  <div class="sign">

    <div id="triggers">
      <button @click="trigger('light')">Light</button>
      <button @click="trigger('noise')">Noise</button>
      <button @click="trigger('motion')">Motion</button>
    </div>

    <div id="signs" v-if="signsAreVisible">
      <img alt="Durchfahrt verboten" src="https://www.die-auto-welt.de/wp-content/uploads/2017/05/verbot-fahrzeuge.gif">
      <img alt="Anlieger frei" src="https://www.die-auto-welt.de/wp-content/uploads/2017/05/anlieger-frei.png">
    </div>

  </div>
</template>

<script>
export default {
  name: "Sign",

  data() {
    return {
      signsAreVisible: false
    };
  },

  methods: {
    trigger(action) {
      window.console.log("triggered action: " + action);
    }
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
