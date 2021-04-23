<template>
  <div class="task" :class="{ complete: task.complete }">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ task.title }}</h3>
      <div class="icons">
        <span @click="deleteProject" class="material-icons">delete</span>
        <router-link :to="{ name: 'EditTask', params: { id: task.id }}">
          <span class="material-icons">edit</span>
        </router-link>
        <span @click="toggleComplete" class="material-icons tick">done</span>
      </div>
    </div>
    <div v-if="showDetails" class="details">
      <p>{{ task.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['task'],
  data() {
    return {
      showDetails: false,
      uri: 'http://localhost:8085/tasks/' + this.task.id
    }
  },
  methods: {
    deleteProject() {
      fetch(this.uri, { method: 'DELETE' })
        .then(() => this.$emit('delete', this.task.id))
        .catch(err => console.log(err))
    },
    toggleComplete() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ complete: !this.task.complete })
      }).then(() => {
        this.$emit('complete', this.task.id)
      }).catch(err => console.log(err))
    }
  }
  
};
</script>

<style scoped>
  .task {
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 4px solid #e90074;
  }
  h3 {
    cursor: pointer;
  }
  .actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .material-icons {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
  }
  .material-icons:hover {
    color: #777;
  }
  /* completed projects */
  .task.complete {
    border-left: 4px solid #00ce89;
  }
  .task.complete h3, .task.complete p{
    text-decoration: line-through;
    font-style: italic;
  }
  .task.complete .tick {
    color: #00ce89;
  }
</style>