<template>
	<el-container>
		<el-main>
			<el-row :gutter="20">
				<el-col :span="8"></el-col>
				<el-col :span="8"
					>
					<img class="image" :src="url" />
					<el-input
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
	import { inject, ref } from "vue"

	const search = inject('search')

	const url = ref("")
	url.value = require("@/assets/rick and morty.png")

</script>

<style scoped>
	.el-container {
		background-image: url("../assets/portal-morty-smith-rick.png");
		background-position: left top;
		background-attachment: fixed;
		background-size: cover;
		height: 350px;
		color: #fff;
		text-align: center;
		padding-top: 80px;
	}
	.image{
		width: 350px;
	}
</style>
