<template>
  <v-data-table

   style="backdrop-filter: blur(10px);
    width: 900px;
  margin: 0;
  padding: 0;
  background: linear-gradient(#141e30, #243b52);
  background: transparent;
  border: 2px solid rgba(255, 255, 255, .2);
  border-radius: 30px;
  box-shadow: 0 0 10px rgba(0,0,0,0.2);
  color: white;margin:60px; margin-left: 140px; "
   :search="search"
    :headers="headers"
    :items="desserts"
    :sort-by="[{ key: 'id', order: 'asc' }]"
  >
  
    <template v-slot:top>
      <v-toolbar style="backdrop-filter: blur(10px);
    width: 900px;
  margin: 0;
  padding: 0;
  background: linear-gradient(#141e30, #243b52);
  background: transparent;
  color: white;
  border-radius: 30px;
  box-shadow: 0 0 10px rgba(0,0,0,0.2);"
        flat
      >
        <v-toolbar-title>{{ member }}</v-toolbar-title>
        <v-divider
      
          class="mx-4"
          inset
          vertical
        ></v-divider> 

<!-- search -->

     <v-text-field

          v-model="search"
          label="Search"
          prepend-inner-icon="mdi-magnify"
          variant="outlined"
          hide-details
          single-line
        ></v-text-field>
        
<!-- end search -->

        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ props }">
            <v-btn
         
             
              dark
              v-bind="props"
            >
              เพิ่มข้อมูลใหม่
            </v-btn>
          </template>

    

          <v-card style=" backdrop-filter: blur(10px);
    width: 600px;
  margin: 0;
  padding: 0;
  background: linear-gradient(#141e30, #243b52);
  background: transparent;
  border: 2px solid rgba(255, 255, 255, .2);
  border-radius: 30px;
  box-shadow: 0 0 10px rgba(0,0,0,0.2);
  color: white;margin:60px; margin-left: 310px;">
            
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.id"
                      label="ID"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.username"
                      label="usersname"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.password"
                      label="password"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.email"
                      label="email"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.status"
                      label="status"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.picture"
                      label="picture"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        class="me-2"
        size="small"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        size="small"
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
</template>
<script>
import axios from 'axios'
export default {
  data: () => ({
      search: '',
      member : 'รายชื่อสมาชิก',
    dialog: false,
    dialogDelete: false,
    headers: [
      // {
      //     title: 'id',
      //   align: 'start',
      //   sortable: false,
      //   key: 'id',
      // },
      { title: 'ID', align:'start', sortable: false, key: 'id',},
      { title: 'username', key: 'username' },
      { title: 'email', key: 'email' },
      { title: 'password', key: 'password' },
      { title: 'status', key: 'status' },
      { title: 'picture', key: 'picture'},
      { title: 'Actions', key: 'actions', sortable: false },
    ],
    desserts: [],
    students:[],
    editedIndex: -1,
    editedItem: {
      id: '',
      username: '',
      email: '',
      status: '',
      password: '',
      picture: '',
    },
    defaultItem: {
      id: '',
      username: '',
      email: '',
      status: '',
      password: '',
      picture: '',
    },
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'เพิ่มสมาชิก' : 'แก้ไขสมาชิก'
    },
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },
  mounted() {
        let user = window.sessionStorage.getItem('email')
        console.log('user=' , user) 
        if (!user){
          this.$router.replace('/login')
        }
      },

  async created() {
    this.initialize()
      console.log('init list data')
      const response = await axios.get('http://localhost:7000/list')
      this.desserts = response.data.rows
      this.initialize()
  },

  methods: {
    initialize () {},

    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

//  delete

    deleteItem (item) {
      // this.editedIndex = this.desserts.indexOf(item)
      // console.log('delete',this.editedIndex)
      console.log('item',item.id)
      this.id = item.id
      // this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    async deleteItemConfirm () {
      console.log('confirm')
      const response = await axios.get(' http://localhost:7000/delete?id='+this.id)
      this.desserts = response.data.rows
     
      // this.desserts.splice(this.editedIndex, 1)
      this.closeDelete()
    },

 

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

// end delete

// insert and update
    async save () {
      // this.stds = response.data
      if (this.editedIndex === -1) {
        console.log(this.editedItem)
      const response = await axios.post('http://localhost:7000/insert',this.editedItem)
      this.desserts = response.data.rows
      } else {
        console.log('update item', this.editedItem)
        const response = await axios.post('http://localhost:7000/update', this.editedItem)
        this.desserts = response.data.rows
        // console.log('save data', Object.assign(this.students[this.editedIndex], this.editedItem))
        // let std = Object.assign(this.students[this.editedIndex], this.editedItem)
        // const response = await axios.post('http://localhost:7000/update',std)
        // this.stds = response.data
        // console.log("stds=",this.stds)
      }

//end insert and update

      this.close()
    },
  },
}
</script>
