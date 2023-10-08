<template>
  <v-card>
    <v-card-title
      style="
        color: #202214;
        font-size: calc(1rem + 1vmin);
        font-family: 'Product-Sans-Regular', 'Arial';
      "
    >
      Business Name
    </v-card-title>
    <v-card-text>
      <span
        style="
          color: #70757a;
          font-size: calc(0.6rem + 0.75vmin);
          font-family: 'Roboto-Regular', 'Arial';
        "
        >{{ parseFloat(rating).toFixed(1) }}
        <v-icon
          v-for="n in rating"
          size="calc(0.5rem + 1.5vmin)"
          color="#fcbc0c"
          :key="n"
          >mdi-star</v-icon
        >
        <strong
          v-on:click="
            openInNewTab(
              '<Link to Google Reviews of your Business>'
            )
          "
          style="
            color: #1a73e8;
            text-decoration: none;
            font-weight: inherit;
            cursor: pointer;
          "
          >{{ userRatingsCount }} Google-Reviews</strong
        >
        <v-icon size="calc(0.5rem + 1.5vmin)" color="#5F6368"
          >mdi-information-outline</v-icon
        ><br />
        Business Type</span
      >
    </v-card-text>
    <Google
      size="calc(1rem + 1.25vmin)"
      style="position: absolute; bottom: 1vmin; right: 1vmin"
    />
  </v-card>
</template>
<script>
import { gmapApi } from "vue2-google-maps";
import Google from "./assets/svg/Google.vue";
export default {
  name: "Google Review Card",
  components: {
    Google,
  },
  data() {
    return {
      // Google Maps Fetch Variables
      placeId: "<Maps Place ID>",
      placeName: "<Business Name>",
      place: null,

      // Our Fetched Variables
      rating: null,
      userRatingsCount: null,
    };
  },
  methods: {
    openInNewTab(url) {
      window.open(url, "_blank", "noreferrer");
    },
  },
  mounted() {
    this.$gmapApiPromiseLazy().then(async () => {
      if (this.google) {
        this.place = await new this.google.maps.places.Place({
          id: this.placeId,
          requestedLanguage: "de",
        }).fetchFields({ fields: ["rating", "userRatingCount"] });
        this.rating = this.place.place.rating;
        this.userRatingsCount = this.place.place.userRatingCount;
      }
    });
  },
};
</script>
