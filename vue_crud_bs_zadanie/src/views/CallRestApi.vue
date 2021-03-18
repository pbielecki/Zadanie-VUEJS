<template>
  <div class="home">
    <h1>Zapytania REST API</h1>
    <div class="container jumbotron">
    Tabela z przykładowymi danymi
    <a href="http://dummy.restapiexample.com/api/v1/employees">Link do źródła</a>
    </div>
    <div>
      <table class="table table-bordered">
        <tr>
          <td>ID Pracownika</td>
          <td>Nazwisko</td>
          <td>Wiek</td>
          <td>Wynagrodzenie</td>
        </tr>
        <tr :key="employee.id" v-for="employee in listEmployee">
          <td>{{employee.id}}</td>
          <td>{{employee.employee_name}}</td>
          <td>{{employee.employee_age}}</td>
          <td>{{employee.employee_salary}}</td>
        </tr>
      </table>
      <loading :active.sync="isLoading"
        :can-cancel="true"
        :on-cancel="onCancel"
        :is-full-page="fullPage"></loading>
    </div>
  </div>
</template>

<script>
import { employeeService } from '../service/employeeservice'
import Loading from 'vue-loading-overlay'
import 'vue-loading-overlay/dist/vue-loading.css'

export default {
  name: 'call_rest_api',
  data () {
    return {
      listEmployee: [],
      isLoading: false
    }
  },
  components: {
    Loading
  },
  created () {
    this.loadEmployees()
  },
  methods: {
    loadEmployees () {
      this.isLoading = true
      employeeService.getListEmployees().then((response) => {
        this.listEmployee = response.data
        setTimeout(() => {
          this.isLoading = false
        }, 500)
      })
    }
  }
}
</script>

<style>

</style>
