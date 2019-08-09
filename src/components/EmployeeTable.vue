<template>
  <div>
    <div id="employee-table">
      <p v-if="employees.length < 1" class="empty-table">
        No employees
      </p>
      <table v-if="employees.length > 0" >
        <thead>
          <tr>
            <th>Employee name</th>
            <th>Employee email</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="employee in employees" :key="employee.id">
            <td v-if="editing === employee.id">
              <input type="text" v-model="employee.name">
            </td>
            <td v-else>{{employee.name}}</td>

            <td v-if="editing === employee.id">
              <input type="text" v-model="employee.email" />
            </td>
            <td v-else>{{employee.email}}</td>


            <td v-if="editing === employee.id">
              <button @click="editEmployee(employee)">Save</button>
              <button class="muted-button" @click="editing = null">Cancel</button>
            </td>
            <td v-else>
              <button @click="editMode(employee.id)">Edit</button>
              <button @click="$emit('delete:employee', employee.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'employee-table',
  props: {
    employees: Array,
  },
  data() {
    return {
      editing: null
    }
  },
  methods: {
    editMode(id) {
      this.editing = id
    },

    editEmployee(employee) {
      if (employee.name === '' || employee.email === '') return
      this.$emit('edit:employee', employee.id, employee)
      this.editing = null
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
button {
  margin: 0 0.5rem 0 0;
}
</style>
