<template>
  <div
    ref="imageSelectEl"
    class="image-select"
    @mousedown="onMouseDown"
    @mousemove="onMouseMove"
    @touchstart="onTouchStart"
    @touchmove="onTouchMove"
    :style="{
      backgroundPosition: `0 ${backgroundTopOffset}px`,
      backgroundImage: `url(${selectedImage})`,
    }"
  >
    <input
      ref="inputRef"
      type="file"
      accept=".gif,.jpg,.jpeg,.png"
      class="file-input"
      @change="onFileSelected($event)"
    >

    <button
      v-if="!selectedImage"
      class="select-button"
      @click="inputRef.click()"
    >
      Select image
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';

export default defineComponent({
  name: 'ImageSelect',
  props: {
    msg: String,
  },
  emits: ['imageSelected'],
  setup(props, context) {
    const inputRef = ref<HTMLElement | null>(null)
    const selectedImage = ref<string>('')

    const onFileSelected = (event: Event) => {
      const target = event.target as HTMLInputElement;

      const image = target!.files![0];
      const reader = new FileReader();
      reader.onload = () => {
          selectedImage.value = reader.result as string;
          context.emit('imageSelected')
      };
      reader.readAsDataURL(image);

    }

    const imageSelectEl = ref<HTMLElement | null>(null)
    const isMouseDown = ref(false)
    const mouseStartTopOffset = ref(0)
    const backgroundStartTopOffset = ref(0)

    const backgroundTopOffset = ref(0)

    const mouseDown = (pageY: number) => {
      isMouseDown.value = true
      mouseStartTopOffset.value = pageY
      backgroundStartTopOffset.value = Number(
        window.getComputedStyle(imageSelectEl.value!)
          .backgroundPositionY
          .replace(/[^-\d.]/g, '')
      );
    }

    const onMouseDown = (event: MouseEvent) => mouseDown(event.pageY)
    const onTouchStart = (event: TouchEvent) => mouseDown(event.targetTouches[0].pageY)

    const onMouseUp = () => {
      isMouseDown.value = false
    }

    const moveBg = (pageY: number) => {
        if (!isMouseDown.value) {
          return
        }

        const move = pageY - mouseStartTopOffset.value;
        backgroundTopOffset.value = backgroundStartTopOffset.value + move
    }

    const onTouchMove = (event: TouchEvent) => {
      event.preventDefault()
      moveBg(event.targetTouches[0].pageY)
    }

    const onMouseMove = (event: MouseEvent) => moveBg(event.pageY)

    onMounted(() => {
      document.addEventListener('touchend', onMouseUp)
      document.addEventListener('mouseup', onMouseUp)
    })

    return {
      inputRef,
      onFileSelected,
      selectedImage,
      mouseStartTopOffset,
      backgroundTopOffset,
      imageSelectEl,
      onMouseDown,
      onTouchStart,
      onTouchMove,
      onMouseMove,
    }
  }
});
</script>

<style scoped>
.image-select {
  background-color: white;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  background-size: 100%;
  background-repeat: no-repeat;
  width: 100%;
}

.file-input {
  display: none;
}

.select-button {
  font-size: 50px;
  padding: 50px;
}
</style>
