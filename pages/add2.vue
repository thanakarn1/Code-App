<template>
    <div class="text-center pa-4">
      <v-dialog v-model="dialog" width="auto">
        <v-card v-if="status_save" max-width="400" prepend-icon="mdi-update" title="สถานะการบันทึก">
          <v-card-text>{{ status_save }}</v-card-text>
          <v-card-actions>
            <v-btn class="ms-auto" text @click="dialog = false">Ok</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  
    <v-sheet class="mx-auto" width="300">
      <v-form ref="form">
        <v-text-field v-model="username" label="Username" required></v-text-field>
        <v-text-field v-model="password" label="Password" required></v-text-field>
        <v-text-field v-model="email" label="Email" required></v-text-field>
        <v-text-field v-model="picture" label="Picture" required></v-text-field>
        <v-select v-model="status" :items="items" :rules="[v => !!v || 'Item is required']" label="Status" required></v-select>
  
        <div class="d-flex flex-column">
          <v-btn class="mt-4" color="success" block @click="save">SAVE</v-btn>
          <v-btn class="mt-4" color="error" block @click="reset">Reset Form</v-btn>
        </div>
      </v-form>
    </v-sheet>
  </template>
  
  <script>
  import axios from 'axios'
  
  export default {
    data: () => ({
      status_save: '',
      dialog: false,
      username: '',
      password: '',
      email: '',
      status: '',
      picture: '',
      items: [
        'Yes',
        'No',
      ],
    }),
  
    methods: {
      async save() {
        let students = {
          username: this.username,
          password: this.password,
          email: this.email,
          status: this.status,
          picture: this.picture,
        }
  
        try {
          const response = await axios.post('http://localhost:7000/insert', students)
          this.stds = response.data
          console.log("stds=", this.stds.status)
  
          if (this.stds.status == 'ok') {
            this.status_save = 'บันทึกสำเร็จ'
          } else {
            this.status_save = 'บันทึกไม่สำเร็จ'
          }
  
          this.dialog = true
        } catch (error) {
          console.error('Error:', error)
          this.status_save = 'เกิดข้อผิดพลาด: ' + error.message
          this.dialog = true
        }
      },
  
      reset() {
        this.$refs.form.reset()
      },
    },
  }
  </script>
  
  <style scoped>
  .text-center {
    text-align: center;
  }
  </style>
  