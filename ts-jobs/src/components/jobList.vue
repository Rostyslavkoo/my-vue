<template>
	<div class="order-text">order by: {{ orderTerm }}</div>
	<ul>
		<li v-for="job in orderedJobs" :key="job.id">
			<h2>{{ job.title }}</h2>
			<div>
				<p>${{ getSalaryFormat(job.salary) }}</p>
			</div>
			<p>{{ job.location }}</p>
			<p class="description">
				{{ job.description }}
			</p>
		</li>
	</ul>
</template>
<script lang="ts">
import OrderTerm from '@/types/order';
import { computed, defineComponent, PropType } from 'vue';
import Job from './../types/job';

export default defineComponent({
	props: {
		jobs: {
			required: true,
			type: Array as PropType<Job[]>,
		},
		orderTerm: {
			required: true,
			type: String as PropType<OrderTerm>,
		},
	},
	setup(props) {
		const getSalaryFormat = function (salary: Job['salary']) {
			return String(salary).replace(
				/(?<!\..*)(\d)(?=(?:\d{3})+(?:\.|$))/g,
				'$1 '
			);
		};
		const orderedJobs = computed(() => {
			return [...props.jobs].sort((a: Job, b: Job) => {
				return a[props.orderTerm] > b[props.orderTerm] ? 1: -1;
			});
		});
        console.log(orderedJobs.value)
		return { getSalaryFormat, orderedJobs };
	},
});
</script>
<style scoped>
.order-text {
	text-align: center;
	font-size: 20px;
	font-weight: bold;
	margin: 20px 0;
}
ul {
	list-style: none;
	padding: 0;
	margin: 0;
	display: flex;
	align-items: center;
	justify-content: space-around;
	flex-wrap: wrap;
}
li {
	display: block;
	border: thin solid #f6f6f6;
	border-radius: 5%;
	padding: 30px;
	margin: 50px 0;
	width: 30%;
	background: #f6f6f6;
	box-shadow: 0 1px 25px #e3e3e3, 0 1px 2px #e3e3e3;
	cursor: pointer;
	height: 300px;
}
li:hover {
	transform: scale(1.01);
	box-shadow: 0 1px 25px #fff, 0 1px 2px #fff;
	background: #ffff;
	transition: all 0.1s ease-in-out;
	border: thin solid #fff;
}
li > h2 {
	margin: 0;
}
li > div {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

div > p {
	color: rgb(25, 133, 25);
	font-weight: bold;
}
.description {
	max-height: 300px;
	display: -webkit-box;
	-webkit-line-clamp: 4;
	-webkit-box-orient: vertical;
	overflow: hidden;
}
</style>
