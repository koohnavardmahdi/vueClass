<template>
  <div>

    <div v-if="isModalVisible" class="overlay" @click.self="toggleModal">
      <div id="modalwrapper" class="flex justify-center items-center">
        <div
            :class="`lg:${modalSizes.width.lg} md:${modalSizes.width.md} sm:${modalSizes.width.sm}  lg:${modalSizes.minHeight.lg} md:${modalSizes.minHeight.md} sm:${modalSizes.minHeight.sm} bg-white shadow-md overflow-auto rounded-lg `">
          <div id="modalHeading" class="flex justify-between mx-8 my-10">
            <h1 class="text-2xl font-bold">{{ heading }}</h1>
            <button @click="toggleModal" class="bg-red-500 rounded-full w-5 h-5 leading-5 text-white text-sm"><i
                class="fas fa-times"></i></button>
          </div>
          <slot></slot>
          <div class="modalFooter">
            <button @click="toggleModal" class="bg-blue-500 text-white p-2 rounded-md">
              confirm
            </button>
          </div>
        </div>
      </div>
    </div>


    <button class="btn" @click="toggleModal">
      open modal
    </button>
  </div>
</template>

<script>
export default {
  name: "Modal",

  props: {
    heading: {
      type: String,
      required: true,
    },
    buttons: {type: Array},
    eventColors: {type: Array},
    inputs: {type: Array},
    modalSizes: {
      type: Object,
      required: true,
    }
  },

  data() {
    return {
      isModalVisible: false,

    }
  },
  methods: {
    toggleModal() {
      this.isModalVisible = !this.isModalVisible
    },
  }
}
</script>


<style lang="postcss" scoped>
.overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}


</style>