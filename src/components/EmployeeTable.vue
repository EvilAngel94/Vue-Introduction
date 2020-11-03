<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>
    <table v-else>
      <thead>
      <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Actions</th>
      </tr>
      </thead>
      <tbody>
      <!-- Generate the rows with all the employee information known -->
      <tr :key="employee.id" v-for="employee in employees">
        <!-- If the editing action is active, we can update the employee name -->
        <td v-if="editing === employee.id">
          <label>
            <input type="text" v-model="employee.name">
          </label>
        </td>
        <td v-else>{{ employee.name }}</td>
        <!-- If the editing action is active, the email can be updated. -->
        <td v-if="editing === employee.id">
          <label>
            <input type="text" v-model="employee.email">
          </label>
        </td>
        <td v-else>{{ employee.email }}</td>
        <td v-if="editing === employee.id">
          <button @click="editEmployee(employee)">Save</button>
          <button class="muted-button" @click="editing = null">Cancel</button>
        </td>
        <td v-else>
          <button @click="editMode(employee.id)">Edit</button>
          <!-- emits an event to the parent component -->
          <button @click="$emit('delete:employee', employee.id)">Delete</button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  // The name used in the App.vue to refer to this.
  name: "employee-table",
  // The values who are passed through from the App.vue (thus parent to child)
  props: {
    employees: Array
  },
  // These values are private to this component. (with the function, we make sure it is component independent)
  data: function() {
    return {
      editing: null
    };
  },
  // These are the methods which can be used within this component
  methods: {
    editMode(id) {
      this.editing = id;
    },
    editEmployee(employee) {
      if (employee.name === "" || employee.email === "") return;
      // With the $emit option, we are going to search for a method which is defined in a parent component.
      // In this case it's in the App.vue -> editEmployee.
      this.$emit("edit:employee", employee.id, employee);
      this.editing = null;
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}

input {
  margin: 0;
}

.empty-table {
  text-align: center;
}
</style>