<template>
	<ul class="todo-main">
		<MyItem :deleteTodo="deleteTodo"  v-for="(todo,index) in todos" :key="todo.id" :todo="todo" :updateTodo="updateTodo"/>
	</ul>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import MyItem from './MyItem.vue'
export default defineComponent({
	name: 'myList',
	components: { MyItem },
	props:['todos','deleteTodo','updateTodo'],
})


</script>

<style scoped>
/*main*/
.todo-main {
	margin-left: 0px;
	border: 1px solid #ddd;
	border-radius: 2px;
	padding: 0px;
}

.todo-empty {
	height: 40px;
	line-height: 40px;
	border: 1px solid #ddd;
	border-radius: 2px;
	padding-left: 5px;
	margin-top: 10px;
}
</style>