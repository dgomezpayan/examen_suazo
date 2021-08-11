<template>
	<el-container>
		<el-main>
			<el-row :gutter="20">
				<el-col :span="8"></el-col>
				<el-col :span="8"
					><el-input
						placeholder="Buscar Personaje..."
						prefix-icon="el-icon-search"
						v-model="search"
					>
					</el-input
				></el-col>
				<el-col :span="8"></el-col>
			</el-row>
		</el-main>
	</el-container>
</template>

<script setup>
	import { inject } from "vue"

	const search = inject("search")

</script>

<style scoped>
	.el-main {
		background-image: url("../assets/rickandmorty.png");
		background-position: center center;
		background-attachment: fixed;
		background-size: cover;
		height: 350px;
		color: #fff;
		text-align: center;
		padding-top: 150px;
	}
</style>
