<script >
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import IconContact from './icons/IconContact.vue'
import IconSend from './icons/IconSend.vue';
import axios  from 'axios';

  export default{
    components:{
      WelcomeItem,
      DocumentationIcon,
      IconContact,
      IconSend
    },
    data(){
      return{
        firstname:"",
        lastname:"",
        email:"",
        phone:"",
        isSign: false
      }
    },
    methods:{
      haddleSign(){
 
        localStorage.setItem("smartmirrorFirstname", this.firstname);
        localStorage.setItem("smartmirrorLastname", this.lastname);
        localStorage.setItem("smartmirrorEmail", this.email);
        localStorage.setItem("smartmirrorPhone", this.phone);

        this.isSign = true;
      },
      async haddleSync(){
        console.log("start sync");
        const payload = {
          firstname: this.firstname,
          lastname: this.lastname,
          email: this.email,
          phone: this.phone
        }
        const getUrl = await axios.post("http://localhost:8085/testingfit", payload);
        // console.log(getUrl.data);
        // console.log(getUrl.data.url);
        window.location.href = getUrl.data.url
      }
    },
    mounted(){
      if(localStorage.getItem("smartmirrorFirstname")&&localStorage.getItem("smartmirrorLastname") &&localStorage.getItem("smartmirrorEmail")&&localStorage.getItem("smartmirrorPhone")){
        this.firstname = localStorage.getItem("smartmirrorFirstname"); 
        this.lastname = localStorage.getItem("smartmirrorLastname"); 
        this.email = localStorage.getItem("smartmirrorEmail");
        this.phone = localStorage.getItem("smartmirrorPhone");
        this.isSign = true
      }else{
        localStorage.removeItem("smartmirrorFirstname");
        localStorage.removeItem("smartmirrorLastname");
        localStorage.removeItem("smartmirrorEmail");
        localStorage.removeItem("smartmirrorPhone");
        this.isSign = false;
      }
    },

  }
</script>

<template>
  <WelcomeItem  v-if="isSign === false">
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>Register</template>
    To start this project you need to register your name and email, all of your data will be encrypted, if
    you have any question you can contact our team at the buttom.
     <br/>
    <div class="register-data">
      <div class="sign-firstname">
        <input placeholder="Fristname" v-model="firstname" required/>
      </div>
      <div class="sign-lastname">
        <input placeholder="Lastname" v-model="lastname" required/>
      </div>
      <div class="sign-email">
        <input placeholder="Email" v-model="email" required/>
      </div>
      <div class="sign-tel">
        <input placeholder="Number phone" v-model="phone" required/>
      </div>
      <div class="haddle-sign">
        <button class="btn-sign" @click="haddleSign">Sign</button>
      </div>
    </div>

  </WelcomeItem>

  <WelcomeItem  v-if="isSign === true">
    <template #icon>
      <IconSend />
    </template>
    <template #heading>Sync data</template>
      The data from your brand or smart watch that will be sync to our platform are 
      step count, calories, active minutes, heart minutes, distance, 
      user height, user weight, sleep data. <br/>
       read more at <a src="https://developers.google.com/fit/datatypes/aggregate">google fit</a>
     <br/>
 
    <div class="userprofile"  >
      <div class="container-email">
        <h4>Welcome user {{ this.email }}</h4>
      </div>
      <div class="sync-btn-container">
        <br/>
        <button class="sync-button" @click="haddleSync">Start Sync</button>
      </div>
    </div>
  </WelcomeItem>


  <WelcomeItem>
 
    <template #icon>
      <IconContact />
    </template>
    <template #heading>Contact us</template>

  </WelcomeItem>

</template>

<style scoped>

.register-data, .userprofile{
  width: 65%;
  margin-top: 30px;
  margin-bottom: 50px;
  padding-top:20px;
  padding-bottom: 30px;
  border: 1px solid gray;
  text-align: center;
  border-radius: 15px;
}

.register-data > div {
  margin-top: 20px;
}
.register-data > div > input {
  width: 90%;
  height: 40px;
  border-radius: 10px;
  text-indent: 30px;
  border: none;
}

.btn-sign, .sync-button{
  width: 50%;
  height: 30px;
  border: none;
  border-radius: 10px;
  background: rgb(172, 172, 172);

}


</style>

 