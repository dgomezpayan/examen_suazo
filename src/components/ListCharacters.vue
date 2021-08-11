<template>
	<el-row :gutter="20">
		<el-col :span="8" v-for="item in items" :key="item">
			<el-card shadow="hover" :body-style="{ padding: '0px' }">
				<el-row>
					<el-col :span="10">
						<div class="image">
							<el-avatar
								shape="square"
                size="200"
								fit="cover"
								:src="item.image"
							></el-avatar>
						</div>
					</el-col>
					<el-col :span="14">
						<div class="info">
							<div class="cred"></div>
							<div style="color: #d3d3d3;">{{ item.status }} - {{ item.species }}</div>
							<br />
							<span style="font-weight: bold;">{{ item.name }}</span
							><br />
							<span style="color: #d3d3d3;">Last Known location:</span><br />
							<span style="font-weight: bold;">{{ item.location.name }}</span>
						</div>
					</el-col>
				</el-row>
			</el-card>
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
	.image {
		width: 100%;
		display: flex;
	}
	.el-col {
		margin-bottom: 20px;
	}
	.info {
		text-align: left;
		display: inline-block;
		margin-top: 10px;
		padding-left: 10px;
		font-size: 12px;
	}
	.cgreen {
		height: 8px;
		width: 8px;
		-moz-border-radius: 50px;
		-webkit-border-radius: 50px;
		border-radius: 50px;
		background: #5cb85c;
	}
	.cred {
		height: 8px;
		width: 8px;
		background: #ee6969;
		-moz-border-radius: 50px;
		-webkit-border-radius: 50px;
		border-radius: 50px;
	}
</style>
