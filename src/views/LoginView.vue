<template>
    <div>
        <v-card
            class="pa-4 mt-16 mx-auto"
            color="grey lighten-3"
            max-width="400"
            elevation="5"
        >
            <v-text-field
            v-model="eml"
                label="Email"
                placeholder="Enter your email id"
                :rules="[rules.required]"
                outlined
            ></v-text-field>

            <v-text-field
                v-model="pswd"
                label="Password"
                placeholder="Password"
                outlined
                :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                :rules="[rules.required, rules.min]"
                :type="show1 ? 'text' : 'password'"
                @click:append="show1 = !show1"
            ></v-text-field>

            <v-row>
                <v-col></v-col>
                    <v-col>
                            <v-btn color="green" @click="login">Login</v-btn>
                    </v-col>
                <v-col></v-col>
            </v-row>
        </v-card>    
    </div>
</template>

<script>



export default {
    name: 'LoginView',
    data () {
    return {
        eml:'',
        pswd:'',
        show1: false,
        password: 'Password',
        mail:'',
        rules: {
          required: value => !!value || 'Required.',
          min: v => v.length >= 8 && v.length <= 20|| 'Min 8 to Max 20 characters',
          emailMatch: () => (`The email and password you entered don't match`),
        },
      }
    },

    methods: {

        login() {
            this.mail='Mathu@gmail.com' 
            if(this.eml.toLowerCase == this.mail.toLowerCase  && this.pswd == 'Mathu@123'){
                this.$router.push({ path: "/home" });
            
            localStorage.setItem("userMail", this.eml);

            }

        }

    }
  }
</script>

<style>

</style>