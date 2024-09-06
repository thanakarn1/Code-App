<template>
  <v-responsive  max-height="1000" style="margin: 60px;margin-left: 20px; border-radius: 30px;border: 2px solid white;">
    <v-app>
      <v-app-bar :elevation="2"  class="bg-purple-darken-4">
        <template v-slot:prepend>
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
        </template>

      <v-app-bar-title> Email: {{ email }}      Username: {{ username }}</v-app-bar-title>
    
      </v-app-bar>

      <v-app>
    <v-container-fluid>
     
      <v-row>
        <v-col>
          <v-card class="w-100  card2 ">
            <v-card-title primary-title>
              <div>
                <h1 class=" text-center pt-3 text-purple">Welcome to Home Page</h1>
                <div></div>
              </div>
            </v-card-title>
            <v-img  style="backdrop-filter: blur(10px);

  background: linear-gradient(#141e30, #243b52);
  background: transparent;
  border: 2px solid rgba(255, 255, 255, .2);
  border-radius: 30px;
  box-shadow: 0 0 10px rgba(0,0,0,0.2);"
              src="https://wallpapercat.com/w/full/0/5/a/90024-3840x2160-desktop-4k-alan-walker-background.jpg"        
            ></v-img>
           
           
          </v-card>
        </v-col>
      </v-row>
      
    </v-container-fluid>
  </v-app>
    </v-app>
  </v-responsive>
</template>
<style scoped>
.card2{
  background-color:white;
  padding-top: 50px;
  color: white;

}
</style>
<script>
import axios from "axios";
export default {
  data: () => ({
    id: '',
      username: '',
      email: '',
      status: '',
      passwd: '',
      picture: '',
  }),
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.replace("/login");
    },
  },
  mounted() {
    let user = window.sessionStorage.getItem("email");
    console.log("user=", user);
    if (!user) {
      this.$router.replace("/login");
    }
  },
  async created() {
   
   let user = window.sessionStorage.getItem("email");
   const response = await axios.get("http://localhost:7000/listid?email="+ user);
   let data = response.data;
   console.log('user=',data)
   this.email=data.row.email
   this.username=data.row.username

 },
};

</script>
