<template>
  <div>
    <!-- table -->
    <table class="w-full">
      <thead>
      <tr>
        <th class="px-4 py-2 bg-blue-500 text-white text-left">Name</th>
        <th class="px-4 py-2 bg-blue-500 text-white text-left">Last Name</th>
        <th class="px-4 py-2 bg-blue-500 text-white text-left">Email</th>
        <th class="px-4 py-2 bg-blue-500 text-white text-left">Age</th>
        <th class="px-4 py-2 bg-blue-500 text-white text-left">Country</th>
        <th class="px-4 py-2 bg-blue-500 text-white text-left">Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(item, index) in userList" :key="index">
        <td class="px-4 py-2 text-left">
          {{ item.name }}
        </td>
        <td class="px-4 py-2 text-left">
          {{ item.lastname }}
        </td>
        <td class="px-4 py-2 text-left">
          {{ item.email }}
        </td>
        <td class="px-4 py-2 text-left">
          {{ item.age }}
        </td>
        <td class="px-4 py-2 text-left">
          {{ item.country }}
        </td>
        <td class="px-4 py-2 text-left">
          <button @click="handleClick(index)" class="p-2 text-white bg-blue-500 rounded block">Action</button>
          <button @click="deleteRecord(index)" class="p-2 text-white bg-red-500 rounded block">Delete</button>
        </td>

      </tr>
      </tbody>
    </table>
    <!-- /table -->
  </div>
</template>

<script>
export default {

  name: "Table",
  props: {
    userList : {
      type : Array,
      required: true,
    },
},

  methods:{
    handleClick(index){
      this.$emit('changeAge',index)
    },
    deleteRecord(index){
      this.$emit('deleteRecord', index)
    },
  },


};
</script>
<style scoped></style>
