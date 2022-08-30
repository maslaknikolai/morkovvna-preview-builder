<template>
  <div class="carrots">
    <img
      v-for="(it, i) in rating"
      :key="{i}"
      src="@/assets/carrot.png"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'CarrotsRating',
  props: {
    rating: {
      required: true,
      type: Number
    },
  },
  setup() {
    const inputRef = ref<HTMLElement | null>(null)
    const selectedImage = ref<string>('')

    return {
      inputRef,
      selectedImage,
    }
  }
});
</script>

<style scoped lang="scss">
.carrots {
  display: flex;
  margin-bottom: 20px;
}
</style>
