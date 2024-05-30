<template>
  <div class="container mt-5">
    <h1>Employee Manager</h1>
    <button class="btn btn-primary mb-3" @click="showModal('add')">Add Employee</button>
    <table class="table">
      <thead>
        <tr>
          <th>EmployeeId</th>
          <th>First Name</th>
          <th>Last Name</th>
          <th>DOB</th>
          <th>Salary</th>
          <th>Address</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" :key="employee.EmployeeId">
          <td>{{ employee.EmployeeId }}</td>
          <td>{{ employee.FirstName }}</td>
          <td>{{ employee.LastName }}</td>
          <td>{{ employee.DOB }}</td>
          <td>{{ employee.Salary }}</td>
          <td>{{ employee.Address }}</td>
          <td>
            <button class="btn btn-warning btn-sm" @click="showModal('edit', employee)">Edit</button>
            <button class="btn btn-danger btn-sm" @click="deleteEmployee(employee.EmployeeId)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Modal -->
    <div class="modal" tabindex="-1" v-if="modalVisible">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ modalType === 'add' ? 'Add Employee' : 'Edit Employee' }}</h5>
            <button type="button" class="btn-close" @click="hideModal"></button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="submitForm">
              <div class="mb-3">
                <label class="form-label">First Name</label>
                <input type="text" class="form-control" v-model="form.FirstName" required>
              </div>
              <div class="mb-3">
                <label class="form-label">Last Name</label>
                <input type="text" class="form-control" v-model="form.LastName" required>
              </div>
              <div class="mb-3">
                <label class="form-label">DOB</label>
                <input type="date" class="form-control" v-model="form.DOB" required>
              </div>
              <div class="mb-3">
                <label class="form-label">Salary</label>
                <input type="number" class="form-control" v-model="form.Salary" required>
              </div>
              <div class="mb-3">
                <label class="form-label">Address</label>
                <input type="text" class="form-control" v-model="form.Address" required>
              </div>
              <button type="submit" class="btn btn-primary">Submit</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      employees: [
        { EmployeeId: 1, FirstName: "John", LastName: "Doe", DOB: "1990-01-01", Salary: "50000", Address: "123 Main St" },
        { EmployeeId: 2, FirstName: "Jane", LastName: "Smith", DOB: "1985-02-15", Salary: "60000", Address: "456 Elm St" },
        { EmployeeId: 3, FirstName: "Alice", LastName: "Johnson", DOB: "1992-03-23", Salary: "70000", Address: "789 Oak St" },
        { EmployeeId: 4, FirstName: "Bob", LastName: "Williams", DOB: "1980-11-11", Salary: "80000", Address: "101 Pine St" },
        { EmployeeId: 5, FirstName: "Charlie", LastName: "Brown", DOB: "1975-05-30", Salary: "90000", Address: "202 Maple St" },
        { EmployeeId: 6, FirstName: "Dave", LastName: "Wilson", DOB: "1995-07-19", Salary: "55000", Address: "303 Cedar St" },
        { EmployeeId: 7, FirstName: "Eve", LastName: "Davis", DOB: "1998-09-10", Salary: "65000", Address: "404 Birch St" },
        { EmployeeId: 8, FirstName: "Frank", LastName: "Garcia", DOB: "1978-12-25", Salary: "75000", Address: "505 Walnut St" },
        { EmployeeId: 9, FirstName: "Grace", LastName: "Martinez", DOB: "1983-04-14", Salary: "85000", Address: "606 Ash St" },
        { EmployeeId: 10, FirstName: "Hank", LastName: "Rodriguez", DOB: "1987-06-22", Salary: "95000", Address: "707 Chestnut St" },
        { EmployeeId: 11, FirstName: "Ivy", LastName: "Lee", DOB: "1990-08-30", Salary: "60000", Address: "808 Hickory St" },
        { EmployeeId: 12, FirstName: "Jack", LastName: "Walker", DOB: "1985-10-05", Salary: "70000", Address: "909 Spruce St" },
        { EmployeeId: 13, FirstName: "Kate", LastName: "Young", DOB: "1972-11-15", Salary: "80000", Address: "1010 Fir St" },
        { EmployeeId: 14, FirstName: "Leo", LastName: "King", DOB: "1981-01-22", Salary: "90000", Address: "1111 Redwood St" },
        { EmployeeId: 15, FirstName: "Mia", LastName: "Scott", DOB: "1994-03-17", Salary: "55000", Address: "1212 Willow St" },
        { EmployeeId: 16, FirstName: "Nick", LastName: "Baker", DOB: "1993-07-09", Salary: "65000", Address: "1313 Poplar St" },
        { EmployeeId: 17, FirstName: "Olivia", LastName: "Harris", DOB: "1979-10-31", Salary: "75000", Address: "1414 Sycamore St" },
        { EmployeeId: 18, FirstName: "Paul", LastName: "Clark", DOB: "1986-02-27", Salary: "85000", Address: "1515 Magnolia St" },
        { EmployeeId: 19, FirstName: "Quinn", LastName: "Lewis", DOB: "1991-05-13", Salary: "95000", Address: "1616 Aspen St" },
        { EmployeeId: 20, FirstName: "Ruth", LastName: "Robinson", DOB: "1984-11-18", Salary: "60000", Address: "1717 Dogwood St" },
      ],
      modalVisible: false,
      modalType: 'add',
      form: {
        EmployeeId: null,
        FirstName: '',
        LastName: '',
        DOB: '',
        Salary: '',
        Address: ''
      }
    };
  },
  methods: {
    showModal(type, employee = {}) {
      this.modalType = type;
      this.modalVisible = true;
      if (type === 'edit') {
        this.form = { ...employee };
      } else {
        this.form = {
          EmployeeId: this.employees.length ? this.employees[this.employees.length - 1].EmployeeId + 1 : 1,
          FirstName: '',
          LastName: '',
          DOB: '',
          Salary: '',
          Address: ''
        };
      }
    },
    hideModal() {
      this.modalVisible = false;
    },
    submitForm() {
      if (this.modalType === 'add') {
        this.employees.push({ ...this.form });
      } else {
        const index = this.employees.findIndex(emp => emp.EmployeeId === this.form.EmployeeId);
        if (index !== -1) {
          this.employees.splice(index, 1, { ...this.form });
        }
      }
      this.hideModal();
    },
    deleteEmployee(id) {
      this.employees = this.employees.filter(emp => emp.EmployeeId !== id);
    }
  }
};
</script>

<style scoped>
.modal {
  display: block;
  background: rgba(0, 0, 0, 0.5);
}
.modal-dialog {
  margin: 10% auto;
}
</style>
