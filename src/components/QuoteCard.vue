<template>
  <div>
    <div v-for="(item, i) in quotesData" :key="i" class="py-8">
      <div
        class="flex flex-col items-center justify-center w-96 border-l-4 border-orange-200 px-8"
      >
        <div v-if="languageData === 'en'">
          {{ `"${item.quote}"` }}
        </div>
        <div v-if="languageData === 'es'">
          {{ `"${item.translatedQuote}"` }}
        </div>
      </div>
      <div
        v-if="quotesData.length === 1"
        :class="[
          'px-5 mt-10 py-3 mx-8',
          'flex flex-row justify-between',
          'hover:bg-neutral-800 hover:text-white',
        ]"
        style="cursor: pointer"
        @click="setAuthor(item.author)"
        @keyup="() => {}"
      >
        <div>
          <div class="text-[13px] text-gray-600 font-bold">{{ item.author }}</div>
          <div class="text-[10px] text-gray-500">{{ item.genre }}</div>
        </div>
        <button>
          <span class="material-icons inline text-white">arrow_right_alt</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    quotesData: {
      type: Array,
      default() {
        return [];
      },
    },
    languageData: {
      type: String,
      default() {
        return 'en';
      },
    },
  },
  methods: {
    setAuthor(author) {
      this.$emit('setAuthor', author);
    },
  },
};
</script>

<style></style>
