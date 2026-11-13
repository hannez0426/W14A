<template>
  <div id="app">
    <header>
      <h1 class="google-title">
        Joke Generator
      </h1>

      <p>Need a laugh? Generate a random joke!</p>
    </header>

    <main>
      <button
        class="generate-btn"
        @click="getJoke"
        :disabled="loading"
      >
        {{ loading ? "Loading..." : "Generate New Joke" }}
      </button>

      <p v-if="error" class="error">{{ error }}</p>

      <div class="joke-layout">

        <!-- LEFT: Normal Joke -->
        <section class="left-column">
          <NormalJokeButton
            v-if="currentJoke"
            :joke="
              currentJoke.type === 'single'
                ? currentJoke.joke
                : currentJoke.setup + '\n' + currentJoke.delivery
            "
            :key="jokeNumber"
          />
        </section>

        <!-- RIGHT: Search and Two-Part Joke -->
        <section class="right-column">

          <!-- Top Right -->
          <SearchJoke />

          <!-- Bottom Right -->
          <TwoPartJoke
            v-if="currentJoke && currentJoke.type === 'twopart'"
            :setup="currentJoke.setup"
            :delivery="currentJoke.delivery"
          />

          <div v-else class="joke-card">
            <h2>Two-Part Joke</h2>
            <p>Generate a two-part joke to see it here.</p>
          </div>

        </section>
      </div>
    </main>

    <footer>
      <p>Made with Vue.js and Axios</p>
    </footer>
  </div>
</template>


<script>
import axios from "axios";

import NormalJokeButton from "./components/NormalJokeButton.vue";
import TwoPartJoke from "./components/TwoPartJoke.vue";
import SearchJoke from "./components/SearchJoke.vue";

export default {
  name: "App",

  components: {
    NormalJokeButton,
    TwoPartJoke,
    SearchJoke
  },

  data() {
    return {
      currentJoke: null,
      loading: false,
      error: "",
      jokeNumber: 0
    };
  },

  mounted() {
    this.getJoke();
  },

  methods: {
    async getJoke() {
      this.loading = true;
      this.error = "";

      try {
        const response = await axios.get(
          "https://v2.jokeapi.dev/joke/Any?safe-mode"
        );

        if (response.data.error) {
          throw new Error("JokeAPI returned an error.");
        }

        this.currentJoke = response.data;
        this.jokeNumber++;

      } catch (error) {
        console.error(error);
        this.error = "Could not load a joke. Try again.";
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>




<style>
/* General */
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
  background-color: #ffffff;
  color: #111111;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  text-align: center;
}

/* Header */
header {
  background-color: #ffffff;
  padding: 45px 20px 25px;
  border-bottom: 1px solid #e5e5e5;
}

header h1 {
  font-size: 38px;
  font-weight: 600;
  color: #000000;
  margin: 0 0 12px;
  letter-spacing: -1px;
}

header p {
  color: #555555;
  font-size: 15px;
  margin: 0;
}

/* Main */
main {
  width: 95%;
  max-width: 1200px;
  margin: 30px auto 50px;
  flex: 1;
}

/* Generate Button */
.generate-btn {
  background-color: #000000;
  color: #ffffff;
  border: 1px solid #000000;
  border-radius: 25px;
  padding: 14px 30px;
  font-size: 15px;
  margin: 5px 0 30px;
}

/* All Buttons */
button {
  font-family: Arial, Helvetica, sans-serif;
  background-color: #f5f5f5;
  color: #111111;
  border: 1px solid #dddddd;
  border-radius: 6px;
  padding: 12px 18px;
  margin: 5px;
  font-size: 14px;
  cursor: pointer;
  transition: 0.2s ease;
}

button:hover:not(:disabled) {
  background-color: #e5e5e5;
  border-color: #999999;
}

.generate-btn {
  background-color: #000000;
  color: #ffffff;
  border-color: #000000;
}

.generate-btn:hover:not(:disabled) {
  background-color: #333333;
  border-color: #333333;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Two Column Layout */
.joke-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
  align-items: stretch;
}

/* Left Column */
.left-column {
  display: flex;
  flex-direction: column;
}

.left-column .joke-card {
  flex: 1;
  margin: 0;
  min-height: 350px;
}

/* Right Column */
.right-column {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.right-column .search-card,
.right-column .joke-card {
  margin: 0;
  min-height: 180px;
}

/* Joke Cards */
.joke-card,
.search-card {
  background-color: #ffffff;
  border: 1px solid #dcdcdc;
  border-radius: 12px;
  padding: 28px;
  text-align: center;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.04);
}

.joke-card h2,
.search-card h2 {
  color: #000000;
  font-size: 21px;
  font-weight: 600;
  margin: 0 0 22px;
}

/* Joke Text */
.joke-text {
  font-size: 18px;
  color: #222222;
  line-height: 1.8;
  white-space: pre-line;
  overflow-wrap: anywhere;
}

.setup {
  font-size: 17px;
  font-weight: 600;
  color: #111111;
  line-height: 1.7;
  overflow-wrap: anywhere;
}

.delivery {
  font-size: 17px;
  color: #555555;
  line-height: 1.7;
  overflow-wrap: anywhere;
}

/* Joke Options */
.button-group {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 25px;
}

.button-group button {
  margin: 0;
}

/* Search Bar */
input {
  width: 100%;
  max-width: 450px;
  padding: 15px 20px;
  border: 1px solid #cccccc;
  border-radius: 30px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 15px;
  color: #111111;
  background-color: #ffffff;
  outline: none;
}

input:focus {
  border-color: #000000;
  box-shadow: 0 0 0 1px #000000;
}

/* Search Result */
.search-result {
  background-color: #f7f7f7;
  color: #222222;
  padding: 20px;
  margin-top: 22px;
  border-radius: 10px;
  line-height: 1.7;
  overflow-wrap: anywhere;
}

/* Error */
.error {
  color: #444444;
  font-size: 14px;
}

/* Footer */
footer {
  background-color: #f5f5f5;
  color: #666666;
  padding: 25px 15px;
  font-size: 13px;
  border-top: 1px solid #e5e5e5;
}

/* Mobile Responsive */
@media (max-width: 768px) {
  header {
    padding: 30px 15px 20px;
  }

  header h1 {
    font-size: 30px;
  }

  main {
    width: 94%;
    margin: 20px auto 35px;
  }

  .joke-layout {
    grid-template-columns: 1fr;
    gap: 18px;
  }

  .right-column {
    gap: 18px;
  }

  .left-column .joke-card,
  .right-column .search-card,
  .right-column .joke-card {
    min-height: auto;
  }

  .joke-card,
  .search-card {
    padding: 22px 16px;
  }

  .button-group {
    flex-direction: column;
  }

  .button-group button {
    width: 100%;
  }

  input {
    max-width: 100%;
  }
}
</style>

