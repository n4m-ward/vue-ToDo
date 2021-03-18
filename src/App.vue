<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TaskProgress :progress="progress" />
		<NewTask @TaskAdded="addTask"/>
		<TasksGrid 
		@taskDeleted="deleteTask"
		:tasks="tasks"
		/>
	</div>
</template>

<script>
import TaskProgress from './components/TaskProgress'
import TasksGrid from './components/TasksGrid.vue'
import NewTask from './components/NewTask'
export default {
	components: {TasksGrid, NewTask, TaskProgress},
	data(){
		return {
			tasks: [

			]
		}
	},
	watch: {
		tasks:{
			deep: true,
			handler(){
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		}
	},
	methods:{
		addTask(task){
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0
			if(reallyNew) this.tasks.push({name:task.name,pending:task.pending || true}) 
		},
		deleteTask(i){
			this.tasks.splice(i,1)
		}
	},
	computed: {
		progress(){
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			return Math.round(done / total * 100) || 0
		}
	},
	created() {
		this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
	}
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
