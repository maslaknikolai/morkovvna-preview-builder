<template>
  <div class="container">
    <div class="result-wrapper">
      <div
        class="result"
        ref="resultEl"
      >
        <ImageSelect @imageSelected="onImageSelected()" />
        <BrandElements
          v-if="isImageSelected"
          :title="title"
          :titleSize="titleSize"
          :location="location"
          :locationSize="locationSize"
          :rating="Number(rating)"
        />
      </div>
    </div>

    <div class="fields">
      <input type="range" min="0.5" max="5" step="0.5" v-model="rating">

      <input
        placeholder="Title"
        type="text"
        class="field"
        v-model="title"
      />
      <input type="range" min="140" max="320" v-model="titleSize">

      <input
        placeholder="Location"
        type="text"
        class="field"
        v-model="location"
      />

      <input type="range" min="80" max="220" v-model.number="locationSize">

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
import html2canvas from 'html2canvas'

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
    const titleSize = ref(186)
    const location = ref('Canggu')
    const locationSize = ref(130)
    const rating = ref(5)

    const resultEl = ref<HTMLElement | null>(null)

    const saveImage = async () => {
      const canvas = await html2canvas(resultEl.value!);
      canvas.toBlob(blob => {
        // @ts-ignore
        window.saveAs(blob, `${title.value}.png`);
      });
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
      resultEl,
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
  margin: 40px auto;
  width: 320px;
  overflow: hidden;
}

.result-wrapper {
  border: 1px solid #fff;
}

.result {
  width: 1280px;
  height: 1280px;
  zoom: 0.25;
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
  margin-top: 20px;
}

.main-button {
  display: block;
  padding: 10px 20px;
  margin-top: 10px;
  font-size: 20px;
}
</style>
