<template>
  <div class="card--primary" style="--box-shadow: var(--primary-color)">
    <div class="card--primary__img">
      <lazy-img :path="tv_show.poster_path" size="w185" :alt="tv_show.name" draggable="false" />
    </div>

    <div class="card__actions">
      <CardHeartButton :data="tv_show" type="tv" />
    </div>

    <div class="card--primary__body">
      <div class="badge badge--primary" v-text="tv_show.vote_average"></div>
      <h1 class="card--primary__title" v-text="truncateTitle"></h1>

      <div class="card--primary__genre">
        <span
          v-for="genre in tv_show.genre_ids.slice(0, 2)"
          :key="genre"
          v-text="extractName(genre, genres)"
        />
      </div>
      <p class="card--primary__date" v-text="airedDate"></p>
    </div>
    <nuxt-link
      class="card__link"
      :to="{
        name: 'view-tv-id',
        params: { id: parseLink(tv_show.name, tv_show.id, true) }
      }"
    />
  </div>
</template>

<script>
import { mdiHeartOutline, mdiHeart } from "@mdi/js";
import imagePath from "@/utils/imagePath";
import parseLink from "@/utils/parseLink";
import findProperties from "@/utils/findProperties";
import dayjs from "dayjs";
import CardHeartButton from "@/components/button/CardHeartButton";
import cliTruncate from "cli-truncate";

export default {
  props: ["tv_show"],
  data() {
    return {
      icons: {
        heart: mdiHeartOutline,
        heartFull: mdiHeart
      }
    };
  },
  components: { CardHeartButton },
  computed: {
    genres() {
      return this.$store.getters["getTVGenres"];
    },
    airedDate() {
      return dayjs(this.tv_show.first_air_date).format("YYYY");
    },
    truncateTitle() {
      return cliTruncate(this.tv_show.name, 40, { position: "end" });
    }
  },
  methods: {
    imagePath,
    parseLink,
    extractName(value, genres) {
      const val = findProperties(genres, "id", value);
      if (val === undefined) return "";
      return val.name;
    }
  }
};
</script>

<style></style>
