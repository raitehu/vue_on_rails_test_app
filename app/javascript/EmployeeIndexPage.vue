<template>
  <div id='app'>
    <table>
      <tbody>
        <tr>
          <th>ID</th>
          <th>name</th>
          <th>department</th>
          <th>gender</th>
        </tr>
        <tr v-for="e in employees" :key="e.id">
          <td><router-link :to="{ name: 'EmployeeDetailPage', params: { id: e.id }}">{{ e.id }}</router-link></td>
          <td>{{ e.name }}</td>
          <td>{{ e.department }}</td>
          <td>{{ e.gender }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// scriptタグ内でaxiosが使えるようにする
import axios from 'axios'

export default {
  data: function() {
    return {
      // employeesは空配列 -> th要素だけ表示する
      employees: []
    }
  },
  // 仮想DOMが本物のDOMに置き換わる瞬間がmounted
  // cf. https://jp.vuejs.org/v2/guide/instance.html
  mounted() {
    axios.get('/api/v1/employees')
         .then(response => (this.employees = response.data))
  }
}
</script>

<style scoped>
p {
  font-size: 2em;
  text-align: center;
}
</style>
