
import { RouterLink } from 'vue-router';

<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                <h4>
                    List of Students👨🏼‍🎓
                    <RouterLink to="/students/create" class="btn btn-primary float-end">Add Student</RouterLink>
                </h4>
            </div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th><strong>ID</strong></th>
                            <th><strong>Name</strong></th>
                            <th><strong>Course</strong></th>
                            <th><strong>Email</strong></th>
                            <th><strong>Phone</strong></th>
                            <th><strong>Created At</strong></th>
                            <th><strong>Action</strong></th>
                        </tr>
                    </thead>
                    <tbody v-if="this.students.length > 0">
                        <tr v-for="(student, index) in this.students" :key="index">
                            <td>{{ student.id }}</td>
                            <td>{{ student.name }}</td>
                            <td>{{ student.course }}</td>
                            <td>{{ student.email }}</td>
                            <td>{{ student.phone }}</td>
                            <td>{{ student.created_at }}</td>
                            <td>
                                <RouterLink :to="{ path: '/students/' + student.id + '/edit' }" class="btn btn-success">Edit
                                </RouterLink>

                                <button type="button" @click="deleteStudent(student.id)"
                                    class="btn btn-danger">Delete</button>
                            </td>
                        </tr>
                    </tbody>
                    <tbody v-else>
                        <tr>
                            <td colspan="7">Loading...Please Wait...</td>
                        </tr>
                    </tbody>

                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'students',
    data() {
        return {
            students: []
        }
    },
    mounted() {
        this.getStudents();
        //axios.get('/api/student')
    },
    methods: {
        getStudents() {
            axios.get('http://localhost:8000/api/students').then(res => {
                this.students = res.data.students
                //console.log(this.students)
            });
        },


        deleteStudent(studentId) {

            if (confirm('Are you sure, you want to delete this student data?')) {
                //console.log(studentId)
                axios.delete(`http://localhost:8000/api/students/${studentId}/delete`).then(res => {

                    alert(res.data.message);
                    this.getStudents();
                })

                    .catch(function (error) {

                        if (error.response) {

                            if (error.response.status == 404) {

                                alert(error.response.data.message);

                            }
                        }
                    });
            }
        },
    },
}
</script>