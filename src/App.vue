<template>
  <div class="px-6 md:px-14">
    <div class="flex flex-row w-full justify-between pt-4">
      <div class="flex flex-row">
        <div
          :class="[
            'py-2 px-2 text-[12px]',
            language === 'es' ? 'bg-gray-700 text-white' : '',
            'rounded-md',
          ]"
          @keyup="() => {}"
          @click="setSpanish"
          style="cursor: pointer"
        >
          Español
        </div>
        <div
          :class="[
            'py-2 px-2 text-[12px]',
            language === 'en' ? 'bg-gray-700 text-white' : '',
            'rounded-md',
          ]"
          @click="setEnglish"
          @keyup="() => {}"
          style="cursor: pointer"
        >
          English
        </div>
      </div>
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
      <QuoteCard :quotesData="quotes" :languageData="language" @setAuthor="setAuthor" />
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
      language: 'es',
      author: '',
      quotes: [],
      baseUrl: 'https://inspiring-quotes.p.rapidapi.com/multiple',
      transBaseUrl: 'https://translated-mymemory---translation-memory.p.rapidapi.com/api/get',
      quotesHeaders: {
        'x-rapidapi-key': 'e7efca42d3msh724414ab88b87c9p174415jsn385d1bb520aa',
        'x-rapidapi-host': 'inspiring-quotes.p.rapidapi.com',
      },
      translateParams: {
        langpair: 'en|es',
        q: '',
        mt: '1',
        onlyprivate: '0',
        de: 'a@b.c',
      },
      translateHeaders: {
        'x-rapidapi-host': 'translated-mymemory---translation-memory.p.rapidapi.com',
        'x-rapidapi-key': 'a03292e16bmsh16bbd044a355108p178b58jsnf37ef0ad6671',
      },
    };
  },
  mounted() {
    this.getQuote();
    document.title = 'DevChallenges | Quotes Generator';
  },
  methods: {
    async getQuote() {
      this.author = '';
      this.quotes = [];
      const res = await axios.get(`${this.baseUrl}/?count=1`, {
        headers: this.quotesHeaders,
      });
      if (res) {
        const quoteText = res.data.data[0].quote;
        const translatedQuote = await this.translateQuote(quoteText);
        this.quotes = res.data.data;
        if (translatedQuote) {
          this.quotes[0].translatedQuote = translatedQuote;
        } else {
          this.quotes[0].translatedQuote = quoteText;
        }
      }
    },
    async translateQuote(quote) {
      const res = await axios.get(`${this.transBaseUrl}?q=${quote}&langpair=en|es&mt-1`, {
        headers: this.translateHeaders,
      });
      if (res) {
        return res.data.responseData.translatedText;
      }
      return false;
    },
    async getQuotesByAuthor() {
      this.quotes = [];
      const res = await axios.get(`${this.baseUrl}/?count=4&author=${this.author}`, {
        headers: this.quotesHeaders,
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
    setSpanish() {
      this.language = 'es';
    },
    setEnglish() {
      this.language = 'en';
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
