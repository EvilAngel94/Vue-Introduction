<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <!-- Hierin geven we aan wat voor soort event er ge-emit wordt vanuit de child component.
    Wanneer dat gebeurt, dan gaan we de methode aanroepen die ernaast staat -->
    <employee-form @add:employee="addEmployee"/>
    <employee-table
        :employees="employees"
        @delete:employee="deleteEmployee"
        @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from "@/components/EmployeeTable";
import EmployeeForm from "@/components/EmployeeForm";

export default {
  name: 'App',
  components: {
    EmployeeForm,
    EmployeeTable
  },
  data() {
    return {
      employees: [],
    }
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        this.employees = await response.json()
      } catch (error) {
        console.error(error)
      }
    },
    async addEmployee(employee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: {'Content-type': 'application/json; charset=UTF-8'},
        })
        const data = await response.json()
        this.employees = [...this.employees, data]
      } catch (error) {
        console.error(error)
      }
    },
    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: {'Content-type': 'application/json; charset=UTF-8'},
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
  // Function is executed once the project is loaded by the DOM.
  mounted() {
    this.getEmployees()
  }
}
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

button:hover,
button:active,
button:focus {
  background: #32a95d;
  border: 1px solid #32a95d;
}

.small-container {
  max-width: 680px;
}
</style>
