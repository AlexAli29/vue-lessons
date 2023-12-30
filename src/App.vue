<script setup lang="ts">
import { computed, ref } from "vue";
const todos = ref<{ id: string; title: string; description: string }[]>([]);
const title = ref("");
const description = ref("");
const search = ref("");
const filteredTodos = computed(() => {
	if (!search.value) {
		return todos.value;
	}
	if (search) {
		return todos.value.filter((todo) =>
			todo.title.toLowerCase().includes(search.value.toLowerCase())
		);
	}
});
const handleAddTodo = () => {
	if (title.value && description.value) {
		todos.value.push({
			id: new Date().toISOString(),
			description: description.value,
			title: title.value,
		});
		title.value = "";
		description.value = "";
	}
};
const handleDelete = (id: string) => {
	todos.value = todos.value.filter((todo) => todo.id !== id);
};
</script>

<template>
	<div class="main">
		<input type="text" placeholder="search" v-model="search" />
		<div class="controls">
			<input placeholder="title" v-model="title" />
			<input placeholder="description" v-model="description" />
			<button @click="handleAddTodo">Add to todo</button>
		</div>
		<h2>Todos</h2>
		<div class="todos">
			<div class="todo" v-for="todo in filteredTodos">
				<span>Title:{{ todo.title }}</span>
				<span>Description:{{ todo.description }}</span>
				<button @click="handleDelete(todo.id)">Delete</button>
			</div>
		</div>
	</div>
</template>

<style lang="scss" scoped>
button {
	color: white;
	background-color: rgb(153, 201, 83);
	padding: 5px;
	border: none;
	border-radius: 5px;
}
.main {
	display: flex;
	flex-direction: column;
	align-items: center;
}
.controls {
	display: flex;
	gap: 10px;
}
.todos {
	display: flex;
	flex-direction: column;
	gap: 6px;
	background-color: rgb(253, 246, 242);
	padding: 5px 20px;
	border-radius: 5px;
	.todo {
		display: flex;

		flex-direction: column;
		border: 1px solid rgb(207, 83, 26);
		background-color: rgb(189, 207, 233);
		padding: 10px;
		border-radius: 10px;
	}
}
</style>
