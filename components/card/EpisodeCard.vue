<template>
  <div class="card--episode">
    <div class="card--episode__poster">
      <lazy-img
        :path="episode.still_path"
        size="w300"
        :alt="`${episode.name} poster`"
        class="fit-image"
      />
    </div>

    <div class="card--episode__description">
      <h1 class="card--episode__title" aria-label="season title">
        {{ episode.name }}
        <span>{{ episodeRating }}</span>
      </h1>
      <p class="card--episode__episodes mb-2" v-text="airedDate" />

      <p v-show="overview_trucate" class="card--episode__overview">
        {{ overview_trucate.text }}
        <button
          v-if="overview_trucate.length > official_lenght"
          class="btn btn--peek"
          aria-label="read more"
          @click="toggle_more = !toggle_more"
        >see {{ toggleText }}</button>
      </p>
    </div>
  </div>
</template>

<script>
import truncate from "cli-truncate";
import dayjs from "dayjs";
export default {
  props: ["episode", "title"],
  data() {
    return {
      toggle_more: true,
      official_lenght: 120
    };
  },
  computed: {
    overview_trucate() {
      const { overview } = this.$props.episode;
      const { toggle_more, official_lenght } = this;
      if (overview == null) return false;

      return {
        length: overview.length,
        text: truncate(
          overview,
          toggle_more ? official_lenght : overview.length
        )
      };
    },
    toggleText() {
      return this.toggle_more ? "more" : "less";
    },
    episodeRating() {
      return parseFloat(this.episode.vote_average).toFixed(1);
    },
    airedDate() {
      return dayjs(this.episode.air_date).format("MMMM D, YYYY");
    }
  }
};
</script>

<style></style>
