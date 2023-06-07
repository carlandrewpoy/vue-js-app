<template>
  <div>
   <form @submit.prevent="save">
    <input type="text" v-model="employee.name" id="" placeholder="Employee Name"><br/><br/>
    <input type="text" v-model="employee.address" id="" placeholder="Employee Address"><br/><br/>
    <input type="text" v-model="employee.mobile" id="" placeholder="Employee Mobile Number"><br/><br/>
    <button class="btn btn-secondary" type="submit">Save</button>
   </form>
    <h1>Employee View</h1>
    <table class="table table-striped table-hover">
  <thead class="table-dark">
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Employee Name</th>
      <th scope="col">Address</th>
      <th scope="col">Mobile</th>
      <th scope="col">Option</th>

    </tr>
  </thead>
  <tbody>
    <tr v-for="employee in result" v-bind:key="employee.id">
      <td>{{ employee.id }}</td>
      <td>{{ employee.name }}</td>
      <td>{{ employee.address }}</td>
      <td>{{ employee.mobile }}</td>
      <td>
        <button class="btn btn-warning" @click="edit(employee)">Edit</button>
        <button class="btn btn-danger" @click="remove(employee)">Delete</button>
      </td>
    </tr>
  </tbody>
</table>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
Vue.use(axios)
export default {
  name: 'EmployeeView',
  data () {
    return {
      result: {},
      employee: {
        id: '',
        name: '',
        address: '',
        mobile: ''
      }
    }
  },
  created () {
    this.EmployeeLoad()
    console.log('created')
  },
  mounted () {
    console.log('mounted() called.....')
  },
  methods: {
    EmployeeLoad () {
      axios.get('http://127.0.0.1:8000/api/employees')
        .then(
          ({data}) => {
            this.result = data
            console.log(this.result[0].name)
          }
        )
    },
    save () {
      if (this.employee.id === '') {
        this.saveData()
      } else {
        this.updateData()
      }
    },
    saveData () {
      axios.post('http://127.0.0.1:8000/api/save/', this.employee)
        .then(
          ({data}) => {
            this.employee.id = ''
            this.employee.name = ''
            this.employee.address = ''
            this.employee.mobile = ''
            alert('Added successfully!')
            this.EmployeeLoad()
          }
        )
    },
    edit (employee) {
      this.employee = employee
    },
    updateData () {
      let editrecords = 'http://127.0.0.1:8000/api/update/' + this.employee.id
      axios.put(editrecords, this.employee)
        .then(
          ({data}) => {
            this.employee.id = ''
            this.employee.name = ''
            this.employee.address = ''
            this.employee.mobile = ''
            alert('Update successfully!')
            this.EmployeeLoad()
          }
        )
    },
    remove (employee) {
      let url = 'http://127.0.0.1:8000/api/delete/' + employee.id
      axios.delete(url)
      alert('Delete successfully')
      this.EmployeeLoad()
    }
  }
}
</script>
