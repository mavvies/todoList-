<template>
	<div class="todo-footer">
		<label>
			<!-- <input type="checkbox" :checked="isAll" @change="checkAll"/> -->
			<input type="checkbox" v-model="checkedAll"/>
		</label>
		<span>
			<span>已完成{{count}}</span> / 全部{{todos.length}}
		</span>
		<button class="btn btn-danger" @click="clearTodo">清除已完成任务</button>
	</div>
</template>

<script lang="ts">
import { defineComponent ,computed} from "vue";
import { Todo } from "../types/todo";
export default defineComponent({
	name: 'Myfooter',
	props:{
		todos:{
			type:Array as()=>Todo[],
			required:true
		},
		checkedAll:{
			type:Function,
			required:true
		},
		clearAllTruetodo:{
			type:Function,
			required:true
		}
	},
	setup(props){
		//已完成项目计算操作
		const count = computed(()=>{
			return props.todos.reduce((pre,todo,index)=>pre+(todo.isTrue?1:0),0)
		})
		//全选/全不选的计算操作
		const checkedAll = computed({
			get(){
				return count.value>0&&props.todos.length===count.value
			},
			set(value){
				props.checkedAll(value)
			}
		})
		//点击清除已完成任务
		const clearTodo = ()=>{
			props.clearAllTruetodo()
		}
		
		return{
			count,checkedAll,clearTodo
		}
	}
})
</script>

<style scoped>
/*footer*/
.todo-footer {
	height: 40px;
	line-height: 40px;
	padding-left: 6px;
	margin-top: 5px;
}

.todo-footer label {
	display: inline-block;
	margin-right: 20px;
	cursor: pointer;
}

.todo-footer label input {
	position: relative;
	top: -1px;
	vertical-align: middle;
	margin-right: 5px;
}

.todo-footer button {
	float: right;
	margin-top: 5px;
}
</style>