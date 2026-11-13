
<template>
  <div class="joke-card">
    <h2>Your Joke</h2>

    <p class="joke-text">{{ formattedJoke }}</p>

    <div class="button-group">
      <button @click="mode = 'normal'">
        Normal Joke
      </button>

      <button @click="mode = 'loud'">
        Loud Joke
      </button>

      <button @click="mode = 'snake'">
        Snake Joke
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "NormalJokeButton",

  props: {
    joke: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      mode: "normal"
    };
  },

  computed: {
    formattedJoke() {
      if (this.mode === "loud") {
        return this.joke.toUpperCase();
      }

      if (this.mode === "snake") {
        return this.joke.replace(/\s/g, "_");
      }

      return this.joke;
    }
  }
};
</script>
