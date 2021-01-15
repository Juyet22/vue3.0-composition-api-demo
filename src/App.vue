<template>
  <div class="main">
    
    <Inputer @submit="submit" />
    <Status @change="onStatusChanged" />
    <TodoList
      :list="onShowList"
      @toggle="toggleStatus"
      @delete="onItemDelete"
    />
  </div>
</template>

<script>
import Inputer from './components/Inputer'
import TodoList from './components/TodoList'
import Status from './components/Status'

// 按需引入
import { reactive, computed, toRefs ,isRef, watch, onMounted, ref} from '@vue/composition-api'

export default {
  components: {
    Status,
    Inputer,
    TodoList
  },

  // Vue 3.0 组合式API
  setup (props,context) {

    const data = reactive({
      todoList: [],
      showingStatus: 'all',
      onShowList: computed(() => {
        if (data.showingStatus === 'all') {
          return data.todoList
        } else if (data.showingStatus === 'completed') {
          return data.todoList.filter(({ completed }) => completed)
        } else if (data.showingStatus === 'uncompleted') {
          return data.todoList.filter(({ completed }) => !completed)
        }
      })
    })

    
    function submit (content) {
      data.todoList.push({
        completed: false,
        content,
        id: parseInt(Math.random(0, 1) * 100000)
      })
    }
    function onStatusChanged (status) {
      data.showingStatus = status
    }
    function toggleStatus ({ isChecked, id }) {
      data.todoList.forEach(item => {
        if (item.id === id) {
          item.completed = isChecked
        }
      })
    }
    function onItemDelete (id) {
      let index = 0
      data.todoList.forEach((item, i) => {
        if (item.id === id) {
          index = i
        }
      })
      data.todoList.splice(index, 1)
    }

    return {
      ...toRefs(data),  // toRefs将reactive对象转换为普通对象
      submit,
      onStatusChanged,
      toggleStatus,
      onItemDelete,
    }
  },

  // Vue 2.x

  // data () {
  //   return {
  //     todoList: [],
  //     showingStatus: 'all'
  //   }
  // },
  // computed: {
  //   onShowList () {
  //     if (this.showingStatus === 'all') {
  //       return this.todoList
  //     } else if (this.showingStatus === 'completed') {
  //       return this.todoList.filter(({ completed }) => completed)
  //     } else if (this.showingStatus === 'uncompleted') {
  //       return this.todoList.filter(({ completed }) => !completed)
  //     }
  //   }
  // },
  // methods: {
  //   submit (content) {
  //     this.todoList.push({
  //       completed: false,
  //       content,
  //       id: parseInt(Math.random(0, 1) * 100000)
  //     })
  //   },
  //   onStatusChanged (status) {
  //     this.showingStatus = status
  //   },
  //   toggleStatus ({ isChecked, id }) {
  //     this.todoList.forEach(item => {
  //       if (item.id === id) {
  //         item.completed = isChecked
  //       }
  //     })
  //   },
  //   onItemDelete (id) {
  //     let index = 0
  //     this.todoList.forEach((item, i) => {
  //       if (item.id === id) {
  //         index = i
  //       }
  //     })
  //     this.todoList.splice(index, 1)
  //   }
  // }
}
</script>
