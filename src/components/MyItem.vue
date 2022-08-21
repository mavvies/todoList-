<template>
	<li >
		<label>
			<input type="checkbox" v-model="isTrue" />
			<span>{{ todo.title }}</span>
		</label>
		<button class="btn btn-danger" @click="delTodo(todo.id)">删除</button>
	</li>
</template>

<script lang="ts">

import { defineComponent,computed } from "vue";
import { Todo } from "../types/todo";
export default defineComponent({
	name: 'MyItem',
	props:{
		todo:{
           type:Object as()=>Todo,//函数返回的是Todo类型
		   required:true
		},
		deleteTodo:{
			type:Function,
            required:true
		},
		updateTodo:{
			type:Function,
			required:true
		}
	},
	setup(props) {
		//传来的数据
		const todo = props.todo
		//删除todo
		const delTodo=(id:number)=>{
           //提示
		   if(window.confirm('确定要删除吗？')){
			props.deleteTodo(id)
		   }
		   
		} 
		//计算属性的方式--来让当前的复选框选中或不选中
		const isTrue = computed({
			get(){
				return props.todo.isTrue
			},
			set(value){
              props.updateTodo(props.todo,value)
			}
		})
		return {
			todo,delTodo,isTrue
		}
	}
})
</script>

<style scoped>
/*item*/
li {
	list-style: none;
	height: 36px;
	line-height: 36px;
	padding: 0 5px;
	border-bottom: 1px solid #ddd;
}

li label {
	float: left;
	cursor: pointer;
}

li label li input {
	vertical-align: middle;
	margin-right: 6px;
	position: relative;
	top: -1px;
}

li button {
	float: right;
	display: none;
	margin-top: 3px;
}

li:before {
	content: initial;
}

li:last-child {
	border-bottom: none;
}

li:hover {
	background-color: #ddd;
}

li:hover button {
	display: block;
}
</style>