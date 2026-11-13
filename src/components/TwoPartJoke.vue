
<template>
  <div class="joke-card">
    <h2>Two-Part Snake Joke</h2>

    <p class="setup">{{ snakeSetup }}</p>

    <p class="delivery">{{ snakeDelivery }}</p>
  </div>
</template>

<script>
export default {
  name: "TwoPartJoke",

  props: {
    setup: {
      type: String,
      required: true
    },

    delivery: {
      type: String,
      required: true
    }
  },

  computed: {
    snakeSetup() {
      return this.setup.replace(/\s/g, "_");
    },

    snakeDelivery() {
      return this.delivery.replace(/\s/g, "_");
    }
  }
};
</script>
