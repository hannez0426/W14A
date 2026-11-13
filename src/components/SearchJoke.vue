
<template>
  <div class="search-card">
    <h2>Search for a Joke</h2>

    <input
      v-model="searchWord"
      type="text"
      placeholder="Enter a word..."
      @keyup.enter="searchJoke"
    />

    <button
      @click="searchJoke"
      :disabled="loading"
    >
      {{ loading ? "Searching..." : "Search Joke" }}
    </button>

    <p v-if="error" class="error">{{ error }}</p>

    <div v-if="result" class="search-result">
      <p v-if="result.type === 'single'">
        {{ result.joke }}
      </p>

      <div v-else>
        <p>{{ result.setup }}</p>
        <p><strong>{{ result.delivery }}</strong></p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "SearchJoke",

  data() {
    return {
      searchWord: "",
      result: null,
      error: "",
      loading: false
    };
  },

  methods: {
    async searchJoke() {
      const word = this.searchWord.trim();

      if (!word) {
        this.error = "Please enter a word.";
        this.result = null;
        return;
      }

      this.loading = true;
      this.error = "";
      this.result = null;

      try {
        const response = await axios.get(
          "https://v2.jokeapi.dev/joke/Any",
          {
            params: {
              contains: word,
              safeMode: ""
            }
          }
        );

        if (response.data.error) {
          this.error = "No joke found. Try another word.";
          return;
        }

        this.result = response.data;

      } catch (error) {
        console.error(error);
        this.error = "Unable to search for jokes.";
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>
