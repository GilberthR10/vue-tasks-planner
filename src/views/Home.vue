<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div class="" v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from '@/components/SingleProject.vue';
import FilterNav from '@/components/FilterNav.vue';
export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav
  },
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  methods: {
    handleDelete(id){
      this.projects = this.projects.filter(project => project.id !== id);
    },
    handleComplete(id){
      let p = this.projects.find(project => project.id === id);
      p.complete = !p.complete;
    }
  },
  computed: {
    filteredProjects(){
      if (this.current === 'all') {
        return this.projects
      }else if (this.current === 'completed'){
        return this.projects.filter(project => project.complete === true);
      }else {
        return this.projects.filter(project => project.complete === false);
      }
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err.message))
  }
}
</script>
