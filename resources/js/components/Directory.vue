<template>
    <div>
      <div class="form-group">
          <label for="name">Name</label>
          <input type="text" class="form-control" name="name" id="name" placeholder="Enter name" v-model="item.name">
      </div>
      <div class="form-group">
          <label for="contact">Contact</label>
          <input type="text" class="form-control" name="contact" id="contact" placeholder="Enter contact" v-model="item.contact">
      </div>
      <button class="btn btn-success btn-block my-2" @click="save">Save</button>
      <div class="col-md-8 offset-md-2" v-if="lists.length > 0">
        <h2 class="text-center">List</h2>
        <ul class="list-group">
            <li class="list-group-item" v-for="item in lists" :key="item.id">
                {{ item.name }} - {{ item.contact }}
                <span class="float-right">
                    <button class="btn btn-warning btn-sm mr-2">View</button>
                    <button class="btn btn-danger btn-sm mr-2">Delete</button>
                </span>
            </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script>
    export default {
      name: "Directory",
      data(){
          return {
              lists: [],
              item: {
                  name: "",
                  contact: ""
              },
            }
        },
        mounted() {
            this.fetchAll();
        },
        methods: {
          fetchAll(){
              axios.get('/api/contact').then(res => this.lists = res.data)
          },
          save() {
              try {
              axios.post('/api/contact', this.item)
              // .then(response => {
                  
              // }
              // );
              } catch (e) {
                  console.log(e)
              }
          }
        }
    }
  </script>
  
  <style scoped>
    
  </style>
  