<template>
	<el-container>
		<el-main>
			<span style="font-size: 30px;">Bienvenido a Rick and Morty</span>
			<p>
				En esta prueba, evaluaremos su capacidad para construir la aplicación
				mediante el análisis de código y la reproducción del siguiente diseño
			</p>
			<el-button @click="$router.push('Home')" type="primary" round>
				Continuar
			</el-button>
		</el-main>
	</el-container>
</template>

<script setup></script>

<style scoped>
	.el-main {
		background-image: url("../assets/rickandmorty.png");
		background-position: center center;
		background-attachment: fixed;
		background-size: cover;
		height: 100vh;
		color: #fff;
		text-align: center;

		padding-top: 500px;
	}

</style>
