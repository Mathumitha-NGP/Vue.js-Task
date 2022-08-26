<template>
  <div>
    
    
    <v-row>
      <v-col></v-col>
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
              maxlength="50"
              outlined
            ></v-text-field>

            <v-text-field
              value=""
              v-model="tchr"
              label="Teacher"
              counter
              maxlength="50"
              outlined
            ></v-text-field>

          </v-col>

        <v-row>
          <v-col></v-col>
          <v-col>
            <v-btn v-if="!isEdit" depressed color ="green" v-on:click="submitData">SUBMIT</v-btn>
            <v-btn v-else depressed color ="blue" v-on:click="updateClass">UPDATE</v-btn>
          </v-col>
          <v-col></v-col>

        </v-row>
      </v-card>     
      </v-col>
      <v-col cols="5">
        <v-simple-table class="table1">
            <template v-slot:default>
              <table border="1px">
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
                    <th class="text-center">
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
                      <v-btn
                        class="mx-2"
                        fab
                        dark
                        x-small
                        color="cyan"
                        @click=" editData(index)"
                      >
                        <v-icon dark>
                          mdi-pencil
                        </v-icon>
                      </v-btn>
                      
                      <v-btn
                        class="mx-2"
                        fab
                        dark
                        x-small
                        color="black"
                        @click="deleteData(index)"
                      >
                        <v-icon dark>
                          mdi-trash-can
                        </v-icon>
                      </v-btn>
                    </td>
                  </tr>
                </tbody>
              </table>
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
        row:''
      } 
    },
    
    mounted(){
      
      axios.get('http://127.0.0.1:3333/class/')
      .then((response) => {
      this.list=response.data
      })
    },

    methods: {
        
      submitData() {
      axios
        .post("http://127.0.0.1:3333/class/", {
          subject: this.sub,
          credits: this.crdts,
          teacher: this.tchr,
        })
        .then((response) => {
          console.log(response.data);
          // this.list.push(data);
          this.sub = "";
          this.crdts = "";
          this.tchr = "";
        });
      },

      deleteData(id) {
      console.log("delete")
        axios.delete('http://127.0.0.1:3333/class/'+this.list[id].class_id)
      },

      editData(id){
        console.log("edit")
        this.classid = this.list[id].class_id
        this.sub = this.list[id].subject
        this.crdts = this.list[id].credits
        this.tchr = this.list[id].teacher
        this.row=id
        this.isEdit=true
        console.log(this.classid);
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
      })
      this.isEdit=false
      this.sub = "";
      this.crdts = "";
      this.tchr = "";
    },
  }


    
    
    
    
  }

    

</script>

<style>


  .table1 {
    width:auto;
    height:auto;
    margin:20px;
    padding: 10px;
  }

  /* .form1{
    
  } */

</style>