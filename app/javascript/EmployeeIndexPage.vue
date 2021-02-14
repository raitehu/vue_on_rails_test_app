<template>
  <div id='app'>
    <table>
      <tbody>
        <tr>
          <th>ID</th>
          <th>name</th>
          <th>department</th>
          <th>gender</th>
          <th>actions</th>
        </tr>
        <tr v-for="e in employees" :key="e.id">
          <td><router-link :to="{ name: 'EmployeeDetailPage', params: { id: e.id }}">{{ e.id }}</router-link></td>
          <td>{{ e.name }}</td>
          <td>{{ e.department }}</td>
          <td>{{ e.gender }}</td>
          <td><button @click='deleteTarget = e.id; showModal = true'>Delete</button></td>
        </tr>
      </tbody>
    </table>
    <modal v-if='showModal' @cancel='showModal = false' @ok="deleteEmployee(); showModal = false;">
      <div slot='body'>Are You Sure?</div>
    </modal>
  </div>
</template>

<script>
// scriptタグ内でaxiosが使えるようにする
import axios from 'axios';
import Modal from 'Modal.vue'

export default {
  components: {
    Modal
  },
  data: function() {
    return {
      // employeesは空配列 -> th要素だけ表示する
      employees: [],
      showModal: false,
      deleteTarget: -1,
      errors: ''
    }
  },
  // 仮想DOMが本物のDOMに置き換わる瞬間がmounted
  // cf. https://jp.vuejs.org/v2/guide/instance.html
  mounted() {
    this.updateEmployees();
  },
  methods: {
    updateEmployees: function(){
      axios.get('/api/v1/employees.json')
           .then(response => (this.employees = response.data))
    },
    deleteEmployee: function() {
      if (this.deleteTarget <= 0) {
        console.warn('deleteTarget should be grater than zero');
        return;
      }

      axios.delete(`/api/v1/employees/${this.deleteTarget}`)
           .then(response => {
             this.deleteTarget = -1;
             this.updateEmployees();
           })
           .catch(error => {
             console.error(error);
             if (error.response.data && error.response.data.errors) {
               this.errors = error.response.data.errors;
             }
           });
    }
  }
}
</script>

<style scoped>
p {
  font-size: 2em;
  text-align: center;
}
</style>
