<template>
	<el-row :gutter="15">
		<el-col :span="12" v-for="item in items" :key="item">
			<el-card
				shadow="hover"
				:body-style="{ padding: '0px' }"
				@click="detail(item)"
			>
				<el-image :src="item.image"></el-image>
				<div
					class="point"
					:class="[
						{ green: item.status === 'Alive' },
						{ red: item.status === 'Dead' },
						{ gray: item.status === 'unknown' },
					]"
				></div>
				<div class="status">{{ item.status }} - {{ item.species }}</div>
				<div class="name">{{ item.name }}</div>
				<div class="locationtitle">Last known location:</div>
				<div class="location">{{ item.location.name }}</div>
				<div class="firstseentitle">First seen in:</div>
				<div class="firstseen">{{ episodeName(item.episode[0]) }}</div>
			</el-card>
		</el-col>
	</el-row>

	<el-dialog v-model="centerDialogVisible" width="70%" center>
		<div class="image"></div>
		<div class="photo"></div>
		<div class="detail">
			<span class="">Alive</span>
			<span class="">Beth Smith</span>
			<span class="">HUMAN</span>
		</div>
	</el-dialog>
</template>

<script setup>
	import axios from "axios"
	import { inject, onMounted, provide, ref, watchEffect } from "vue"

	const props = defineProps({
		search: String,
		type: String,
	})

	const items = ref([])
	const itemsFilter = ref([])

	const centerDialogVisible = ref(false)

	const episodes = ref([])

	const search = inject("search")

	const episodeName = (e) => {
		let item = episodes.value.find((item) => item.url === e)
		let name = ""
		if (item !== undefined) {
			name = item.name
		}

		return name
	}
	const url = ref("https://rickandmortyapi.com/api/character?")

	const searchCharacter = (word) => {
		if (word !== "") {
			items.value = itemsFilter.value
			items.value = items.value.filter((it) =>
				it.name.toUpperCase().includes(word.toUpperCase())
			)
		}
	}

	const detail = (item) => {
		console.log("si funciona el click", item)
		centerDialogVisible.value = true
	}

	watchEffect(() => searchCharacter(search.value))

	onMounted(() => {
		axios
			.get("https://rickandmortyapi.com/api/episode")
			.then((response) => {
				episodes.value = response.data.results
			})
			.catch((error) => {
				console.log(error)
			})

		if (props.type === "All") {
			url.value = "https://rickandmortyapi.com/api/character"
		} else {
			url.value += `gender=${props.type}`
		}

		axios
			.get(url.value)
			.then((response) => {
				items.value = response.data.results
				itemsFilter.value = response.data.results
			})
			.catch((error) => {
				console.log(error)
			})
	})
</script>
<style scoped>
	.el-card {
		border: 1px solid #e0e0e0;
		box-sizing: border-box;
		border-radius: 10px;
		margin-bottom: 15px;
		height: 140px;
	}
	.el-image {
		left: -31%;
		top: 0px;
		bottom: -10px;
		width: 40%;
		height: 140px;
		border-radius: 10px 0px 0px 10px;
	}
	.point {
		width: 6px;
		height: 6px;
		border-radius: 50%;
		position: relative;
		left: 43%;
		bottom: 120px;
	}
	.green {
		background: #27ae60;
	}
	.red {
		background: #eb5757;
	}
	.gray {
		background: #6b6b6b98;
	}
	.status {
		position: relative;
		left: 2%;
		bottom: 128px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;
		color: #4f4f4f;
	}
	.name {
		position: relative;
		left: 4.5%;
		bottom: 126px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 16px;
		line-height: 20px;
		color: #000000;
	}
	.locationtitle {
		position: relative;
		left: 4.5%;
		bottom: 122px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;
		color: #4f4f4f;
	}
	.location {
		position: relative;
		left: 13%;
		bottom: 120px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 12px;
		line-height: 15px;
		text-align: center;
		color: #000000;
	}
	.firstseentitle {
		position: relative;
		left: 1.3%;
		bottom: 116px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;
		color: #4f4f4f;
	}
	.firstseen {
		position: relative;
		left: 3.5%;
		bottom: 114px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 12px;
		line-height: 15px;
		text-align: center;
		color: #000000;
	}
	.image {
		width: 100%;
		height: 220px;
		left: 345px;
		top: 295px;

		background: url("../assets/runrickandmorty.png");
		background-size: cover;
		border-radius: 10px 10px 0px 0px;
	}
	.detail {
		position: relative;
		width: 100%;
		height: 179px;
		left: 0%;
		z-index: 1;
		bottom: 150px;
		text-align: center;

		background: #f2f2f2;
	}
	.photo {
		position: relative;
		left: 45%;
		bottom: 80px;
		z-index: 2;
		height: 150px;
		width: 150px;

		border: 2px solid #fffbfb;
		box-sizing: border-box;
		border-radius: 50%;

		background: url("../assets/runrickandmorty.png");

	}
</style>
