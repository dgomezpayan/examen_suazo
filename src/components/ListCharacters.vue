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
				<div class="firstseen">{{ getepisode(item.episode[0], "name") }}</div>
			</el-card>
		</el-col>
	</el-row>

	<el-dialog v-model="centerDialogVisible" width="70%" center>
		<div class="image"></div>
		<div class="photo"></div>
		<div class="detail">
			<span class="status-detail">Alive</span>
			<span class="name-detail">Beth Smith</span>
			<span class="species-detail">HUMAN</span>
		</div>
		<div class="description">
			<div class="information">
				<span class="info-title">Informacion</span>
				<el-row :gutter="60">
					<el-col :span="8">
						<el-card
							class="info-card"
							shadow="hover"
							:body-style="{ padding: '0px' }"
						>
							<div>
								<i class="el-icon-info"></i>
								Gender:
							</div>
							<div class="">Male</div>
						</el-card>
					</el-col>
					<el-col :span="8"
						><el-card
							class="info-card"
							shadow="hover"
							:body-style="{ padding: '0px' }"
						>
							<div>
								<i class="el-icon-info"></i>
								Origin:
							</div>
							<div class="">Earth</div>
						</el-card></el-col
					>
					<el-col :span="8"
						><el-card
							class="info-card"
							shadow="hover"
							:body-style="{ padding: '0px' }"
						>
							<div>
								<i class="el-icon-info"></i>
								Type:
							</div>
							<div class="">Unknow</div>
						</el-card></el-col
					>
				</el-row>
			</div>
			<el-divider></el-divider>
			<div class="episodios">
				<span class="ep-title">Episodios</span>
				<el-scrollbar height="300px">
					<el-row :gutter="25">
						<el-col :span="6" v-for="ep in character.episode" :key="ep">
							<el-card
								class="ep-card"
								shadow="hover"
								:body-style="{ padding: '0px' }"
							>
								<div class="">{{ getepisode(ep, "name") }}</div>
								<div class="">{{ getepisode(ep, "episode") }}</div>
								<div class="">{{ getepisode(ep, "air_date") }}</div>
							</el-card>
						</el-col>
					</el-row>
				</el-scrollbar>
			</div>
			<el-divider></el-divider>
			<div class="episodios">
				<span class="ep-title">Personajes Interesantes</span>
				<el-row :gutter="15">
					<el-col :span="12" v-for="item in characterInter" :key="item.id">
						<el-card shadow="hover" :body-style="{ padding: '0px' }">
							<el-image class="image-inter" :src="item.image"></el-image>
							<div
								class="point"
								:class="[
									{ green: item.status === 'Alive' },
									{ red: item.status === 'Dead' },
									{ gray: item.status === 'unknown' },
								]"
							></div>
							<div class="status-inter">{{ item.status }} - {{ item.species }}</div>
							<div class="name-inter">{{ item.name }}</div>
							<div class="locationtitle-inter">Last known location:</div>
							<div class="location-inter">{{ item.location.name }}</div>
							<div class="firstseentitle-inter">First seen in:</div>
							<div class="firstseen-inter">
								{{ getepisode(item.episode[0], "name") }}
							</div>
						</el-card>
					</el-col>
				</el-row>
			</div>
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
	const character = ref()
	const characterInter = ref([])

	const centerDialogVisible = ref(false)

	const episodes = ref([])

	const search = inject("search")

	const getepisode = (e, type) => {
		let item = episodes.value.find((item) => item.url === e)
		let value = ""
		if (item !== undefined) {
			switch (type) {
				case "name":
					value = item.name
					break
				case "episode":
					value = item.episode
					break
				case "air_date":
					value = item.air_date
					break
			}
		}

		return value
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
		character.value = item

		const array = getArrayNumAleatorio()

		console.log(`https://rickandmortyapi.com/api/character/${array}`)
		axios
			.get(`https://rickandmortyapi.com/api/character/${array}`)
			.then((response) => {
				console.log(response.data)
				characterInter.value = response.data
			})
			.catch((error) => {
				console.log(error)
			})
	}

	const getArrayNumAleatorio = () => {
		let cad = []
		for (let i = 0; i < 3; i++) {
			let num = Math.floor(Math.random() * 10 * (i + 1))
			cad.push(num)
		}

		return cad.join(",")
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
		left: 43%;
		bottom: 80px;
		z-index: 2;
		height: 150px;
		width: 150px;

		border: 2px solid #fffbfb;
		box-sizing: border-box;
		border-radius: 50%;

		background: url("../assets/runrickandmorty.png");
	}
	span {
		display: block;
	}
	.status-detail {
		padding-top: 80px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: 500;
		font-size: 15px;
		line-height: 20px;
		color: #4f4f4f;
	}
	.name-detail {
		font-family: Montserrat;
		font-style: normal;
		font-weight: 600;
		font-size: 20px;
		line-height: 26px;
		color: #081f32;
	}
	.species-detail {
		font-family: Montserrat;
		font-style: normal;
		font-weight: 500;
		font-size: 13px;
		line-height: 20px;
		color: #4f4f4f;
	}
	.description {
		height: 750px;
	}
	.info-title {
		position: absolute;
		width: 126px;
		height: 24px;
		left: 2.5%;
		top: 465px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: 600;
		font-size: 20px;
		line-height: 24px;

		color: #4f4f4f;
	}
	.info-card {
		height: 55px;
		margin-top: -100px;
	}
	.info-card div {
		margin-left: 10px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 11px;
		line-height: 13px;
		letter-spacing: 0.07px;
		color: #828282;
	}
	.info-card div:nth-child(1) {
		margin-top: 10px;
	}
	.info-card div:nth-child(2) {
		font-weight: 600;
		font-size: 17px;
		line-height: 22px;
		letter-spacing: -0.41px;
		color: #081f32;
	}
	.ep-title {
		position: relative;
		width: 300px;
		height: 24px;
		left: 0%;
		top: -10px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: 600;
		font-size: 20px;
		line-height: 24px;

		color: #4f4f4f;
	}
	.ep-card {
		height: 75px;
		margin-top: 0px;
	}
	.ep-card div {
		margin-left: 10px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 11px;
		line-height: 13px;
		letter-spacing: 0.07px;
		color: #828282;
	}
	.ep-card div:nth-child(1) {
		margin-top: 10px;
	}
	.ep-card div:nth-child(2) {
		font-family: Montserrat;
		font-style: normal;
		font-weight: 600;
		font-size: 17px;
		line-height: 22px;
		/* identical to box height, or 129% */

		letter-spacing: -0.41px;

		color: #081f32;
	}
	.image-inter {
		left: 0%;
	}
	.status-inter {
		position: relative;
		left: 47%;
		bottom: 128px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;
		color: #4f4f4f;
	}
	.name-inter {
		position: relative;
		left: 47%;
		bottom: 126px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 16px;
		line-height: 20px;
		color: #000000;
	}
	.locationtitle-inter {
		position: relative;
		left: 47%;
		bottom: 122px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;
		color: #4f4f4f;
	}
	.location-inter {
		position: relative;
		left: 20%;
		bottom: 120px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 12px;
		line-height: 15px;
		text-align: center;
		color: #000000;
	}
	.firstseentitle-inter {
		position: relative;
		left: 47%;
		bottom: 116px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;
		color: #4f4f4f;
	}
	.firstseen-inter {
		position: relative;
		left: 8%;
		bottom: 114px;
		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 12px;
		line-height: 15px;
		text-align: center;
		color: #000000;
	}
</style>
