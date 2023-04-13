<template>
  <div class="container mt-5">
      <div class="row">
        <div class="mx-auto col-10 col-md-8 col-lg-6">
          <Header @toggle-add-task="toggleAddTask" title="Project Task Manager" :showAddTask="showAddTask" />
          
          <router-view :showAddTask="showAddTask"></router-view>
          <Footer />
        </div>
      </div>
  </div>
</template>

<script>
import Header from './components/Header'
import Footer from './components/Footer'

export default { 
  name: 'App', 
  components: {
    Header,
    Footer
  },
  data() {
    return {
      tasks: [],
      showAddTask: false
    }
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask 
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

</style>
