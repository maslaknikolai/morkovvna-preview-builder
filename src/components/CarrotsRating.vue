<template>
  <div class="carrots">
    <img
      v-for="(it, i) in fullCarrotsCount"
      :key="i"
      src="@/assets/carrot.png"
    />

    <img
      v-if="hasHalfOfCarrot"
      src="@/assets/carrot.png"
      class="half-carrot"
    />
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'CarrotsRating',
  props: {
    rating: {
      required: true,
      type: Number
    },
  },
  setup(props) {
    const inputRef = ref<HTMLElement | null>(null)
    const selectedImage = ref<string>('')

    const fullCarrotsCount = computed(() => {
      return Math.floor(props.rating)
    })

    const hasHalfOfCarrot = computed(() => Boolean(props.rating - fullCarrotsCount.value))

    return {
      inputRef,
      selectedImage,
      fullCarrotsCount,
      hasHalfOfCarrot,
    }
  }
});
</script>

<style scoped lang="scss">
.carrots {
  display: flex;
  margin-bottom: 20px;
}

.half-carrot {
  clip-path: polygon(0 20%, 0% 100%, 100% 100%);
}
</style>
