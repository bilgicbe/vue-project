<template>
  <div class="flex justify-between mx-auto sm:p-16 p-4">
    <div class=" max-w-xs bg-white rounded-lg relative hover:bg-gray-900 hover:text-white">
      <div @click="inactive = !inactive" class="px-4 py-1 border-b font-bold text-lg cursor-pointer">
        Select a category
      </div>
      <ul :class="{ hidden: inactive }" class="absolute top-full left-0 w-full bg-white z-50">
        <li class="hover:bg-gray-900 hover:text-white px-4 py-1 cursor-pointer border-b w-full text-black"
          @click="(inactive = true), (filter = category)" v-for="(category, index) in categories"
          :key="index">
          {{ category }}
        </li>
      </ul>
    </div>
    <span class="text-lg font-bold">Category: {{ filter }}</span>
  </div>
  <div class="flex flex-wrap justify-center">
    <template v-for="product in filteredProducts" :key="product.id">
        <Card :data="product" />
    </template>
  </div>
  <div class="flex p-8 justify-center">
    <button @click="changePage(index)" v-for="index in pages" :key="index"
      class="p-1 border-2 border-black m-1 hover:bg-yellow-600 hover:text-gray-200 hover:border-gray-200">
      {{ index }}
    </button>
    <button v-if="next < 3" @click="changePage(next + 1)"
      class="p-1 border-2 m-1 border-black hover:bg-yellow-600 hover:text-gray-200 hover:border-gray-200">
      >>
    </button>
  </div>
</template>

<script>
import axios from "axios";
import Card from "./Card.vue";
export default {
  name: "Products",
  components: {
    Card,
  },
  data() {
    return {
      posts: [],
      next: Number,
      pages: Number,
      inactive: true,
      filter: "All",
    };
  },
  computed: {
    categories() {
      let categoryList = ["All"];
      this.posts.forEach((product) => {
        if (!categoryList.includes(product.catname)) {
          categoryList.push(product.catname);
        }
      });
      return categoryList;
    },
    filteredProducts() {
      if (this.filter !== "All") {
        let filteredProducts = this.posts.filter((product) => {
          console.log(product.catname)
          return product.catname === this.filter;
        })
      return filteredProducts
      } else return this.posts
    }
  },
  methods: {
    changePage(page) {
      axios
        .get(
          `https://tnorthern.xyz/Edgewood-API/public/api/products?page=${page}`
        )
        .then((response) => {
          console.log(response.data);
          this.posts = response.data.data;
          this.next = response.data["current_page"];
          this.pages = response.data["last_page"];
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    this.changePage(1);
  },
};
</script>

<style>
</style>