<template>
  <div id="app">
    <h1>Employees</h1>

    <EmployeeForm @add:employee="addEmployee" />
    <br>
    <EmployeeTable 
      :employees = 'employees' 
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from './components/EmployeeTable'
import EmployeeForm from './components/EmployeeForm'
import { constants } from 'crypto';

export default {
  name: 'app',
  components: {
    EmployeeTable,
    EmployeeForm
  },

  data() {
    return {
      employees: []
    }
  },

  methods: {
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        const data = await response.json()
        this.employees = data
      } catch (error) {
        console.error(error)
      }
    },

    async addEmployee(employee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'appliction/json; charset=UTF-8' }
        })
        const data = await response.json()
        this.employees = [...this.employees, data]
      } catch (error) {
        console.error(errror)
      }
    },

    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },        
        })
        const data = await response.json()
        this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
      } catch (error) {
        console.error(error)
      }
    },

    async deleteEmployee(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: "DELETE"
        });
        this.employees = this.employees.filter(employee => employee.id !== id);
      } catch (error) {
        console.error(error);
      }
    }
  },

  mounted() {
    this.getEmployees()
  },
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 30px;
    padding: 60px;
  }
</style>
