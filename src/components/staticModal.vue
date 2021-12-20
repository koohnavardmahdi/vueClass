<template>
  <div>
    <button v-for="item in buttons" :key="item" @click="$emit(`${item.event}`)" class="bg-green-500 text-white rounded-xl px-5 py-2">
      {{ item.label }}
    </button>
  </div>

</template>

<script>
export default {
  props: {
    buttons: {type: Array}
  },
  name: "staticModal",
  data() {
    return {
    }
  },
  methods: {

  },
  created() {
    console.log(this.buttons)
  }
}
</script>

<style scoped>

</style>