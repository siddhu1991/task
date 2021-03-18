<template>
    <div>
        <section id="tolist">
            <div class="container">
                <h1>To Do List</h1>
                <div class="row">
                    <div class="col-md-12">
                        <div class="block_todo">
                            <div class="header_blok">To do list <button class="btn btn-primary btn-sm" data-toggle="modal" data-target="#myModal">Add</button></div>
                            <div class="header_inner">
                                <ul class="row" >
                                    
                                    <li class="col-md-4"  v-for="item in todolist" v-bind:key="item.id" >
                                        <div class="cards">
                                            <h6>{{item.id}}</h6>
                                            <p>{{item.name}}</p>
                                            <button v-on:click="delete_todolist(item.id)" class="btn btn-danger btn-xs">Delete</button>
                                        </div>
                                    </li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>


            </div>
        </section>


        <!-- Modal -->
        <div class="modal fade" id="myModal" role="dialog">
            <div class="modal-dialog">

                <!-- Modal content-->
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal">&times;</button>
                        <h4 class="modal-title">Create New To Do</h4>
                    </div>
                    <div class="modal-body">
                        <div class="form-group">
                            <label for="FirstName">First Name:</label>
                            <input type="text" v-model="adding_todo.FirstName" class="form-control" id="FirstName" placeholder="Enter First Name">
                        </div>

                        <div class="form-group">
                            <label for="LastName">Last Name:</label>
                            <input type="text" v-model="adding_todo.LastName" class="form-control" id="LastName" placeholder="Enter Last Name">
                        </div>

                        <div class="form-group">
                            <label for="LastName">Birthdate:</label>
                            <datepicker v-model="adding_todo.Birthdate" class="form-control" id="birthdate" style="background-color:white;" placeholder="Enter Birth Date" />
                        </div>
                        <div class="form-group">
                            <label for="Gender">Gender:</label>
                            <div>
                                <label class="radio-inline"><input type="radio" v-model="adding_todo.Gender" name="Gender" value="Male" checked>Male</label>
                                <label class="radio-inline"><input type="radio" v-model="adding_todo.Gender" name="Gender" value="Female">Female</label>
                            </div>

                        </div>

                        <div class="form-group">
                            <label for="email">Email:</label>
                            <input type="email" class="form-control" v-model="adding_todo.Email" id="email" placeholder="Enter email">
                        </div>

                        <div class="form-group">
                            <label for="phonenumber">Phone Number:</label>
                            <input type="number" class="form-control" v-model="adding_todo.PhoneNumber" id="phonenumber" placeholder="Enter Phone Number">
                        </div>
                        <div class="form-group">
                            <label for="subject">Select Subject:</label>
                            <select class="form-control" v-model="adding_todo.Subject" id="subject">
                                <option>Select Any One</option>
                                <option value="Sub one">Sub one</option>
                                <option value="Sub Two">Sub Two</option>
                                <option value="Sub Three">Sub Three</option>
                                <option value="Sub Four">Sub Four</option>

                            </select>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" v-on:click="addingtodo" class="btn btn-success">Save</button>
                        <button type="button"   class="btn btn-default" data-dismiss="modal">Close</button>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
import Datepicker from 'vue3-datepicker';
import axios from "axios";
    export default {
        name: 'Home',
        components: { Datepicker },
        data() {
            return {
                picked: null,
                adding_todo:  {
                    FirstName: '',
                    LastName: '',
                    Birthdate: '',
                    Gender: '',
                    Email: '',
                    PhoneNumber: '',
                    Subject:''
                },
                todolist: []
            }
        },
        created() {
            this.tolist();
        },
        methods: {
            addingtodo: function () {
                axios.post("https://api.fake.rest/189bf93b-4d78-4f00-86ac-76d87cfccbd1/task/add", this.adding_todo)
                    .then(res => {

                        console.log(res.data);

                        this.adding_todo.FirstName = '';
                        this.adding_todo.LastName = '';
                        this.adding_todo.Birthdate = '';
                        this.adding_todo.Gender = '';
                        this.adding_todo.Email = '';
                        this.adding_todo.PhoneNumber = '';
                        this.adding_todo.Subject = '';

                       
               
                        this.tolist();
                    })
                    .catch(error => {
                        console.log(error)
                        // Manage errors if found any
                    })
            },
            tolist: function () {
                axios.get("https://api.fake.rest/189bf93b-4d78-4f00-86ac-76d87cfccbd1/task/list")
                    .then(res => {
                        this.todolist = res.data.data;
                        console.log(this.todolist);
                       
                    })
                    .catch(error => {
                        console.log(error)
                        // Manage errors if found any
                    })
            },
            delete_todolist: function (id) {
                axios.post("https://api.fake.rest/189bf93b-4d78-4f00-86ac-76d87cfccbd1/task/delete", {id: id})
                    .then(res => {
                        console.log(res.data);
                        this.tolist();
                    })
                    .catch(error => {
                        console.log(error)
                        // Manage errors if found any
                    })
            }
           
        }

    }
</script>