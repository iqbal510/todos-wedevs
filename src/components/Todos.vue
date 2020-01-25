<template>
	<section class="todoapp">
		<header class="header">
			<h1>todos</h1>
			<input type="text" class="new-todo" placeholder="What needs to be done?" v-model="newTodo" @keyup.enter="addTodo">
			
		</header>
		<div class="main">
			<input type="checkbox" class="toggle-all" v-model="alldone">
			<ul class="todo-list">
				<li class="todo " v-for="todo in filterdTodos" :class="{completed:todo.completed, editing: todo === editing}" >
					<div class="view">
						<input type="checkbox" v-model="todo.completed" class="toggle">
						<label @dblclick="editTodo(todo)">{{todo.name}} </label>
						<button class="destroy"@click.prevent="deleteTodo(todo)"></button>
					</div>
					<input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit"@blur="doneEdit" v-focus="todo === editing">
				</li>
			</ul>
		</div>
		<footer class="footer"v-show="hasTodos">
			<span class="todo-count"><strong>{{remaining}}</strong> items left</span>
			<ul class="filters">
				<li><a href="#" :class="{selected:filter === 'all'}" @click.prevent="filter ='all'">All</a></li>

				<li><a href="#" :class="{selected:filter === 'todo'}" @click.prevent="filter ='todo'">Active</a></li>

				<li><a href="#" :class="{selected:filter === 'done'}" @click.prevent="filter ='done'">Completed</a></li>
			</ul>
			<button class="clear-completed" v-show="complede" @click.prevent="deletecomplede">Clear completed</button>
		</footer>

	</section>
</template>


<script >
import Vue from 'vue'
	export default{
		data (){
			return{
				alldone:false,
				todos:[{
					name:'first',
					completed:false
				}],
				newTodo: '',
				filter: 'all',
				editing:null
			}
		},
		methods:{
			addTodo(){
				this.todos.push({
					completed:false,
					name:this.newTodo
				})
				this.newTodo = ''
			},
			deleteTodo(todo){
				this.todos = this.todos.filter(i => i !== todo)
			},
			deletecomplede(){
				this.todos = this.todos.filter(todo => !todo.completed)
			},
			editTodo(todo){
				this.editing = todo
			},
			doneEdit(){
				this.editing = null
			}

		},
		computed:{
			remaining(){
				
				return this.todos.filter(todo => !todo.completed).length
			},
			complede(){
				
				return this.todos.filter(todo => todo.completed).length
			},
			hasTodos(){
				return this.todos.length > 0
			},
			filterdTodos(){
			 if (this.filter == 'all') {
		        return this.todos
		      } else if (this.filter == 'todo') {
		        return this.todos.filter(todo => !todo.completed)
		      } else if (this.filter == 'done') {
		        return this.todos.filter(todo => todo.completed)
		      }

		      return this.todos
			}
			
		},
		directives:{
			focus(el,value){
				if (value) {
					Vue.nextTick(() => {
						el.focus()
					})
					
				}
			}
		}

	}
</script>

<style src="./todos.css">
	
</style>
