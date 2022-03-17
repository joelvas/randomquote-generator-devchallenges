<template>
  <div class="px-8 md:px-14">
    <div class="flex flex-row justify-end pt-4">
      <button class="flex flex-row items-center text-gray-500" @click="getQuote">
        <div class="text-[11px]">random</div>
        <span class="material-icons text-[16px] pl-1">autorenew</span>
      </button>
    </div>
    <div v-if="author !== ''" class="font-bold">{{ author }}</div>
    <div
      :class="[
        'flex flex-row justify-center items-center',
        'h-full',
        author !== '' ? ' mt-10' : 'mt-20',
      ]"
    >
      <QuoteCard :quotesData="quotes" @setAuthor="setAuthor" />
    </div>
    <div></div>
  </div>
</template>

<script>
import QuoteCard from './components/QuoteCard.vue';

const axios = require('axios');

export default {
  components: {
    QuoteCard,
  },
  name: 'App',
  data() {
    return {
      author: '',
      count: 1,
      quotes: [],
      baseUrl: 'https://inspiring-quotes.p.rapidapi.com/multiple',
      headers: {
        'x-rapidapi-key': 'e7efca42d3msh724414ab88b87c9p174415jsn385d1bb520aa',
        'x-rapidapi-host': 'inspiring-quotes.p.rapidapi.com',
      },
    };
  },
  mounted() {
    this.getQuote();
  },
  methods: {
    async getQuote() {
      this.author = '';
      this.quotes = [];
      const res = await axios.get(`${this.baseUrl}/?count= ${this.count}`, {
        headers: this.headers,
      });
      if (res) {
        this.quotes = res.data.data;
      }
    },
    async getQuotesByAuthor() {
      this.quotes = [];
      const res = await axios.get(`${this.baseUrl}/?count=5&author=${this.author}`, {
        headers: this.headers,
      });
      if (res) {
        this.quotes = res.data.data;
      }
    },
    setAuthor(author) {
      this.author = author;
      this.quotes = [];
      this.getQuotesByAuthor();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
