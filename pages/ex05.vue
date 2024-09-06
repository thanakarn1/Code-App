<template>
    <div>
      <h1>Hello {{ name }}</h1>
      <input type="text" :value="name"><br>
      <input type="text" v-model="name"><br>
    </div>
  </template>
   <script>
   export default {
     data() {
       return {
         name: "World ctc",
       }
     },
   }
   </script>