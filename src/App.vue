<template>

  <!-- main wrapper div -->
  <div class="w-10/12 my-0 mx-auto mb-10">
    <!-- fields div -->
    <div class="flex justify-between items-center">
      <div class="grid grid-cols-5 gap-4">
        <div class="text-left">
          <label for="name">Name</label><br />
          <input
              @keyup="nameValidation"
              type="text"
              name="name"
              id="name"
              v-model="formData.name"
              :class="`border-2 ${nameError ? 'border-red-500' : ' border-grey-500' } rounded-md p-1 `"
          />
          <p id="nameError"></p>
        </div>

        <div class="text-left">
          <label for="lastName">Last Name</label><br />
          <input
              @keyup="lastnameValidation"
              type="text"
              name="lastName"
              id="lastName"
              v-model="formData.lastname"
              :class="`border-2 ${lastnameError ? 'border-red-500' : ' border-grey-500' } rounded-md p-1`"
          />
          <p id="lastnameError"></p>
        </div>

        <div class="text-left">
          <label for="email">Email</label><br />
          <input
              type="email"
              name="email"
              id="email"
              v-model="formData.email"
              :class= "`border-2 ${emailError ? 'border-red-500' : ' border-grey-500' } rounded-md p-1`"
          />
          <p id="emailError"></p>
        </div>

        <div class="text-left">
          <label for="age">Age</label><br />
          <input
              @keyup="ageValidation"
              name="age"
              id="age"
              v-model="formData.age"
              :class="`border-2 ${ageError? 'border-red-500' : 'border-grey-500'} rounded-md p-1`"
          />
          <p id="ageError"></p>
        </div>

        <div class="text-left">
          <label for="country">Country</label><br />
          <input
              @keyup="countryValidation"
              type="text"
              name="country"
              id="country"
              v-model="formData.country"
              :class="`border-2 ${countryError ? 'border-red-500' : ' border-grey-500' } rounded-md p-1`"
          />
          <p id="countryError"></p>
        </div>
      </div>
      <button
          @click="generateUser"
          class="border-2 bg-green-600 text-white p-2 rounded-md"
          type="submit"
      >
        Submit
      </button>
    </div>
    <div class="mt-10">
      <Table :userList="userList" @changeAge="handleChangeAge" @deleteRecord="deleteRecord" v-if="userList.length"></Table>
    </div>

    <Modal :title="myTitle" :buttons="buttons" :modalSizes="modalSizes" @alert="Alert">
        this is not a body text
    </Modal>

  </div>
  <!-- /main wrapper div -->



</template>

<script>




import Table from "@/components/Table";
import Modal from "@/components/Modal";


export default {


  name: "App",
  components: {
    Modal,
    Table,
  },

  data() {
    return {

      formData: {
        name: "",
        email: "",
        lastname: "",
        country: "",
        age: "",
      },

      userList: [],

      nameError: false,
      lastnameError: false,
      countryError: false,
      ageError:false,
      emailError:false,


      activeIndex: null,
      editMode: false,

      myTitle:'Modal title',

      buttons : [
        {
          label : 'btn1',
          evt : 'alert' ,
          color: 'red',
        },
        {
          label : 'btn2',
          evt : 'alert' ,
          color: 'blue',
        },
        {
          label : 'btn3',
          evt : 'alert' ,
          color: 'green',
        },
      ],

      modalSizes : {
        width : {
          sm : 'w-48',
          md : 'w-64',
          lg : 'w-96',
        },
        minHeight: {
          sm : 'h-48',
          md : 'h-64',
          lg : 'h-96',
        }
      },


    };
  },

  methods: {

    nameValidation(){

        this.nameError = (this.formData.name === "");
        return  this.nameError;
    },

    lastnameValidation(){

        this.lastnameError = (this.formData.lastname === "");
        return  this.lastnameError;

    },

    countryValidation(){
      this.countryError = (this.formData.country === "");
        return  this.countryError;

    },

    ageValidation(){
      this.ageError = (this.formData.age < 1 || this.formData.age > 100);
      return this.ageError;
    },

    emailValidation(){
      let re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      this.emailError = ! re.test(String(this.formData.email.toLowerCase()));
      return this.emailError;
    },

    generateUser() {
      this.nameValidation();
      this.lastnameValidation();
      this.ageValidation();
      this.countryValidation();
      this.emailValidation();

      this.isDestroyed = !this.isDestroyed;

      if (!(this.nameValidation() || this.lastnameValidation() || this.ageValidation() || this.countryValidation() || this.emailValidation())){
          if (this.activeIndex != null){

            console.log(" no.1" , this.userList[this.activeIndex])
            this.userList.splice(this.activeIndex, 1, this.formData)
            // this.userList[this.activeIndex] = this.formData;
            console.log(" no.2" , this.userList[this.activeIndex])

            this.formData = {
              name: "",
              email: "",
              lastname: "",
              country: "",
              age: "",
            };
            this.activeIndex = null

          }else {
            this.userList.push(this.formData);
            this.formData = {
              name: "",
              email: "",
              lastname: "",
              country: "",
              age: "",
            };
          }

      }
    },

    handleChangeAge(index){
      // this.isDestroyed = !this.isDestroyed
      this.formData = Object.assign({}, this.userList[index])
      this.activeIndex = index
      // this.editMode = true
      // this.formData = item;
      // this.formData.name = item.name;
      // this.formData.lastname = item.lastname;
      // this.formData.email = item.email;
      // this.formData.country = item.country;
      // this.formData.age = item.age;
    },

    deleteRecord(index){
      this.activeIndex = index
      this.userList.splice(this.activeIndex, 1)
    },

    Alert(){
      alert('fuck u')
    },

    mounted() {
      console.log(this.userList)
    }

  },





};


</script>


<style lang="postcss" scoped>


</style>




<!--<template>-->
<!--  <div class="container mx-auto">-->
<!--    <div class="flex justify-between">-->
<!--      <div class="grid grid-cols-5 gap-5 mr-5">-->
<!--        <div class="">-->
<!--          <div class="font-bold text-lg text-left mb-2">Name</div>-->
<!--          <input v-model="formData.name" type="text"-->
<!--                 class="px-4 py-2 rounded-lg border-2 border-gray-500 text-gray-500 w-full"-->
<!--                 placeholder="Enter Your Name">-->
<!--        </div>-->
<!--        <div class="">-->
<!--          <div class="font-bold text-lg text-left mb-2">Last Name</div>-->
<!--          <input v-model="formData.lastName" type="text"-->
<!--                 class="px-4 py-2 rounded-lg border-2 border-gray-500 text-gray-500 w-full"-->
<!--                 placeholder="Enter Your Last Name">-->
<!--        </div>-->
<!--        <div class="">-->
<!--          <div class="font-bold text-lg text-left mb-2">Email</div>-->
<!--          <input v-model="formData.email" @keyup="emailValidation" type="text"-->
<!--                 :class="`px-4 py-2 rounded-lg border-2 ${emailError ? 'border-red-500' : 'border-gray-500'} text-gray-500 w-full`"-->
<!--                 placeholder="Enter Your Email">-->
<!--        </div>-->
<!--        <div class="">-->
<!--          <div class="font-bold text-lg text-left mb-2">Country</div>-->
<!--          <input v-model="formData.country" type="text"-->
<!--                 class="px-4 py-2 rounded-lg border-2 border-gray-500 text-gray-500 w-full"-->
<!--                 placeholder="Enter Your Country">-->
<!--        </div>-->
<!--        <div class="">-->
<!--          <div class="font-bold text-lg text-left mb-2">Age</div>-->
<!--          <input v-model="formData.age" type="text"-->
<!--                 :class="`px-4 py-2 rounded-lg border-2 ${ageError ? 'border-red-500' : 'border-gray-500'} text-gray-500 w-full`"-->
<!--                 placeholder="Enter Your Country">-->
<!--        </div>-->
<!--      </div>-->
<!--      <div @click.self="handleAlert" class="flex items-end w-80 bg-red-500" >-->
<!--        <button @click="generateUser"-->
<!--                :class="`text-white ${userList.length > 0 ? 'bg-red-500 hover:bg-red-800' : 'bg-green-500 hover:bg-green-800'} px-4 py-2`">-->
<!--          Add-->
<!--        </button>-->
<!--      </div>-->
<!--    </div>-->
<!--    <staticModal :buttons="modalButtons"-->
<!--                 @alertError="handleAlert">-->
<!--      <template>-->

<!--      </template>-->
<!--    </staticModal>-->
<!--    <div class="" v-html="dynamicText"></div>-->

<!--  </div>-->

<!--</template>-->

<!--<script>-->
<!--import staticModal from "./components/staticModal";-->

<!--export default {-->
<!--  name: 'App',-->
<!--  components: {-->

<!--    staticModal-->
<!--  },-->
<!--  data() {-->
<!--    return {-->
<!--      dynamicText: `<div class="bg-green-500 text-white rounded-full px-4 py-2 w-32 h-32">test</div>`,-->
<!--      modalButtons: [-->
<!--        {-->
<!--          label: 'Handle',-->
<!--          event: 'alertError'-->
<!--        },-->
<!--        {-->
<!--          label: 'remove',-->
<!--          event: 'alertError'-->
<!--        },-->
<!--        {-->
<!--          label: 'add',-->
<!--          event: 'alertError'-->
<!--        },-->
<!--      ],-->
<!--      formData: {-->
<!--        name: '',-->
<!--        lastName: '',-->
<!--        email: '',-->
<!--        country: '',-->
<!--        age: '',-->
<!--      },-->
<!--      userList: [],-->
<!--      ageError: false,-->
<!--      emailError: false,-->
<!--    }-->
<!--  },-->
<!--  methods: {-->
<!--    emailValidation() {-->
<!--      let re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;-->
<!--      this.emailError = !re.test(String(this.formData.email).toLowerCase())-->
<!--      return !re.test(String(this.formData.email).toLowerCase())-->
<!--    },-->
<!--    ageValidation() {-->
<!--      this.ageError = !(this.formData.age < 1 || this.formData.age > 100)-->
<!--      return !(this.formData.age < 1 || this.formData.age > 100);-->
<!--    },-->
<!--    generateUser() {-->
<!--      if (!this.emailValidation() && this.ageValidation()) {-->
<!--        this.userList.push(this.formData)-->
<!--        this.formData = {name: '', lastName: '', email: '', country: ''}-->
<!--      }-->
<!--    },-->
<!--    changeAge (item) {-->
<!--      this.formData.age = item-->
<!--      console.log(item)-->
<!--    },-->
<!--    handleAlert() {-->
<!--      alert('request done')-->
<!--    }-->
<!--  },-->
<!--}-->
<!--</script>-->

<!--<style>-->
<!--#app {-->
<!--  font-family: Avenir, Helvetica, Arial, sans-serif;-->
<!--  -webkit-font-smoothing: antialiased;-->
<!--  -moz-osx-font-smoothing: grayscale;-->
<!--  text-align: center;-->
<!--  color: #2c3e50;-->
<!--  margin-top: 60px;-->
<!--}-->
<!--</style>-->







