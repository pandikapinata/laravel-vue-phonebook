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
                        <tr>
                            <td><a href="javascript:void(0)">1</a></td>
                            <td>Herman Beck</td>
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

        <Addcontact v-if="showModal" @close="showModal = false"></Addcontact>
    </div>
</template>

<script>

import Addcontact from './AddContact';
    export default {
        components:{
            Addcontact
        },
        data(){
            return {
                showModal: false
            }
        },
        methods: {
            openModal() {
                this.showModal = true;
            }
        }
    }
</script>