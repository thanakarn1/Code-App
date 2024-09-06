<template >
    <v-app>
      <v-card style="background-image: url(https://images.unsplash.com/photo-1507977840175-a9ab292be020?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D); background-size: cover;">
       
        <v-layout>
          ///
 
          ///
          <v-app-bar
            color="teal-darken-4"
            image="https://64.media.tumblr.com/93cf3f3fada57a7d4bf8187a99bbd76c/tumblr_p5jmzwod2G1s6158vo1_1280.png"
            cover
          >
            <template v-slot:image>
              <v-img
                gradient="to top right"
              ></v-img>
            </template>
  
          
            <v-app-bar-title>HOME PAGE</v-app-bar-title>
            <v-spacer></v-spacer>
            <v-btn icon>
              <v-icon>mdi-magnify</v-icon>
            </v-btn>
            <v-btn icon>
              <v-icon>mdi-heart</v-icon>
            </v-btn>
            <v-btn icon>
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </v-app-bar>
          <v-navigation-drawer
            v-model="drawer"
            :rail="rail"
            permanent
            @click="rail = false"
          >
            <v-list-item
            class="bg-purple-darken-4"
              prepend-avatar="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQmk0eylD-wIbQohodmsPfe-x28I_Pp0yokQw&s"
              title="Thanakarn Nittayachit"
         
            >
              <template v-slot:append>
                <v-btn
                  icon="mdi-chevron-left"
                  variant="text"
                  @click.stop="rail = !rail"
                ></v-btn>
              </template>
            </v-list-item>
            <v-divider></v-divider>
            <v-list density="compact bg-purple" nav>
              <v-list-item
                prepend-icon="mdi-home-city"
                title="หน้าหลัก"
                value="home"
                @click="main"
              ></v-list-item>
              <v-list-item
             
                prepend-icon="mdi-plus"
                size="x-large"
                title="เพิ่มข้อมูล"
                value="add"
                @click="add"
              ></v-list-item>
              <v-list-item
                prepend-icon="mdi-database"
                title="แสดงข้อมูล"
                value="home"
                @click="listdata"
              ></v-list-item>
              <v-list-item
                prepend-icon="mdi-account"
                title="เกี่ยวกับ"
                value="account"
                @click="about"
              ></v-list-item>
              <v-list-item
                prepend-icon="mdi-view-dashboard"
                title="dashboard"
                value="dashboard"
                @click="dashboard"
              ></v-list-item>
             
              
              <v-list-item
                prepend-icon="mdi-close-circle"
                title="LOGOUT"
                value="users"
                @click="logOut"
                
              ></v-list-item>
            </v-list>
          </v-navigation-drawer>
          <!-- <v-main style="width:10px"></v-main> -->
          <v-main style="">
           <NuxtPage></NuxtPage>
          </v-main>
          <!--  -->
  
          <v-bottom-navigation v-model="value" :bg-color="color" mode="shift" class="bg-purple">
            <v-btn>
              <v-icon>mdi-television-play</v-icon>
  
              <span>Video</span>
            </v-btn>
  
            <v-btn>
              <v-icon>mdi-music-note</v-icon>
  
              <span>Music</span>
            </v-btn>
  
            <v-btn>
              <v-icon>mdi-book</v-icon>
  
              <span>Book</span>
            </v-btn>
  
            <v-btn>
              <v-icon>mdi-image</v-icon>
  
              <span>Image</span>
            </v-btn>
          </v-bottom-navigation>
        </v-layout>
      </v-card>
    </v-app>
  </template>
  <script>
  export default {
    data() {
      return {
        drawer: true,
        rail: true,
      };
    },
    methods: {
      main(){
        this.$router.replace("/home");
      },
      listdata(){
        this.$router.replace("/showdata");
      },
      showdata(){
        this.$router.replace("/");
      },
      about(){
        this.$router.replace("/about");
      },
      add(){
        this.$router.replace("/add");
      },
      dashboard(){
        this.$router.replace("/app_mqtt");
      },
      logOut() {
        window.sessionStorage.clear();
        this.$router.replace("/login");
      },
    },
  };
  </script>