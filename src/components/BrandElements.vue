<template>
  <div class="brand">
    <CarrotsRating :rating="rating" />

    <div
      class="title"
      :style="{
        fontSize: `${titleSize}px`
      }"
    >
      {{title}}
    </div>

    <div
      class="location"
      :style="{
        fontSize: `${locationSize}px`
      }"
    >
      <img
        src="@/assets/location.png"
        class="location-icon"
      >
      {{location}}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import CarrotsRating from './CarrotsRating.vue';

export default defineComponent({
    name: "BrandElements",
    props: {
        title: {
            required: true,
            type: String
        },
        titleSize: {
            required: true,
            type: Number
        },
        location: {
            required: true,
            type: String
        },
        locationSize: {
            required: true,
            type: Number
        },
        rating: {
            required: true,
            type: Number
        },
    },
    setup() {
        const inputRef = ref<HTMLElement | null>(null);
        const selectedImage = ref<string>("");
        return {
            inputRef,
            selectedImage,
        };
    },
    components: { CarrotsRating }
});
</script>

<style scoped lang="scss">
.brand {
  pointer-events: none;
  user-select: none;
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 60%;
  background: rgba(#000, .5);
  padding: 80px;
}

.title {
  font-family: 'Fredoka One', cursive;
  font-size: 83px;
  color: #fff;
  text-shadow: 0px 4px 0 black;
  width: 100%;
  background: transparent;
  border: 0;
}

.location {
  font-family: 'Fredoka One', cursive;
  font-size: 83px;
  color: #fff;
  text-shadow: 0px 4px 0 black;
  width: 100%;
  background: transparent;
  border: 0;
}

.location-icon {
  position: relative;
  top: 6px;
}
</style>
