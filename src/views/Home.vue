<template>
  <div class="home">
    <h2>未達成</h2>
    <ul>
      <li v-for="task in uncheckedTasks" :key="task.id">
        <label>
          <input type="checkbox" v-model="task.checked">
          <span>{{ task.name }}</span>
          <button @click="deleteTaskById(task.id)">削除</button>
        </label>
      </li>
    </ul>
    <div class="console">
      <input type="text" v-model="newTaskName">
      <button @click="addTask">追加</button>
    </div>
    <h2>達成済み</h2>
    <ul>
      <li v-for="task in checkedTasks" :key="task.id">
        <label>
          <input type="checkbox" v-model="task.checked">
          <span>{{ task.name }}</span>
          <button @click="deleteTaskById(task.id)">削除</button>
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      tasks: [],
      newTaskName: ''
    }
  },
  computed: {
    checkedTasks () {
      return this.tasks.filter(t => t.checked)
    },
    uncheckedTasks () {
      return this.tasks.filter(t => !t.checked)
    }
  },
  created () {
    this.tasks = JSON.parse(localStorage.getItem('tasks'))
  },
  methods: {
    deleteTaskById (id) {
      const taskIndex = this.tasks.findIndex(t => t.id === id)
      this.tasks.splice(taskIndex, 1)
    },
    addTask () {
      const newId = Math.max.apply(null, this.tasks.map(t => {
        return t.id
      })) + 1
      this.tasks.push({
        id: newId,
        name: this.newTaskName,
        checked: false
      })
    }
  },
  watch: {
    tasks: {
      handler () {
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
      },
      deep: true
    }
  }
}
</script>

<style lang="scss">
.home {
  li {
    list-style: none;
    span {
      cursor: pointer;
      display: inline-block;
      width: 200px;
      &:hover {
        text-decoration: underline;
      }
    }
  }
  .console {
    padding: 0 20px;
  }
}
</style>
