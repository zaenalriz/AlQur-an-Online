<template >
  <nav-bar></nav-bar>

  <div class="flex justify-center">
    <span class="text-gray-400 text-3xl mt-10">Baca Al Qur'an</span>
  </div>
  <div class="flex justify-center">
    <div>
      <input
        type="text"
        placeholder="search surah"
        class="text-gray-400 w-96 focus:outline-none text-xl shadow-md p-2 border-2 rounded-full border-green-300 mt-3"
        v-model="search"
      />
      <button class="z-50 relative right-12 top-2">
        <svg
          width="28"
          height="28"
          viewBox="0 0 24 24"
          fill="#ffffff "
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M10 18C11.775 17.9996 13.4988 17.4054 14.897 16.312L19.293 20.708L20.707 19.294L16.311 14.898C17.405 13.4997 17.9996 11.7754 18 10C18 5.589 14.411 2 10 2C5.589 2 2 5.589 2 10C2 14.411 5.589 18 10 18ZM10 4C13.309 4 16 6.691 16 10C16 13.309 13.309 16 10 16C6.691 16 4 13.309 4 10C4 6.691 6.691 4 10 4Z"
            fill="#34D399"
          />
          <path
            d="M11.4118 8.58599C11.7908 8.96599 11.9998 9.46799 11.9998 9.99999H13.9998C14.0007 9.47442 13.8974 8.95389 13.6959 8.46848C13.4944 7.98307 13.1987 7.54242 12.8258 7.17199C11.3118 5.65999 8.68683 5.65999 7.17383 7.17199L8.58583 8.58799C9.34583 7.82999 10.6558 7.83199 11.4118 8.58599Z"
            fill="#34D399"
          />
        </svg>
      </button>
    </div>
  </div>
  <div class="lg:px-36">
    <div class="flex px-5  md:justify-center lg:justify-center flex-wrap lg:mt-2">
      <router-link
        v-for="(item, index) in getListSurah"
        :key="index"
        :to="{ name: 'detailSurah', params: { id: item.number } }"
     class=" w-full mt-2 flex-col sm:w-80  md:w-72 lg:w-64 " >
        <div
          class="bg-white flex justify-between shadow border-2 border-gray-200 p-3 lg:my-2 lg:mx-1"
        >
          <div class="flex w-auto justify-between">
            <div class="flex flex-col w-full">
              <span class="text-lg font-medium">
                {{ item.number }}.{{ item.name.transliteration.id }}
              </span>
              <span class="text-md text-gray-500">
                {{ item.name.translation.id }}
              </span>
            </div>
          </div>
          <div class="w-auto flex justify-end">
            <span class="text-3xl text-green-400 font-arabic">
              {{ item.name.short }}
            </span>
          </div>
        </div>
      </router-link>
    </div>

    <footer class="text-center">&copy;zaenal {{ $store.state.count }}</footer>
  </div>
</template>
<script>
import URL from "@/api/Url.js";
import navbar from "@/components/element/navbar";
export default {
  name: "App",
  components: {
    "nav-bar": navbar,
  },
  async mounted() {
    await this.$store.dispatch("fetchGet", `${URL}surah`);
  },
  data() {
    return {
      search: "",
    };
  },
  watch: {
    search() {
      this.result();
    },
  },
  methods: {
    result() {
      const dataSurah = this.$store.state.listSurah;
      if (this.search === "") {
        return dataSurah;
      }
      let data = [];
      for (let i = 0; i < dataSurah.length; i++) {
        const namaSurah = dataSurah[i].name.transliteration.id.toLowerCase();
        if (namaSurah.includes(this.search.toLowerCase())) {
          data.push(dataSurah[i]);
        }
      }
      if (data.length !== 0) {
        return data;
      }
      return console.log("data tifak di temukan");
    },
  },
  computed: {
    getListSurah() {
      const dataSearch = this.result();
      return dataSearch;
    },
  },
};
</script>


