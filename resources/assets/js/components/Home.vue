<template>
    <div>
        <div class="card">
        <div class="card-body">
            <h4 class="card-title">Phonebook Table </h4>
            <div class="table-responsive">
                <div style="padding-bottom:15px">
                    <button class="btn btn-success my-2 my-sm-0" id="show-modal" @click="openModal()">Create New Contact</button>
                    <form class="form-inline my-2 my-lg-0" style="float:right">
                        <input class="form-control mr-sm-2" type="text" placeholder="Search">
                        <button class="btn btn-secondary my-2 my-sm-0" type="submit">Search</button>
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
                        <tr v-for="item, key in lists">
                            <td>{{key+1}}</td>
                            <td>{{item.first_name}} {{item.last_name}}</td>
                            <td>
                                <button type="button" class="btn btn-secondary"><i class="fa fa-eye" aria-hidden="true"></i></button>
                                <button type="button" class="btn btn-primary"><i class="fa fa-pencil-square-o" aria-hidden="true"></i></button>
                                <button type="button" class="btn btn-danger"><i class="fa fa-trash" aria-hidden="true"></i></button>
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
                <label for="firstname">First Name</label>
                <input type="text" class="form-control" v-bind:class="{ 'is-invalid': errors.firstname }" id="firstname" placeholder="Enter First Name" v-model="list.firstname">
                <Formerror class="invalid-feedback" v-if="errors.firstname" :errors="errors">{{ errors.firstname[0] }}</Formerror>
            </div>

            <div class="form-group">
                <label for="lastname">Last Name</label>
                <input type="text" class="form-control" id="lastname" placeholder="Enter Last Name" v-model="list.lastname">
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
            <button type="button" class="btn btn-primary" @click="save()">Save changes</button>
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

                list:{
                    firstname:'',
                    lastname:'',
                    telp:'',
                    email:''
                },
                errors:{},
                lists:{},
            }
        },
        mounted(){
            axios.post('/getData').
            then((response)=> this.lists=response.data)
            .catch(error => {this.errors = error.response.data.errors;
            });
        },

        methods: {
            openModal() {
                this.showModal = true;
            },
            save(){
                axios.post('/phonebook', this.$data.list).then((response)=> this.showModal=false)
                .catch(error => {this.errors = error.response.data.errors;
                });
            }
        }
    }
</script>