<template>
  <div class="container">
    <div class="result-wrapper">
      <div class="result">
        <ImageSelect @imageSelected="onImageSelected()" />
        <BrandElements
          v-if="isImageSelected"
          :title="title"
          :titleSize="titleSize"
          :location="location"
          :locationSize="locationSize"
          :rating="rating"
        />
      </div>
    </div>

    <div class="fields">
      <input
        placeholder="Title"
        type="text"
        class="field"
        v-model="title"
      />

      <input
        placeholder="Title size"
        type="number"
        class="field"
        v-model="titleSize"
      />

      <input
        placeholder="Location size"
        type="number"
        class="field"
        v-model="locationSize"
      />

      <input
        placeholder="Rating"
        type="number"
        class="field"
        max="5"
        min="1"
        v-model="rating"
      />

      <button
        @click="saveImage()"
        class="main-button"
      >
        Save image
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import ImageSelect from './components/ImageSelect.vue';
import BrandElements from './components/BrandElements.vue';

export default defineComponent({
  name: 'App',
  components: {
    ImageSelect,
    BrandElements
  },
  setup() {
    const isImageSelected = ref(false)

    const onImageSelected = () => {
      isImageSelected.value = true
    }

    const title = ref('Milk & Madu')
    const titleSize = ref(83)
    const location = ref('Canggu')
    const locationSize = ref(61)
    const rating = ref(5)

    const saveImage = () => {
      console.log(1);
    }

    return {
      isImageSelected,
      onImageSelected,
      title,
      titleSize,
      location,
      locationSize,
      rating,
      saveImage,
    }
  }
});
</script>

<style>
* {
  box-sizing: border-box;
}

html, body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
  background: #000;
  display: flex;
  justify-content: center;
}

.container {
  margin: 0 auto;
}

.result-wrapper {
  border: 1px solid #fff;
}

.result {
  width: 600px;
  height: 600px;
  position: relative;
}

.fields {
  margin-top: 10px;
}

.field {
  display: block;
  width: 100%;
  padding: 5px;
  font-size: 20px;
}

.main-button {
  padding: 10px 20px;
  margin-top: 10px;
  font-size: 20px;
}
</style>
