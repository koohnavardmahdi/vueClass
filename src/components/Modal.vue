<template>
  <div>

    <div v-if="isModalVisible" class="overlay" @click.self="toggleModal">
      <div id="modalwrapper" class="flex justify-center items-center">
        <div :class="`lg:${modalSizes.width.lg} md:${modalSizes.width.md} sm:${modalSizes.width.sm}  lg:${modalSizes.minHeight.lg} md:${modalSizes.minHeight.md} sm:${modalSizes.minHeight.sm} bg-white shadow-md overflow-auto rounded-lg `">
          <div id="modalHeading" class="flex justify-between mx-8 my-10">
            <h1 class="text-2xl font-bold">{{  heading }}</h1>
            <button @click="toggleModal" class="bg-red-500 rounded-full w-5 h-5 leading-5 text-white text-sm"><i class="fas fa-times"></i></button>
          </div>
          <div id="modalBody" class="mx-8 my-10 flex flex-col justify-around">
            <input type="text" name="title" id="title" class="py-1 border-b-2 border-grey-500 text-2xl" :value="`${title}`" placeholder="Title">
            <div class="flex justify-start my-3">
              <div class="bg-gray-200 inline-block rounded-full px-1.5 py-1.5">
                <span :class="` ${isEvent ? 'bg-white' : ''} rounded-full px-2 py-0.5`" @click="this.isEvent.toggle()" >Event</span>
                <span :class="` ${isEvent ? '' : 'bg-white'} rounded-full px-2 py-0.5 bg-white`" @click="this.isEvent.toggle()" >Reminder</span>
              </div>
            </div>
            <div class="flex justify-start my-3">
              <span>Color</span>
              <div class="flex items-center ml-5">
                <span v-for="item in eventColors" :key="item.id" :class="`bg-${item.color} rounded-full w-5 h-5 inline-block mx-1`"></span>
<!--                <span class="bg-pink-500 rounded-full w-5 h-5 inline-block mx-1"></span>-->
<!--                <span class="bg-yellow-500 rounded-full w-5 h-5 inline-block mx-1"></span>-->
<!--                <span class="bg-green-500 rounded-full w-5 h-5 inline-block mx-1"></span>-->
<!--                <span class="bg-blue-500 rounded-full w-5 h-5 inline-block mx-1"></span>-->
<!--                <span class="bg-purple-500 rounded-full w-5 h-5 inline-block mx-1"></span>-->
              </div>
            </div>
            <div class="my-3">
              <label for="date">Pick a Date</label>
              <input class="block w-full border-gray-200 border-2 rounded-lg p-2" type="date" name="date" id="date">
              <div class="my-3">
                <div class="inline-block w-1/2">
                  <label for="startTime">Start</label>
                  <input class="block w-5/6 border-gray-200 border-2 rounded-lg p-2" type="time" name="startTime" id="startTime" placeholder="20 : 00">
                </div>
                <div class="inline-block w-1/2">
                  <label for="endTime">End</label>
                  <input class="block w-5/6 border-gray-200 border-2 rounded-lg p-2" type="time" name="endTime" id="endTime">
                </div>
              </div>
            </div>
              <slot></slot>
              <div class="my-5">
                <button v-for="item in buttons" :key="item.index" :class="`bg-${item.color}-500 text-white rounded p-1 mx-2`" @click="$emit(`${item.evt}`)">
                  {{item.label}}
                </button>
              </div>

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

  props:{
    heading:{
      type:String,
      required:true,
    },
    title:{
      type:String,
      required:true,
    },
    buttons:{type:Array},
    eventColors:{type:Array},
    inputs:{type:Array},
    modalSizes : {
      type : Object,
      required: true,
    }
  },

  data(){
    return{
      isModalVisible:false,
      isEvent:true,
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



</style>