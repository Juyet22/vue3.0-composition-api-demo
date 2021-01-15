<template>
  <div class="inputer">
    <h2>TODO LIST</h2>
    <input type="text" v-model="todoContent" @keyup.enter="submit">
    <button @click="submit">Add</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      todoContent: ''
    }
  },
  methods: {
    submit () {
      if (!!this.todoContent) {
        this.$emit('submit', this.todoContent)
      }
      this.todoContent = ''
    }
  }
}
</script>

<style lang="scss" scoped>
.inputer {


  h2 {
    text-align: center;
  }
  input {
    border: 2px solid #333;
    width: 90vw;
    padding: 5px 10px;
    font-size: 24px;
  }
  button {
    background: rgb(56, 211, 250);
    flex: 0 0 60px;
    text-align: center;
    color: #fff;
    font-size: 12px;
    height: 30px;
    line-height: 30px;
    border: none;
    outline: none;
    margin-top: 10px;
  }
}
</style>