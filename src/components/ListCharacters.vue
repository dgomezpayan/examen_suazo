<template>
	<el-row :gutter="20">
		<el-col :span="8" v-for="item in items" :key="item">
			<el-card shadow="hover" :body-style="{ padding: '0px' }"> </el-card>
		</el-col>
	</el-row>
</template>

<script setup>
	import { computed, ref } from "vue"

	const currentDate = ref(new Date())

	const props = defineProps({
		search: String,
		type: String,
		dataItems: Array,
	})

	const items = computed(() => {
		return props.dataItems
	})

	console.log(props.search, props.type, items)
</script>
<style scoped>
	.el-card {
		position: absolute;
		left: 0%;
		right: 0%;
		top: 0%;
		bottom: 0%;

		border: 1px solid #e0e0e0;
		box-sizing: border-box;
		border-radius: 10px;
	}
	.image {
		position: absolute;
		left: 0%;
		right: 57.06%;
		top: 0%;
		bottom: 0%;

		/* background: url(); */
		border-radius: 10px 0px 0px 10px;
	}
	.pointG {
		position: absolute;
		left: 47.55%;
		right: 50.61%;
		top: 14.29%;
		bottom: 81.43%;

		background: #27ae60;
	}
	.pointR {
		position: absolute;
		left: 47.55%;
		right: 50.61%;
		top: 14.29%;
		bottom: 81.43%;

		background: #eb5757;
	}
	.status {
		position: absolute;
		left: 51.23%;
		right: 26.38%;
		top: 12.14%;
		bottom: 77.14%;

		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;

		color: #4f4f4f;
	}
	.name {
		position: absolute;
		width: 100px;
		height: 20px;
		left: 155px;
		top: 30px;

		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 16px;
		line-height: 20px;
		/* identical to box height */

		text-align: center;

		color: #000000;
	}
	.locationTitle {
		position: absolute;
		left: 47.55%;
		right: 10.74%;
		top: 41.43%;
		bottom: 47.86%;

		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;

		color: #828282;
	}
	.location {
		position: absolute;
		left: 47.55%;
		right: 32.52%;
		top: 52.14%;
		bottom: 37.14%;

		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 12px;
		line-height: 15px;
		/* identical to box height */

		text-align: center;

		color: #000000;
	}
	.firstSeenTitle {
		position: absolute;
		left: 47.55%;
		right: 10.74%;
		top: 69.29%;
		bottom: 20%;

		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 10px;
		line-height: 12px;

		color: #828282;
	}
	.firstSeen {
		position: absolute;
		left: 47.55%;
		right: 14.72%;
		top: 80%;
		bottom: 9.29%;

		font-family: Montserrat;
		font-style: normal;
		font-weight: normal;
		font-size: 12px;
		line-height: 15px;
		/* identical to box height */

		text-align: center;

		color: #000000;
	}
</style>
