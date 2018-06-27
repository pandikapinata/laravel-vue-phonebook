<template>
    <div>
        <div class="card">
        <div class="card-body">
            <h4 class="card-title">Phonebook Table </h4>
            <div class="table-responsive">
                <div style="padding-bottom:15px">
                    <button class="btn btn-success my-2 my-sm-0" id="show-modal" @click="openModal()">Create New Contact</button>
                    <form class="form-inline my-2 my-lg-0" style="float:right">
                        <input class="form-control" type="text" placeholder="Search" v-model="searchQuery">
                    </form>
                </div>
                <table class="table">
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>Name</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item, key in temp">
                            <td>{{key+1}}</td>
                            <td>{{item.first_name}} {{item.last_name}}</td>
                            <td>
                                <button type="button" class="btn btn-secondary" @click="openShow(key)"><i class="fa fa-eye" aria-hidden="true"></i></button>
                                <button type="button" class="btn btn-primary" @click="openEdit(key)"><i class="fa fa-pencil-square-o" aria-hidden="true"></i></button>
                                <button type="button" class="btn btn-danger" @click="del(key,item.id)"><i class="fa fa-trash" aria-hidden="true"></i></button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
        </div>

        <Addcontact v-if="showModal" @close="showModal = false">
        <div slot="body">
            <div class="form-group">
                <label for="first_name">First Name</label>
                <input type="text" class="form-control" v-bind:class="{ 'is-invalid': errors.first_name }" id="first_name" placeholder="Enter First Name" v-model="list.first_name">
                <Formerror class="invalid-feedback" v-if="errors.first_name" :errors="errors">{{ errors.first_name[0] }}</Formerror>
            </div>

            <div class="form-group">
                <label for="lastname">Last Name</label>
                <input type="text" class="form-control" id="lastname" placeholder="Enter Last Name" v-model="list.last_name">
            </div>

            <div class="form-group">
                <label for="telp">Phone Number</label>
                <input type="number" class="form-control" v-bind:class="{ 'is-invalid': errors.phone }" id="telp" placeholder="Enter Phone Number" v-model="list.telp">
                <Formerror class="invalid-feedback" v-if="errors.phone" :errors="errors">{{ errors.phone[0] }}</Formerror>
            </div>

            <div class="form-group">
                <label for="email">Email address</label>
                <input type="email" class="form-control" v-bind:class="{ 'is-invalid': errors.email }" id="email" aria-describedby="emailHelp" placeholder="Enter email" v-model="list.email">
                <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
                <Formerror class="invalid-feedback" v-if="errors.email" :errors="errors">{{ errors.email[0] }}</Formerror>
            </div>

        </div>
        <div slot="footer">
            <button type="button" class="btn btn-primary" @click="save()">Save</button>
        </div>
        </Addcontact>

        <Addcontact v-if="detailcontactModal" @close="detailcontactModal = false">
        <h5 slot="header">{{content.first_name}} {{content.last_name}} Contant Details</h5>
        <div slot="body">
            <div class="form-group">
                <label for="firstname">First Name</label>
                <input type="text" class="form-control" id="firstname" placeholder="Enter First Name" v-model="content.first_name" readonly>
            </div>

            <div class="form-group">
                <label for="lastname">Last Name</label>
                <input type="text" class="form-control" id="lastname" placeholder="Enter Last Name" v-model="content.last_name" readonly>
            </div>

            <div class="form-group">
                <label for="telp">Phone Number</label>
                <input type="number" class="form-control" id="telp" placeholder="Enter Phone Number" v-model="content.telp" readonly>
            </div>

            <div class="form-group">
                <label for="email">Email address</label>
                <input type="email" class="form-control"  id="email" aria-describedby="emailHelp" placeholder="Enter email" v-model="content.email" readonly>
            </div>
        </div>
        </Addcontact>

        <Addcontact v-if="editModal" @close="editModal = false">
        <div slot="body">
            <div class="form-group">
                <label for="first_name">First Name</label>
                <input type="text" class="form-control" v-bind:class="{ 'is-invalid': errors.first_name }" id="first_name" placeholder="Enter First Name" v-model="contentUpdate.first_name">
                <Formerror class="invalid-feedback" v-if="errors.first_name" :errors="errors">{{ errors.first_name[0] }}</Formerror>
            </div>

            <div class="form-group">
                <label for="lastname">Last Name</label>
                <input type="text" class="form-control" id="lastname" placeholder="Enter Last Name" v-model="contentUpdate.last_name">
            </div>

            <div class="form-group">
                <label for="telp">Phone Number</label>
                <input type="number" class="form-control" v-bind:class="{ 'is-invalid': errors.telp }" id="telp" placeholder="Enter Phone Number" v-model="contentUpdate.telp">
                <Formerror class="invalid-feedback" v-if="errors.telp" :errors="errors">{{ errors.telp[0] }}</Formerror>
            </div>

            <div class="form-group">
                <label for="email">Email address</label>
                <input type="email" class="form-control" v-bind:class="{ 'is-invalid': errors.email }" id="email" aria-describedby="emailHelp" placeholder="Enter email" v-model="contentUpdate.email">
                <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
                <Formerror class="invalid-feedback" v-if="errors.email" :errors="errors">{{ errors.email[0] }}</Formerror>
            </div>

        </div>
        <div slot="footer">
            <button type="button" class="btn btn-primary" @click="update()">Save changes</button>
        </div>
        </Addcontact>
    </div>
</template>

<script>

import Addcontact from './AddContact';
// import FormError component
import Formerror from './FormErrors.vue';
    export default {
        components:{
            Addcontact, Formerror
        },
        data(){
            return {
                showModal: false,
                detailcontactModal:false,
                editModal: false,

                list:{
                    first_name:'',
                    last_name:'',
                    telp:'',
                    email:''
                },
                content:[],
                contentUpdate:{},
                errors:{},
                lists:{},
                searchQuery:"",
                temp:"",
            }
        },
        watch:{
            searchQuery(){
                if(this.searchQuery.length > 0){
                    this.temp = this.lists.filter((item)=>{
                        return Object.keys(item).some((key)=>{
                            let string = String(item[key])
                            return string.toLowerCase().indexOf(this.searchQuery.toLowerCase())> -1
                        })
                    });
                    // console.log(result)
                }else{
                    this.temp = this.lists
                }
            }

        },
        mounted(){
            axios.post('/getData').
            then((response)=> this.lists= this.temp = response.data)
            .catch(error => {this.errors = error.response.data.errors;
            });
        },

        methods: {
            openModal() {
                this.showModal = true;
            },
            openShow(key) {
                this.content=this.temp[key]
                this.detailcontactModal = true;
            },
            openEdit(key) {
                this.contentUpdate=this.temp[key]
                this.editModal = true;
            },
            save(){
                axios.post('/phonebook', this.$data.list).then((response)=> {
                    this.showModal=false
                    this.lists.push(response.data) //merubah data dan mengambil data dari method di controller
                    this.lists.sort(function(a,b){ //sorting
                        if(a.first_name > b.first_name){
                            return 1;
                        }else if (a.first_name < b.first_name){
                            return -1;
                        }
                    })
                    this.list = ""
                })
                .catch(error => {this.errors = error.response.data.errors;
                });
            },
            update(){
                axios.patch(`/phonebook/${this.contentUpdate.id}`, this.$data.contentUpdate).then((response)=> this.editModal=false)
                .catch(error => {this.errors = error.response.data.errors;
                });
            },
            del(key,id){
                if(confirm("Are you sure?")){
                    axios.delete(`/phonebook/${id}`).
                    then((response)=> this.lists.splice(key,1))
                    .catch(error => {this.errors = error.response.data.errors;
                    });
                }

            }
        }
    }
</script>