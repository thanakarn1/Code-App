
<template>
    <div>
      <!-- <h1>Hello=> {{ message.rows}}</h1> -->
      <div>{{ upperName }}</div>
    <div>
        <ul>
        <li v-for="st in students" :key="st.id">
          <span>{{ st.id }}</span>
          <span>{{ st.username }}</span>
          <span>{{ st.email }}</span>
          <span>{{ st.status }}</span>
        </li>
      </ul>
      </div>
      <input type="text" :value="name"><br>
    <input type="text" v-model="name"><br>
    <button style="background-color: red;border-radius: 20px;color: white;padding: 5px;" @click="dolist">liststudent</button>
  </div>
  </template>
  <script>
  import axios from 'axios'
  
  export default {          
    data() {
      return { 
        name: "World",
        student: '',
        message: '',
      }
    },
    async created(){
          console.log('init list data');
          const response = await axios.get('http://localhost:7000/list')
          this.message = response.data
          this.students = response.data.rows
      },
    computed: {
      upperName() {return this.name.toUpperCase()}
    },
    methods: {
      async dolist() {
        console.log('SAVE data');
        const response = await axios.get('http://localhost:7000/list')
        this.message = response.data
        this.students = response.data.rows
        console.log(this.message)
      },
    },
  }
  </script>