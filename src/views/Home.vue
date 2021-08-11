<template>
	<Search />

	<el-main>
		<el-tabs
			style="text-align: center;"
			:tab-position="tabPosition"
			v-model="activeName"
			@tab-click="handleClick"
		>
			<el-tab-pane label="All" name="All">
				<ListCharacters :search="search" type="All" />
			</el-tab-pane>
			<el-tab-pane label="Unknown" name="Unknown">
				<ListCharacters :search="search" type="Unknown" />
			</el-tab-pane>
			<el-tab-pane label="Female" name="Female">
				<ListCharacters :search="search" type="Female" />
			</el-tab-pane>
			<el-tab-pane label="Male" name="Male">
				<ListCharacters :search="search" type="Male" />
			</el-tab-pane>
			<el-tab-pane label="Genderless" name="Genderless">
				<ListCharacters :dataItems="items" :search="search" type="Genderless" />
			</el-tab-pane>
		</el-tabs>
	</el-main>
</template>

<script setup>
	import axios from "axios"
	import { provide, ref } from "vue"
	import Search from "../components/Search.vue"
	import ListCharacters from "../components/ListCharacters.vue"

	const search = ref("")
	provide("search", search)

	const tabPosition = ref("top")
	const activeName = ref("All")
  const items = ref([])

	const handleClick = async (tab) => {
		console.log(tab, search)

		await axios
			.get("https://rickandmortyapi.com/api/character")
			.then((response) => {
				console.log(response)
        items.value = response.data.results
        console.log('items', items.value);
			})
			.catch((error) => {
				console.log(error)
			})
	}
</script>

<style scoped>
	.el-tabs {
		padding-left: 40px;
		padding-right: 40px;
	}
</style>
