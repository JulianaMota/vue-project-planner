<template>
     <form @submit.prevent="handleSubmit">
        <label>Title</label>
        <input type="text" v-model="title" required>
        <label>Details:</label>
        <textarea v-model="details" required></textarea>
        <button>Update Project</button>
    </form>
</template>

<script>
export default {
  props: ['id'],
  data(){
    return{
      title: '',
      details: '',
      complete: '',
      uri: 'https://restju-f026.restdb.io/rest/vue-db/' + this.id
    }
  },
  mounted(){
    fetch(this.uri, {method: 'get',
    headers: {
			'Content-Type': 'application/json; charset=utf-8',
			'x-apikey': '5c9667bddf5d634f46ecae24',
			'cache-control': 'no-cache'}}).then(res => res.json())
    .then(data => {
      this.title = data.title
      this.details = data.detail
      this.complete = data.complete
    })
  },
  methods:{
    handleSubmit(){
      let project = {
        title: this.title,
        detail: this.details,
        complete: this.complete
        }
      fetch(this.uri, {
        method: 'put',
        headers: { 
        'Content-Type': 'application/json; charset=utf-8',
			  'x-apikey': '5c9667bddf5d634f46ecae24',
			  'cache-control': 'no-cache'
        },
        body: JSON.stringify(project)
        }).then(() => {
          this.$router.push('/')
        }).catch((err) => console.log(err))     
    }
  }
}
</script>

<style>

</style>