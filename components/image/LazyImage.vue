<template>
  <img class="lazy-image" v-lazy="imagePath(path, size)" draggable="false" />
</template>

<script>
import imagePath from "@/utils/imagePath";
export default {
  props: ["path", "size"],
  methods: { imagePath }
};
</script>

<style>
</style>