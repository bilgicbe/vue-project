<template>
  <div class="flex justify-between mx-auto sm:p-16 p-4">
    <div class="max-w-xs bg-white rounded-lg relative hover:bg-gray-900 hover:text-white">
      <div
        @click="inactive = !inactive"
        class="px-4 py-1 border-b font-bold text-lg cursor-pointer"
      >
        Select a category
      </div>
      <ul
        :class="{ hidden: inactive }"
        class="absolute top-full left-0 w-full bg-white z-50"
      >
        <li
          class="
            hover:bg-gray-900 hover:text-white
            px-4
            py-1
            cursor-pointer
            border-b
            w-full
            text-black
          "
          @click="(inactive = true), (filter = category)"
          v-for="(category, index) in categories"
          :key="index"
        >
          {{ category }}
        </li>
      </ul>
    </div>
    <span class="text-lg font-bold">Category: {{ filter }}</span>
  </div>
  <div class="flex flex-wrap justify-center">
    <template v-for="post in posts" :key="post.id">
      <div
        v-if="post.catname === filter || filter === 'All'"
        class="relative sm:w-1/5 card"
      >
        <img
          :src="post.image"
          :alt="post.name"
          class="w-full h-full object-cover"
        />
        <div
          class="
            bg-black bg-opacity-80
            absolute
            left-0
            bottom-0
            w-full
            text-white text-center
          "
        >
          <p>{{ post.name }}<br />{{ post.price }}</p>
        </div>
      </div>
    </template>
  </div>
  <div class="flex p-8 justify-center">
    <button @click="changePage(index)" v-for="index in pages" :key="index" class="p-1 border-2 border-black m-1 hover:bg-yellow-600 hover:text-gray-200 hover:border-gray-200">{{ index }}</button>
    <button
      v-if="next < 3"
      @click="changePage(next + 1)"
      class="p-1 border-2 m-1 border-black hover:bg-yellow-600 hover:text-gray-200 hover:border-gray-200"
    >
      >>
    </button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Products",
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
        console.log("fasd");
        if (!categoryList.includes(product.catname)) {
          categoryList.push(product.catname);
        }
      });
      return categoryList;
    },
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
          this.pages = response.data["last_page"]
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
.card {
  margin: 2%;
  height: 20vw;
}

@media only screen and (max-width: 640px) {
    .card {
        height: 40vw;
        width: 40vw;
    }
}
</style>