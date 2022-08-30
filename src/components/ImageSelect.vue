<template>
  <div
    class="image-select"
    :style="{
      backgroundPosition: `0 ${backgroundTopOffset}px`,
      backgroundImage: `url(${selectedImage})`,
    }"
    ref="imageSelectEl"
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

    const onMouseDown = (event: MouseEvent) => {
      isMouseDown.value = true
      mouseStartTopOffset.value = event.pageY
      backgroundStartTopOffset.value = Number(
        window.getComputedStyle(imageSelectEl.value!)
          .backgroundPositionY
          .replace(/[^-\d]/g, '')
      );
    }

    const onMouseUp = () => {
      isMouseDown.value = false
    }

    const onMousemove = (event: MouseEvent) => {
        if (!isMouseDown.value) {
            return
        }

        const move = event.pageY - mouseStartTopOffset.value;
        backgroundTopOffset.value = backgroundStartTopOffset.value + move
    }

    onMounted(() => {
      document.addEventListener('mousedown', onMouseDown)
      document.addEventListener('mouseup', onMouseUp)
      document.addEventListener('mousemove', onMousemove)
    })

    return {
      inputRef,
      onFileSelected,
      selectedImage,
      mouseStartTopOffset,
      backgroundTopOffset,
      imageSelectEl
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
}

.file-input {
  display: none;
}

.select-button {
  height: 50px;
}
</style>
