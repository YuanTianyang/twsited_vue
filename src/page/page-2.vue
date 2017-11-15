<template>
  <div >
    <h1>page-2</h1>
    <h2 @click="goPage">第二个页面 </h2>
    <input  v-model="input1"/><span>+</span><input  v-model="input2"/><span>={{sumLL}}</span>
    <button v-on:click="multiplyAB"> 相乘后的值为 </button><span>={{multiply}}</span>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App',
        sum: null,
        input1: null,
        input2: null,
        multiply: null
      }
    },
    computed: {
      sumLL () {
        return Number(this.input1) + Number(this.input2)
      }
    },
    methods: {
      goPage () {
        this.$router.push('/page1')
      },
      sumAB () {
        var that = this
        that.sum = parseInt(that.input1) + parseInt(that.input2)
        if (that.sum === null) {
          that.sum = 0
        }
      },
      multiplyAB () {
        var that = this
        that.multiply = parseInt(that.input1) * parseInt(that.input2)
      }
    }
  }
</script>
