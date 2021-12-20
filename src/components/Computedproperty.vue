<template>
<div>
  <div class="flex justify-center my-10">
    <div :class="`w-80 h-80 rounded-full bg-gradient-to-r ${circleColor}`"></div>
  </div>
  <div class="flex items-center justify-center my-10">
    <button @click="type='from' " class="bg-blue-500 rounded-xl py-2 px-4 text-white hover:bg-blue-900 mx-2">from</button>
    <button @click="type='to' " class="bg-blue-500 rounded-xl py-2 px-4 text-white hover:bg-blue-900 mx-2">to</button>
  </div>
  <div class="grid grid-cols-5 gap-5 place-items-center my-10">
    <button @click="changeColor('red-300')" class="rounded-full bg-red-300 w-16 h-16"></button>
    <button @click="changeColor('yellow-300')" class="rounded-full bg-yellow-300 w-16 h-16"></button>
    <button @click="changeColor('green-300')" class="rounded-full bg-green-300 w-16 h-16"></button>
    <button @click="changeColor('blue-300')" class="rounded-full bg-blue-300 w-16 h-16"></button>
    <button @click="changeColor('purple-300')" class="rounded-full bg-purple-300 w-16 h-16"></button>
    <button @click="changeColor('red-700')" class="rounded-full bg-red-700 w-16 h-16"></button>
    <button @click="changeColor('yellow-700')" class="rounded-full bg-yellow-700 w-16 h-16"></button>
    <button @click="changeColor('green-700')" class="rounded-full bg-green-700 w-16 h-16"></button>
    <button @click="changeColor('blue-700')" class="rounded-full bg-blue-700 w-16 h-16"></button>
    <button @click="changeColor('purple-700')" class="rounded-full bg-purple-700 w-16 h-16"></button>
  </div>
</div>
</template>

<script>
export default {
  name: "Computedproperty",
  data(){
    return{
      type : 'from',
      bgColor : {
        from : 'bg-blue-300',
        to : 'bg-red-300'
      }
    }
  },
  methods : {
    changeColor(color){
      if (this.type === 'from'){
        this.bgColor.from = `from-${color}` ;
      } else {
        this.bgColor.to = `to-${color}`
      }
    },
  },

  computed : {
    circleColor() {
      return this.bgColor.from + ' ' + this.bgColor.to
    }
  }
}
</script>

<style scoped>

</style>