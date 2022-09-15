<template>
    <div>
        <div class="form-group">
            <label for="name">Name</label>
            <input type="text" class="form-control" name="name" id="name" placeholder="Enter name" v-model="item.name">
        </div>
        <div class="form-group">
            <label for="contact">Contact</label>
            <input type="text" class="form-control" name="contact" id="contact" placeholder="Enter contact"
                v-model="item.contact">
        </div>
        <button class="btn btn-success btn-block my-2" @click="save">{{ isEditing ? "Update" : "Save" }}</button>
        <div class="col-md-8 offset-md-2" v-if="lists.length > 0">
            <h2 class="text-center">List</h2>
            <ul class="list-group">
                <li class="list-group-item" v-for="item in lists" :key="item.id">
                    {{ item.name }} - {{ item.contact }}
                    <span class="float-right">
                        <button class="btn btn-warning btn-sm mr-2" @click="edit(item)">Edit</button>
                        <button class="btn btn-danger btn-sm mr-2" @click="deleteContact(item.id)">Delete</button>
                    </span>
                </li>
            </ul>
        </div>
    </div>
</template>
  
<script>
export default {
    name: "Directory",
    data() {
        return {
            lists: [],
            item: {
                name: "",
                contact: ""
            },
            temp_id: null,
            isEditing: false
        }
    },
    mounted() {
        this.fetchAll();
    },
    methods: {
        fetchAll() {
            axios.get('/api/contact').then(res => this.lists = res.data)
        },
        save() {
            try {
                let method = axios.post;
                let url = 'api/contact'
                if (this.isEditing) {
                    method = axios.put;
                    url = `/api/contact/${this.temp_id}`
                }

                method(url, this.item).then(res => {
                    this.fetchAll();
                    this.item = {
                        name: "",
                        contact: ""
                    };
                    this.temp_id = null,
                        this.isEditing = false;
                })
            } catch (e) {
                console.log(e)
            }
        },
        edit(contact) {
            this.item = {
                name: contact.name,
                contact: contact.contact
            }
            this.temp_id = contact.id;
            this.isEditing = true
        },
        deleteContact(id) {
            try {
                axios.delete(`api/contact/${id}`).then(res => this.fetchAll())
            } catch (e) {
                console.log(e);
            }
        }
    }
}
</script>
  
<style scoped>

</style>
  