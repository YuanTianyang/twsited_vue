<template>
  <div id="app" >
    <h1 v-bind:title="message">page-1</h1>
    <h2>{{ msg }} </h2>
    <tr>
      <td>姓名</td><td>年龄</td><td>公司Id</td>
    </tr>
    <tr v-for="passenger in passengers">
      <td>
        {{passenger.name}}
      </td>
      <td>
        {{passenger.age}}
      </td>
      <td>
        {{passenger.companyId}}
      </td>
    </tr>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        message: '页面加载于 ' + new Date().toLocaleString(),
        passengers: [
          {name: 'aaaaa', age: 23, companyId: 1},
          {name: 'bbbb', age: 45, companyId: 12},
          {name: 'wwww', age: 56, companyId: 234},
          {name: 'rrrrrr', age: 67, companyId: 345},
          {name: 'tttttt', age: 23, companyId: 23},
          {name: 'yyyyyy', age: 123, companyId: 345}
        ]
      }
    }
  }
</script>
