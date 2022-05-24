<template>
  <div class="page">
    <div class="image">
      <div class="second-image">
        <img class="w-100" src="~/assets/images/bgqueen.jpg" />
        <div v-if="!checkForm" @click="signForm">
          <img class="box-sign" src="~/assets/images/banner01.gif"/>
        </div>
      </div>
    
    </div>
    <v-form 
      ref="form" 
      lazy-validation 
      v-if="checkForm && !checkSubmit"  
      v-model="valid"
      class="sign-form"
    >
        <v-text-field
          v-model="p_name"
          :rules="nameRules"
          label="ชื่อ"
          required
          thai_engLanguage
          maxlength="30"
        ></v-text-field>

          <v-text-field
          v-model="p_lastname"
          :rules="lastnameRules"
          label="นามสกุล"
          required
          thai_engLanguage
          maxlength="30"
        ></v-text-field>
    
      <div class="btn-submit">
        <img src="~/assets/images/banner01.gif"  @click="submit"/>
      </div>
    </v-form>
    <div v-if="checkSubmit" class="box-detail">
      <h2 class="style-title">ด้วยเกล้าด้วยกระหม่อมขอเดชะ ข้าพระพุทธเจ้า</h2> 
      <div class="style-xs">
        <h2>ด้วยเกล้าด้วยกระหม่อมขอเดชะ</h2> 
        <h2>ข้าพระพุทธเจ้า</h2> 
      </div>
      <h3 class="style-name">{{item.name}} {{item.lastname}}</h3> 
      <p class="style-number">ผู้ร่วมลงนามลำดับที่ : {{item.counter}}</p>
      <div class="box-footer">
          <a href="https://www.cgd.go.th/">
            <v-btn
              rounded
               large
              class="btn btn-home"
              >
              กลับหน้าหลัก
            </v-btn>
          </a>
          <v-btn
            rounded
             large
            class="btn btn-print"
            @click="printDivContent"
          >
          พิมพ์คำถวายพระพร
        </v-btn>    
      </div>
    </div>
  </div> 
</template>

<script>
import moment from 'moment'
export default {
  name: 'IndexPage',
  data: () => ({
    valid: true,
    checkForm: false,
    checkSubmit: false,
    p_name: '',
    nameRules: [
      v => !!v || 'กรุณากรอกข้อมูล',
      //  v => (v && v.length <= 30) || 'ห้ามกรอกเกิน 30 ตัวอักษร',
      v =>/^[a-zA-Zก-ฮะ-์\s]+$/.test(v) ||` ห้ามกรอกอักขระพิเศษ เเละตัวเลข`
    ],
    p_lastname: '',
    lastname: '',
      lastnameRules: [
      v => !!v || 'กรุณากรอกข้อมูล',
      // v => (v && v.length <= 30) || 'ห้ามกรอกเกิน 30 ตัวอักษร',
      v =>/^[a-zA-Zก-ฮะ-์\s]+$/.test(v) ||` ห้ามกรอกอักขระพิเศษ เเละตัวเลข`
    ],
    p_festival: 'forking',
    regis_date: moment().format('YYYY-MM-DD HH:mm:ss'),
    browser: '',
    device: '',
    item: {
      name: '',
      lastname: '',
      counter: ''
    }
  }),
    created(){
      this.getDeviceType();
      this.getBrowserDetect();
    },
    methods: {
      submit: async function(){
      
        if(this.p_name && this.p_lastname && this.valid){
          this.checkSubmit = true
          let fd = {
            "name" : this.p_name,
            "lastname" : this.p_lastname,
            "p_festival" : this.p_festival,
            "regis_date" : this.regis_date,
            "browser" : this.browser,
            "device" : this.device,
          }
          const response = await this.$axios.$post('/api/for-king', fd) 
          this.item.name = response.data.name
          this.item.lastname = response.data.lastname
          this.item.counter = response.data.counter
        }else{
          this.$refs.form.validate()
        }
      },
      signForm(v){
         this.checkForm = !this.checkForm;
      },
   
      getBrowserDetect(){        
        let userAgent = navigator.userAgent;
        let browserName;   
        if(userAgent.match(/chrome|chromium|crios/i)){
          browserName = "chrome";
        }else if(userAgent.match(/firefox|fxios/i)){
          browserName = "firefox";
        } else if(userAgent.match(/safari/i)){
          browserName = "safari";
        }else if(userAgent.match(/opr\//i)){
          browserName = "opera";
        } else if(userAgent.match(/edg/i)){
          browserName = "edge";
        }else{
          browserName="No browser detection";
        }
        this.browser = browserName
      },
      
      getDeviceType(){
          let ua = navigator.userAgent;
          let deviceName;
          if (/(tablet|ipad|playbook|silk)|(android(?!.*mobi))/i.test(ua)) {
            deviceName = "tablet";
          }else if( /Mobile|iP(hone|od)|Android|BlackBerry|IEMobile|Kindle|Silk-Accelerated|(hpw|web)OS|Opera M(obi|ini)/.test(ua)){
            deviceName = "mobile";
          }else{
            deviceName = "desktop";
          }
          this.device = deviceName  
      },
      printDivContent () {
        window.print()
      },
    }
}
</script>
<style scoped>
  .page{
    text-align: center;
  }
  .box-sign{
    cursor: pointer;
    /* position: absolute;
    bottom: 10px;
    margin-left: auto;
    margin-right: auto;
    left: 0;
    right: 0;
    text-align: center; */
  }
  .image{
    position: relative;
  }
  .sign-form{
    max-width: 700px;
    display: inline-block;
    padding: 1rem;
    width: 100%;
    text-align: left;
  }
  .btn-submit{
    text-align: center;
  }
  .btn-submit img{
    cursor: pointer;
  }
  ::v-deep .v-messages__message{
    color: red!important;
  }
   .btn-home{
    background-color: #7c4b0b!important;
    color: white;
    margin-right: 0.5rem;
    font-weight: 300;
  }
   .btn-print{
    background-color: white!important;
    border: 1px solid #7c4b0b!important;
    color: #7c4b0b;
    font-weight: 400;
   
  }
  .style-title{
    font-weight: 400;
    margin: 2rem 0 1rem 0;
    color: white;
  }
  .style-xs{
    display: none;
  }
  .style-name{
   margin: 1rem 0 1rem 0;
   font-weight: 400;
   color: white;
  }

  .style-number{
    color: #e7c576;
    font-size: 20px;
  }
  
  .box-footer{
    margin: 2rem 0;
  }
  .check-text{
    flex: 1 1 auto;
    font-size: 12px;
    min-height: 14px;
    min-width: 1px;
    position: relative;
    text-align: left;
    color: red;
  }
  @media print {
    .box-footer{
      display: none;
    }
  }
  a{
   text-decoration: none;
  }
  .image .w-100{
    width: 100%;
    max-width: 600px;
  }

  ::v-deep .theme--light.v-input {
    color: #e7c576;
  }

  
  ::v-deep .theme--light.v-input:hover {
    color: #e7c576!important;
  }

  

  ::v-deep .theme--light.v-label {
    color: #e7c576;
  }
  ::v-deep .theme--light.v-input input{
      color: white;
  }
  ::v-deep .theme--light.v-text-field > .v-input__control > .v-input__slot:before {
    border-color: #e7c576;
  }

  @media only screen and (max-width: 600px) {
    .image .w-100{
      width: 100%;
    }
    .box-sign{
      position: relative;
      cursor: pointer;
      margin-left: auto;
      margin-right: auto;
      left: 0;
      right: 0;
      text-align: center;
    }
    .style-title{
     display: none;
    }
    .style-xs{
      display: block;
      color: white;
      margin: 2rem 0 1rem 0;
    }
    .style-xs h2{
     font-weight: 400;
    }
  }
   @media only screen and (max-width: 600px) {
    .btn-print{
      display: none;
    }
   }
    @media only screen and (max-width: 960px) {
    .btn-print{
      display: none;
    }
  }
</style>
