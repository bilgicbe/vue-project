<template>
      <div class="sm:w-1/5 w-[40%] h-0 relative m-[2%] sm:pb-[20%] pb-[40%] overflow-hidden">
  <img :src="data.image" :alt="data.name" class="w-full absolute h-full left-0 top-0 object-cover" @error="replace" />
  <div class=" bg-black bg-opacity-80 absolute left-0 bottom-0 w-full text-white text-center">
    <p>{{ data.name }}<br />{{ data.price }}</p>
  </div>
      </div>
</template>

<script>
import * as faker from "faker";
export default {
  name: "Card",
  props: ["data"],
  methods: {
    replace(e) {
      e.target.src = `${faker.image.image()}`;
    },
  },
};
</script>

<style>
</style>