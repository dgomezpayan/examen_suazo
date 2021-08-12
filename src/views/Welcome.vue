<template>
	<el-container>
		<el-main>
			<img class="image-suazo" :src="url2" />
			<img class="image" :src="url" />
			<span class="title">Bienvenido a Rick and Morty</span>
			<p>
				En esta prueba, evaluaremos su capacidad para construir la aplicación
				mediante el análisis de código y la reproducción del siguiente diseño
			</p>
			<el-button @click="$router.push('Home')" type="primary" round>
				Continuar
			</el-button>
			-->
		</el-main>
	</el-container>
</template>

<script setup>
	import { ref } from "vue"

	const url = ref("")
	const url2 = ref("")
	url.value = require("@/assets/rick and morty.png")
	url2.value = require("@/assets/suazo.png")
</script>

<style scoped>
	.el-container {
		position: absolute;
		width: 100vw;
		height: 100vh;
		left: 0px;
		top: 0px;

		background: rgba(0, 0, 0, 0.7);
		z-index: 1;
	}
	.el-main {
		position: relative;
		width: 100vw;
		top: 0px;
		height: 100vh;
		z-index: 2;

		background-image: url("../assets/portal-morty-smith-rick.png");

		background-position: center center;
		background-attachment: fixed;
		background-size: cover;
	}
	.image {
		position: absolute;
		width: 664px;
		height: 233px;
		left: calc(50% - 664px / 2 + 22px);
		top: calc(50% - 200px / 2 - 35px);
	}
	.image-suazo {
		position: absolute;
		width: 10vw;
		height: 8vh;
		left: 850px;
		top: 190px;
	}
	.title {
		position: absolute;
		width: 872px;
		height: 53px;
		left: calc(50% - 872px / 2);
		top: calc(68%);

		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 36px;
		line-height: 44px;
		text-align: center;

		color: #ffffff;
	}
	p {
		position: absolute;
		width: 872px;
		height: 62px;
		left: calc(50% - 872px / 2);
		top: calc(72%);

		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 18px;
		line-height: 30px;
		/* or 167% */

		text-align: center;

		color: #ffffff;
	}
	.el-button {

		font-family: Lato;
		font-style: normal;
		font-weight: bold;
		font-size: 18px;
		line-height: 22px;

		/* align-items: center;
		text-align: center; */

		color: #ffffff;

		/* Inside Auto Layout */

		flex: none;
		order: 0;
		flex-grow: 0;
		margin: 0px 10px;
		display: flex;
		flex-direction: row;
		align-items: center;
		padding: 11px 20px;

		position: absolute;
		width: 120px;
		height: 44px;
		left: calc(50% - 80px);
		top: 800px;

		background: #11555f;
	}
</style>
