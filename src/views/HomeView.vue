<template>
  <div>
    
    
    <v-row>
      <v-col>
         <v-row class="pa-4">
            <v-col>
              <h2>Hi {{this.usrName}}</h2>
              <v-btn
              x-small
              color="grey"
              @click="logout"
              >
              Logout
              </v-btn>
            </v-col>
          </v-row>
      </v-col>
      <v-col>
        <v-card
          class="mt-16 ml-10 mx-auto"
          width="300"
          height="400"
          outlined
          elevation="5"
          color="grey lighten-4"
          >
            <v-col class="div1">
            
              <v-text-field
                value=""
                v-model="sub"
                label="Subject"
                counter
                maxlength="30"
                outlined
              ></v-text-field>

              <v-text-field
                value=""
                v-model="crdts"
                label="Credits"
                counter
                maxlength="1"
                outlined
              ></v-text-field>

              <v-text-field
                value=""
                v-model="tchr"
                label="Teacher"
                counter
                maxlength="25"
                outlined
              ></v-text-field>

            </v-col>

          <v-row>
            <v-col></v-col>
            <v-col>
              <v-btn v-if="!isEdit" color ="green" @click="submitData">SUBMIT</v-btn>
              <v-btn v-else color ="blue" @click="updateClass">UPDATE</v-btn>
            </v-col>
            <v-col></v-col>

          </v-row>
        </v-card>     
        </v-col>
      <v-col cols="5">
        <v-simple-table class="table1">
            <template v-slot:default>
             
                <thead>
                  <tr>
                    <th class="text-left">
                      class_id
                    </th>
                    <th class="text-left">
                      Subject
                    </th>
                    <th class="text-left">
                      Credits
                    </th>
                    <th class="text-left">
                      Teacher
                    </th>
                    <th class="text-center" colspan="2">
                      Action
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item, index) in list" v-bind:key="item.class_id">
                    <td>{{ item.class_id }}</td>
                    <td>{{ item.subject }}</td>
                    <td>{{ item.credits }}</td>
                    <td>{{ item.teacher }}</td>
                    <td>
                      <!-- <v-btn
                        class="mx-2"
                        fab
                        dark
                        x-small
                        color="cyan"
                        @click=" editData(index)"
                      >
                      </v-btn> -->
                        <v-icon dark
                        color="cyan"
                        @click=" editData(index)"
                        >
                          mdi-pencil
                        </v-icon>
                      
                      <!-- <v-btn
                        class="mx-2"
                        fab
                        dark
                        x-small
                        color="black"
                        @click="deleteData(index)"
                      >
                      </v-btn> -->
                        <v-icon 
                          color="black"
                          @click="deleteData(index)">
                          mdi-trash-can
                        </v-icon>
                    </td>
                  </tr>
                </tbody>
              
            </template>
        </v-simple-table>
      <HelloWorld />
      </v-col>
      <v-col></v-col>

    </v-row>
   
   
  </div> 
</template>


<script>
  
  import HelloWorld from '../components/HelloWorld'
  import axios from 'axios'

  export default {
    name: 'HomeView',

    components: {
      HelloWorld,
    },
    data () {
      return {
        student:{},
        id:'',
        sub:'',
        crdts:'',
        tchr:'',
        isEdit:false,
        list: [],
        row:'',
        name:localStorage.getItem("userMail"),
        usrName:''
      } 
    },
    
    mounted(){
      if(!this.name){
        this.$router.push({path:'/'})
      }
      else{
        this.usrName = this.name.slice(0,this.name.indexOf('@')),

        axios.get('http://127.0.0.1:3333/class/')
        .then((response) => {
        this.list=response.data
        })
      }
    },

    methods: {

      getData()
      {
        axios.get('http://127.0.0.1:3333/class/')
        .then((response) => {
        this.list=response.data
        })
      },
        
      submitData() {
      axios
        .post("http://127.0.0.1:3333/class/", {
          subject: this.sub,
          credits: this.crdts,
          teacher: this.tchr,
        })
        .then((response) => {
          console.log(response.data);
          this.sub = "";
          this.crdts = "";
          this.tchr = "";
          this.getData()
        });
        
      },

      deleteData(id) {
      console.log("delete")
        axios.delete('http://127.0.0.1:3333/class/'+this.list[id].class_id)
        .then((response) => {
          console.log(response.data);
          this.getData()
        })
        
      },

      editData(id){
        console.log("edit")
        this.classid = this.list[id].class_id
        this.sub = this.list[id].subject
        this.crdts = this.list[id].credits
        this.tchr = this.list[id].teacher
        this.row=id
        this.isEdit=true
      },

      updateClass(){
      let updateData = {}
      updateData.id = this.classid
      if(this.list[this.row].subject!= this.sub){
        updateData.subject = this.sub
      }
      if(this.list[this.row].credits!= this.crdts){
        updateData.credits = this.crdts
      }
      if(this.list[this.row].teacher!= this.tchr){
        updateData.teacher = this.tchr
      }
      console.log(updateData);
      axios.patch('http://127.0.0.1:3333/class/',updateData)
      .then((response) => {
        console.log(response.data)
        this.getData()
      })
      this.isEdit=false
      this.sub = ""
      this.crdts = ""
      this.tchr = ""
    },

    logout() {
        this.$router.push({ path: "/" });
        localStorage.clear()
    }
  
    }
  }

</script>


<style>
table, th, td{
  border: 2px solid black;
  border-collapse: collapse
}
  .table1 {
    width:auto;
    height:auto;
    margin:20px;
    padding: 10px;
  }

</style>