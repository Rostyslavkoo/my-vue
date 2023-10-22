<template>
	<div v-if="!loadingJobs" class="container">
		<div class="header">
			<button class="btn-primary" @click="handleclick('title')">
				order by title
			</button>
			<button class="btn-primary" @click="handleclick('salary')">
				order by salary
			</button>
			<button class="btn-primary" @click="handleclick('location')">
				order by location
			</button>
		</div>
		<div class="job-list">
			<job-list :jobs="jobs" :orderTerm="orderTerm" />
		</div>
	</div>
	<div v-else class="container">
		<loader></loader>
	</div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref } from 'vue';
import jobList from './components/jobList.vue';
import Job from './types/job';
import OrderType from './types/order';
import loader from './components/loader.vue';

export default defineComponent({
	name: 'App',
	components: {
		jobList,
		loader,
	},
	setup() {
		// Fill the array with objects with correct keys and real data
		const jobs = ref<Job[]>([]);
		const loadingJobs = ref<boolean>(false);
		const getJobs = async (page: number, pageSize: number) => {
			const url = `https://api.lever.co/v0/postings/leverdemo?mode=json&page=${page}&limit=${pageSize}`;
			try {
				loadingJobs.value = true;
				const data = await fetch(url);
				const json = await data.json();
				json.forEach((el: any) => {
					jobs.value.push({
						title: el.text,
						location: el.categories.location,
						id: el.id,
						salary: +((el.createdAt / 3000000000) * 4.5).toFixed(),
						description: el.descriptionPlain,
					});
				});
			} catch (e) {
				console.error(e);
			} finally {
				loadingJobs.value = false;
			}
		};
		const orderTerm = ref<OrderType>('title');
		const handleclick = (chosenTerm: OrderType) => {
			orderTerm.value = chosenTerm;
		};

		getJobs(1, 10);

		return { jobs, handleclick, loadingJobs, orderTerm };
	},
	methods: {
		// changeName(name: string) {
		// 	this.name = name;
		// },
		// changeAge(age: number | string) {
		// 	this.age = age;
		// },
	},
});
</script>

<style scoped>
.container {
	height: 100vh;
	margin: auto;
	max-width: 1560px;
	width: 100%;
	display: flex;
	flex-direction: column;
}
.container > .header {
	height: 100%;
	width: 100%;
	text-align: center;
}
.container .loader {
	height: 100vh;
	width: 100vw;
	display: flex;
	align-items: center;
}
div > button {
	margin: 10px;
	padding: 10px;
	border-radius: 5px;
	border: none;
	box-shadow: 0 1px 25px #e3e3e3, 0 1px 2px #e3e3e3;
	cursor: pointer;
}
</style>
