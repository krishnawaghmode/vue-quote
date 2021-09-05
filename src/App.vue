<template>
  <div class="container">
  <div class="card justify-content-center align-items-center mt-5 shadow">
  <!-- <img class="img-thumbnail" src="./assets/logo.png" alt="Card image cap"> -->
  <div class="card-body">
     <select
        id="quote-author"
        class="form-control"
        v-model="currentAuthor"
        @change="setCurrentQuote"
      >
        <option value="">Select author</option>
        <option
          v-for="(author, index) in authors"
          v-bind:value="author"
          :key="index"
          >{{ author }}</option
        >
      </select>
    <p 
      id="text"
      class="my-4"
      :style="{ color: currentQuote.color }"
      >{{ currentQuote.quote }}</p>
    <h5 id="author" class="card-title text-end"
        :style="{color: currentQuote.color }"

    >- {{ currentQuote.author }}</h5>
 <!-- :style="{ backgroundColor: currentQuote.color }" -->
     <a
            id="tweet-quote"
            target="_blank"
            rel="nofollow"
            :href="tweetQuoteLink(currentQuote)"
            class="btn btn-primary"
          >
            Tweet
          </a>
           <button
            id="copy-quote"
            class="btn btn-dark m-5"
            @click="copyQuote"
          >
            Copy
          </button>
    <button
          href="javascript:void(0)"
          @click="setCurrentQuote"
          id="new-quote"
          class="btn btn-warning"
        >
          New Quote
        </button>
  </div>
</div>
  </div>
</template>

<script>
const quotes = require("./assets/quotes.json");
export default {
  name: "App",
  data() {
    return {
      quotes,
      currentQuote: {},
      authors: [],
      currentAuthor: "",
    };
  },
  methods: {
    random(items) {
      return items[Math.floor(Math.random() * items.length)];
    },
    setCurrentQuote() {
      if(this.currentAuthor != "") {
        this.currentQuote = this.random(this.quotes.filter(quote => quote.author == this.currentAuthor));
      } else {
        this.currentQuote = this.random(this.quotes);
      }
      this.currentQuote["color"] = this.generateRandColor();
    },
    setAuthors() {
      this.authors = [...new Set(this.quotes.map((item) => item.author))];
    },
    tweetQuoteLink(obj) {
      let encoded_text = encodeURIComponent(`${obj.quote} ${obj.author}`);
      return `https://twitter.com/intent/tweet?hashtags=quotes&related=freecodecamp&text=${encoded_text}`;
    },
    generateRandColor() {
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += Math.floor(Math.random() * 10);
      }
      return color;
    },
    copyQuote() {
     
       let quoteText = `${this.currentQuote.quote} -${this.currentQuote.author}`;
       alert(quoteText)
    },
  },
  mounted() {
    this.setAuthors();
    this.setCurrentQuote();
  },
};
</script>

<style></style>
