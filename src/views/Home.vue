<template>
  <div class="home">
  <FilterNav @filterChange="current = $event" :current="current" />
   <div v-if="projects.length">
     <div v-for="project in filterProjects" :key="project._id">
       <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
     </div>
   </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue' 

export default {
  name: 'Home',
  components: {SingleProject, FilterNav},
  data(){
    return{
      projects: [],
      current: 'all'
    }
  },
  mounted(){
    fetch('https://restju-f026.restdb.io/rest/vue-db?max=7', {method: 'get',
    headers: {
			'Content-Type': 'application/json; charset=utf-8',
			'x-apikey': '5c9667bddf5d634f46ecae24',
			'cache-control': 'no-cache'}})
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  methods:{
    handleDelete(id){
      this.projects = this.projects.filter((project) => {
        return project._id !== id
      })
    },
    handleComplete(id){
      let p = this.projects.find(project => project._id === id)
      p.complete = !p.complete
    }
  },
  computed: {
    filterProjects(){
      if(this.current === 'completed'){
        return this.projects.filter(project => project.complete)
      }
      if(this.current === 'ongoing'){
        return this.projects.filter(project => !project.complete)
      }
      return this.projects
      
    }
  }
}
</script>
