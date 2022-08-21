<template>
	<div id="root">
		<div class="todo-container">
			<div class="todo-wrap">
				<MyHeader :addTodo="addTodo" />
				<MyList :todos="todos" :deleteTodo="deleteTodo" :updateTodo="updateTodo" />
				<MyFooter :todos="todos" :checkedAll="checkedAll" :clearAllTruetodo="clearAllTruetodo" />
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs,watch ,onMounted} from "vue";
import MyHeader from './components/MyHeader.vue'
import MyList from './components/MyList.vue'
import MyFooter from './components/MyFooter.vue'
import { Todo } from "./types/todo";
import { saveTodos,readTodos } from "./utils/localStorageUtils";
export default defineComponent({
	name: 'App',
	//注册组件
	components: {
		MyHeader,
		MyList,
		MyFooter
	},
	//数据一共用数组来存储，数组中的每一个数据都是一个对象，对象中一共有三个属性(id ,title,isTrue)
	//把数组暂且定义在App.vue组件中
	setup() {
		//定义一个数组数据
		// const state = reactive<{ todos: Todo[] }>({
		// 	todos: [
		// 		{ id: 1, title: '用餐', isTrue: false },
		// 		{ id: 2, title: '学习', isTrue: true },
		// 		{ id: 3, title: '运动', isTrue: false }
		// 	],
		// })
         const state = reactive<{ todos: Todo[] }>({
			todos: [],
		})
		//界面加载完毕后过了一会再读取数据
         onMounted(()=>{
			setTimeout(()=>{
				state.todos = readTodos()
			},500)
		 })
		//添加数据的方法
		const addTodo = (todo: Todo) => {
			state.todos.unshift(todo)
		}
		//删除数据的方法
		const deleteTodo = (id: number) => {
			state.todos = state.todos.filter(todo=>todo.id!=id)
		}
		//修改todo的isTrue的状态
		const updateTodo = (todo: Todo, isTrue: boolean) => {
			todo.isTrue = isTrue
		}
		//全选或全不选的方法
		const checkedAll = (isTrue: boolean) => {
			state.todos.forEach(todo => {
				todo.isTrue = isTrue
			})
		}
		//清除所有已完成的项目
		const clearAllTruetodo = () => {
			state.todos = state.todos.filter(todo=>!todo.isTrue)
		}
		//监视操作：如何todos数组的数据变化了，直接存储到浏览器的缓存中
		/* watch(()=>state.todos,(value)=>{
			//保存到浏览器的缓存中
			localStorage.setItem('todos_key',JSON.stringify(value))
		},{deep:true}) */
        
        //watch(()=>state.todos,saveTodos,{deep:true})这一行相当于下面的代码

		watch(()=>state.todos,(value)=>{
			//保存到浏览器的缓存中
			saveTodos(value)
		},{deep:true})
		return {
			...toRefs(state),
			addTodo, deleteTodo, updateTodo, checkedAll, clearAllTruetodo
		}
	}
}
)
</script>

<style>
/*base*/
body {
	background: #fff;
}

.btn {
	display: inline-block;
	padding: 4px 12px;
	margin-bottom: 0;
	font-size: 14px;
	line-height: 20px;
	text-align: center;
	vertical-align: middle;
	cursor: pointer;
	box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
	border-radius: 4px;
}

.btn-danger {
	color: #fff;
	background-color: #da4f49;
	border: 1px solid #bd362f;
}

.btn-danger:hover {
	color: #fff;
	background-color: #bd362f;
}

.btn:focus {
	outline: none;
}

.todo-container {
	width: 600px;
	margin: 0 auto;
}

.todo-container .todo-wrap {
	padding: 10px;
	border: 1px solid #ddd;
	border-radius: 5px;
}
</style>
