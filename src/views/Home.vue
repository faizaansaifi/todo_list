<template>
  <div class="home">
    <div v-if="tasks.length">
      <div v-for="task in tasks" :key="task.id">
      <SingleTask :task="task" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleTask from '../components/SingleTask.vue'

export default {
  name: 'Home',
  components: { SingleTask },
  data() {
    return {
      tasks: [],
    };
  },
  mounted() {
    fetch('http://localhost:8085/tasks')
      .then(res => res.json())
      .then(data => this.tasks = data)
      .catch(err => console.log(err))
  },
  methods: {
    handleDelete(id) {
      this.tasks = this.tasks.filter(task => {
        return task.id !== id
      })
    },
    handleComplete(id) {
      let p = this.tasks.find(task => {
        return task.id === id
      })
      p.complete = !p.complete 
      // console.log(p)
    }
  }
}
</script>
