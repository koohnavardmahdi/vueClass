<template>
  <div>

    <div v-if="isModalVisible" class="overlay" @click.self="toggleModal">
      <div id="modalwrapper" class="flex justify-center items-center">
        <div :class="`lg:${modalSizes.width.lg} md:${modalSizes.width.md} sm:${modalSizes.width.sm}  lg:${modalSizes.minHeight.lg} md:${modalSizes.minHeight.md} sm:${modalSizes.minHeight.sm} bg-white shadow-md overflow-auto`">
          <div id="modalHeading" class="flex justify-between mx-8 my-10">
            <h1 class="text-xl font-bold">{{  title }}</h1>
            <button @click="toggleModal" class="btn">close</button>
          </div>
          <div id="modalBody" class="mx-8 my-10 flex flex-col justify-around" style="height: 2000px">
              <slot></slot>
              <div class="my-5">
                <button v-for="item in buttons" :key="item.index" :class="`bg-${item.color}-500 text-white rounded p-1 mx-2`" @click="$emit(`${item.evt}`)">
                  {{item.label}}
                </button>
              </div>

            <button @click="toggleModal" class="btn">
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

  props:{
    title:{
      type:String,
      required:true,
    },
    buttons:{type:Array},
    modalSizes : {
      type : Object,
      required: true,
    }
  },

  data(){
    return{
      isModalVisible:false,

    }
  },
  methods:{
    toggleModal(){
      this.isModalVisible = !this.isModalVisible
    },
  }
}
</script>


<style lang="postcss" scoped>
.overlay{
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

.btn{
  @apply bg-blue-500 rounded p-1 text-white }

</style>