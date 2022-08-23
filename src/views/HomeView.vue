<template>
  <div class="div1">
    <v-col sm="9">
    <v-text-field
            value=""
            v-model="name"
            label="Name"
            counter
            maxlength="30"
            :rules="[rules.required, rules.name]"
            outlined
          ></v-text-field>

    <v-text-field
            value=""
            v-model="email"
            label="Email"
            counter
            maxlength="50"
            :rules="[rules.required, rules.email]"
            outlined
          ></v-text-field>
    </v-col>

   <v-row>
   <v-col sm="5">
   <v-text-field
            value=""
            v-model="pswd_length"
            label="Password length"
            :rules="[rules.required,rules.Password_length]"
            outlined
          ></v-text-field>

    </v-col>
    <v-col>
     <v-btn @click="pswd_generate"
        depressed
        color="primary"
      >Generate password</v-btn>
    </v-col>
    
    </v-row>
    
    <v-col>
    <v-text-field
            value=''
            v-model="generatedPassword"
            label="Generated Password"
            outlined
            readonly
          ></v-text-field>
    </v-col>

    
    <v-container fluid>
    <label>Gender:</label>
    <v-radio-group
     :rules="[rules.required]"
      v-model="row"
      row
    >
      <v-radio
        label="Male"
        color="blue"
        value="radio-1"
      ></v-radio>
      <v-radio
        label="Female"
        color="pink lighten-2"
        value="radio-2"
      ></v-radio>
    </v-radio-group>
  </v-container>
  
  <label>Select address:</label>
  <v-container fluid>
    <v-row>
      <v-col sm="9">
        <v-combobox
          v-model="select"
          :items="items"
          label="State"
          single
          outlined
          dense
          :rules="[rules.required]"
        ></v-combobox>
      </v-col>
    </v-row>
  </v-container>
  
  <label>Hobbies:</label>
   <v-container fluid>
    <v-row>
      <v-col>
      <v-checkbox
        v-model="selected"
        label="Books"
        value="Books"
      ></v-checkbox>
      </v-col>
      <v-col>
      <v-checkbox
        v-model="selected"
        label="Music"
        value="Music"
      ></v-checkbox>
      </v-col>
      <v-col>
      <v-checkbox
        v-model="selected"
        label="Sports"
        value="Sports"
      ></v-checkbox>
      </v-col>
      <v-col>
      <v-checkbox
        v-model="selected"
        label="Movies"
        value="Movies"
      ></v-checkbox>
      </v-col>
   </v-row>
  </v-container>

   <v-row>
    <v-col></v-col>
    <v-col>

      <v-btn 
        :disabled="enablesubmit"
        color="green"
      >Submit</v-btn>
    </v-col>
    <v-col></v-col>
    </v-row>

  
    <HelloWorld /> 
  </div> 
</template>

<script>
  import HelloWorld from '../components/HelloWorld'

  export default {
    name: 'HomeView',

    components: {
      HelloWorld,
    },
    data () {
      return {
        name: '',
        pswd_length: '',
        title: 'Preliminary report',
        email: '',
        generatedPassword: '',
        select: '',
        rules: {
          required: value => !!value || 'Required.',
          counter: value => value.length <= 30 || 'Max 30 characters',
          email: value => {
            const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
            return pattern.test(value) || 'Invalid e-mail.'
          },
          name: value => {
            const pattern = /^[a-zA-Z\s]*$/
            return pattern.test(value) || 'Invalid name'
          },
          Password_length: value => {
            const pattern = /^([8-9]|1[0-9]|20)*$/
            return pattern.test(value) || 'Input must be number and Length must be 8 to 20 characters'
          },
        
        },
        row:'',
        items: [
          'Tamil Nadu',
          'Andhra',
          'Kerala',
          'Karnataka',
        ],
      selected: [''],
      
      } 
    },

    methods: {
      pswd_generate() {
        const Allowed = {
        Uppers: "QWERTYUIOPASDFGHJKLZXCVBNM",
        Lowers: "qwertyuiopasdfghjklzxcvbnm",
        Numbers: "1234567890",
        Symbols: "!@#$%^&*"
      }
      const randomChar = (str) => str.charAt(Math.floor(Math.random() * str.length))
      const generate = (length=this.pswd_length) => { 
        let pwd = "";
        if(8<=this.pswd_length && this.pswd_length<=20){
        pwd += randomChar(Allowed.Uppers);
        pwd += randomChar(Allowed.Lowers); 
        pwd += randomChar(Allowed.Numbers); 
        pwd += randomChar(Allowed.Symbols);
        for (let i = pwd.length; i < length; i++)
            pwd += randomChar(Object.values(Allowed).join(''));
        this.generatedPassword = pwd ;
        }
        else
        {
         this.generatedPassword = ''
        }
      }
      generate()
      }
    },
    computed:{
      enablesubmit() {
        if(this.name.length ==0 || this.email.length ==0 || this.pswd_length.length ==0 || this.generatedPassword.length ==0 || this.select.length ==0){
          return true;
        }
        else{
          return false
          }
        }
    }
  }
</script>

<style>
  .div1 {
  width:500px;
  /* background-color: lightcyan; */
  /* align-content: center;   */
  margin:auto;
  box-sizing: border-box;
  border: solid black 1px;
  padding: 10px;
}
</style>