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
          class="bg-green-600 text-white p-2 rounded-md"
          type="submit"
      >
        Submit
      </button>
    </div>

    <div class="mt-5">

      <input type="search" name="search" id="search" class="border-2 border-grey-500 rounded-md mr-2 p-1"
             v-model="searchQuery" placeholder="Search ...">
      <button @click="searchUserList"
              class="bg-purple-500 text-white p-1.5 rounded-md"
      >
        search
      </button>
    </div>

    <div class="mt-10">
      <Table :userList="userList" @changeAge="handleChangeAge" @deleteRecord="deleteRecord" v-if="userList.length"></Table>
    </div>

    <Modal :heading="myHeading"  :buttons="buttons"  :inputs="inputs" :modalSizes="modalSizes" @alert="Alert">
      <template v-slot:default>
        <div id="modalBody" class="mx-8 my-10 flex flex-col justify-around">
          <input type="text" name="title" id="title" class="py-1 border-b-2 border-grey-500 text-2xl"
                 :value="`${title}`" placeholder="Title">
          <div class="flex justify-start my-3">
            <div class="bg-gray-200 inline-block rounded-full px-1.5 py-1.5">
              <span :class="` ${isEvent ? 'bg-white' : ''} rounded-full px-2 py-0.5`" @click="this.isEvent.toggle()">Event</span>
              <span :class="` ${isEvent ? '' : 'bg-white'} rounded-full px-2 py-0.5 bg-white`"
                    @click="this.isEvent.toggle()">Reminder</span>
            </div>
          </div>
          <div class="flex justify-start my-3">
            <span>Color</span>
            <div class="flex items-center ml-5">
                <span v-for="item in eventColors" :key="item.id"
                      :class="`bg-${item.color} rounded-full w-5 h-5 inline-block mx-1`"></span>
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
                <input class="block w-5/6 border-gray-200 border-2 rounded-lg p-2" type="time" name="startTime"
                       id="startTime" placeholder="20 : 00">
              </div>
              <div class="inline-block w-1/2">
                <label for="endTime">End</label>
                <input class="block w-5/6 border-gray-200 border-2 rounded-lg p-2" type="time" name="endTime"
                       id="endTime">
              </div>
            </div>
          </div>
          <div class="my-5">
            <button v-for="item in buttons" :key="item.index"
                    :class="`bg-${item.color}-500 text-white rounded p-1 mx-2`" @click="$emit(`${item.evt}`)">
              {{ item.label }}
            </button>
          </div>
        </div>
      </template>
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

      isEvent: true,


      searchQuery:"",

      userList: [],

      nameError: false,
      lastnameError: false,
      countryError: false,
      ageError:false,
      emailError:false,


      activeIndex: null,
      editMode: false,

      myHeading:'Create an Event',
      title:'my',

      inputs: [
        {
          label: "Date",
          inputType: "date",
          width: "w-full",
        },
        {
          label: "Start",
          inputType: "time",
          width: "w-full",
        },
        {
          label: "End",
          inputType: "time",
          width: "w-full",
        },
      ],

      eventColors : [
        {
          color : 'red-500',
          id : 1,
        },
        {
          color : 'pink-500',
          id : 2,
        },
        {
          color : 'yellow-500',
          id : 3,
        },
        {
          color : 'green-500',
          id : 4,
        },
        {
          color : 'blue-500',
          id : 5,
        },
        {
          color : 'purple-500',
          id : 6,
        },
      ],

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
        {
          label : 'btn4',
          evt : 'alert' ,
          color: 'yellow',
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

    searchUserList(){

      let filteredArr = this.userList.filter(
          (item) => {
            let itemArr = Object.values(item);
            console.log(itemArr);

            let vvv = itemArr.filter(key => {
              return key.toLowerCase().indexOf(this.searchQuery) != -1;
            })

            if (vvv.length) {
              return true;
            }

      //     console.log(vvv)
      //     console.log(itemArr.includes(vvv))
      // return itemArr.includes(vvv);



      // return item.name.toLowerCase().indexOf(this.searchQuery) != -1 ||
      //     item.email.toLowerCase().indexOf(this.searchQuery) != -1 ||
      //     item.lastname.toLowerCase().indexOf(this.searchQuery) != -1 ||
      //     item.country.toLowerCase().indexOf(this.searchQuery) != -1 ||
      //     item.age.toLowerCase().indexOf(this.searchQuery) != -1 ;
    });

      console.log(filteredArr);
      this.userList = filteredArr;
    },

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

    checkDuplicate() {

      let formDataArr = Object.values(this.formData);

      for (const item in this.userList) {
        let userListItems = Object.values(item);
        for (let i = 0; i < formDataArr.length; i++) {
          if (!formDataArr[i].includes(userListItems[i])) {
            return true
          }
        }
      }
    },

    generateUser() {
      this.nameValidation();
      this.lastnameValidation();
      this.ageValidation();
      this.countryValidation();
      this.emailValidation();
      this.checkDuplicate();
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

          }
          else {
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
      alert('salaaaam')
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







