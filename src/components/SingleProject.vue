<template>
	<div class="project" :class="{complete: project.complete }">
			<div class="actions">
						<h3 @click="showDetail = !showDetail">{{ project.title }}</h3>
			<div>
				<router-link :to="{ name: 'EditProject', params: { id: project._id }}">
					<span class="material-icons">edit</span>
				</router-link>
				<span @click="deleteProject" class="material-icons">delete</span>
				<span @click="toogleComplete" class="material-icons tick" >done</span>
			</div>
			</div>
			<div class="details" v-if="showDetail">
				<p>{{ project.detail }}</p>
			</div>
	</div>
</template>

<script>
export default {
		props: ['project'],
		data(){
				return{
						showDetail: false,
						url: 'https://restju-f026.restdb.io/rest/vue-db/' + this.project._id

				}
		},
		methods:{
			deleteProject(){
				fetch(this.url, { method: 'DELETE',
				headers: {
				'Content-Type': 'application/json; charset=utf-8',
				'x-apikey': '5c9667bddf5d634f46ecae24',
				'cache-control': 'no-cache'}})
				.then(()=> this.$emit('delete', this.project._id))
				.catch(err => console.log(err.message))
			},
			toogleComplete(){
				fetch(this.url, {
					method: 'PUT',
					headers: { 
						'Content-Type': 'application/json; charset=utf-8',
						'x-apikey': '5c9667bddf5d634f46ecae24',
						'cache-control': 'no-cache' },
					body: JSON.stringify({ title: this.project.title, detail: this.project.detail, complete: !this.project.complete }) 
					}).then(() => {
						this.$emit('complete', this.project._id)
					}).catch(err => console.log(err.message))
			}
		}


}
</script>

<style>
		.project {
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
	.actions{
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
	.project.complete{
		border-left: 4px solid #00ce89;
	}
	.project.complete .tick{
		color: #00ce89;
	}
</style>