<template>
  <div>
    <v-container fluid>
      <v-row>
       <v-col class="div1" cols="4">
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
          
          <v-row>
            <v-col sm="9">
              <v-combobox
                v-model="select"
                :items="items"
                label="Address"
                single
                outlined
                dense
                :rules="[rules.required]"
              ></v-combobox>
            </v-col>
          </v-row>
          
          <v-row>
            <v-col sm="5">
              <v-text-field
              value=""
              v-model="pswd_length"
              label="Password size"
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
      
          
              <v-text-field
                value=''
                v-model="generatedPassword"
                label="Generated Password"
                outlined
                readonly
                ></v-text-field>

      
         
            <label>Gender:</label>
              <v-radio-group
                :rules="[rules.required]"
                v-model="row"
                row
                >
              <v-radio
                label="Male"
                color="blue"
                value="Male"
              ></v-radio>
              <v-radio
                label="Female"
                color="pink lighten-2"
                value="Female"
              ></v-radio>
              </v-radio-group>
          
    
              
          <label>Hobbies:</label>
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
            
          <v-row>
            <v-col></v-col>
            <v-col>

            <v-btn v-if="!isEdit" depressed color ="primary" v-on:click="submitdata()">SUBMIT</v-btn>
            <v-btn v-else depressed color ="primary" v-on:click="updateData(index)">UPDATE</v-btn>
            </v-col>
            <v-col></v-col>
          </v-row>
          </v-col>

        
        <v-col>
          <v-simple-table class="table1">
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-left">
                      id
                    </th>
                    <th class="text-left">
                      name
                    </th>
                    <th class="text-left">
                      email
                    </th>
                    <th class="text-left">
                      address
                    </th>
                    <th class="text-left">
                      gender
                    </th>
                    <th class="text-left">
                      hobbies
                    </th>
                    <th class="text-left">
                      action1
                    </th>
                    <th class="text-left">
                      action2
                    </th>
                  </tr>
                </thead>
                <tbody>
                 <tr v-for="(item, index) in formData" :key="index">
                  <th scope="row">{{ index+1 }}</th>
                    <td>{{ item.name | truncate(10, '...')}}</td>
                    <td>{{ item.email }}</td>
                    <td>{{ item.address }}</td>
                    <td>{{ item.gender }}</td>
                    <td>{{ item.hobbies }}</td>
                    
                    <td><v-btn
                          depressed
                          color="error"
                          @click="deletedata(index)"
                        >
                          Delete
                        </v-btn></td>

                    <td><v-btn
                          depressed
                          color="success"
                          @click="editData(index)"
                        >
                          Edit
                        </v-btn></td>
                  </tr>
                </tbody>
              </template>
          </v-simple-table>
    
          <HelloWorld /> 
        </v-col>
      </v-row>
    </v-container>
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
        email: '',
        generatedPassword: '',
        isEdit : false,
        
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
      
      selected: [],
      formData: [],
          
      } 
    },

    methods: {
      clearForm(){

      this.name = ""
      this.email = ""
      this.select = ""
      this.row = ""
      this.selected = ""
      },
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
        this.generatedPassword = pwd;
        }
        else
        {
         this.generatedPassword = ''
        }

      }
      generate()
      },

      submitdata()
      {
      
        this.formData.push( {
        
            name:this.name,
            email:this.email,
            address:this.select,
            gender:this.row,
            hobbies:this.selected
          
      })
      this.clearForm()
    },

    deletedata(index)
      {
      this.formData.splice(index, 1);
      },


    editData(index){
       console.log(this.formData[index].address)
      this.name = this.formData[index].name
      this.email = this.formData[index].email
      this.select = this.formData[index].address
      this.row = this.formData[index].gender
      this.selected = this.formData[index].hobbies
      this.index = index
      this.isEdit = true

    },

    updateData(index){
    
      this.formData[index].name = this.name
      this.formData[index].email = this.email
      this.formData[index].address = this.select
      this.formData[index].row =  this.row
      this.formData[index].selected = this.selected
      this.isEdit = false

    },
},
    computed:{
      enablesubmit() {
        if(this.name.length ==0 || this.email.length ==0 || this.pswd_length.length ==0 || this.generatedPassword.length ==0 || this.select.length ==0 || this.row ==0 || this.selected ==0){
          return true;
        }
        else{
          return false
          }
        }
    },

     filters: {
        truncate: function (text, length, suffix) {
            if (text.length > length) {
                return text.substring(0, length) + suffix;
            } else {
                return text;
            }
        },
    }
  }
</script>

<style>
  .div1 {
    margin:10px;
    box-sizing: border-box;
    border: solid black 1px;
  }

  .table1 {
    width:auto;
    height:auto;
    margin:10px;
    box-sizing: border-box;
    border: solid black 1px;
    padding: 10px;
  }

</style>