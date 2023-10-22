<template>
	<div class="loader__wrapper">
		<div class="loading">
			<div></div>
			<div></div>
			<div></div>
			<div></div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'loader-component',
};
</script>

<style>
.loader__wrapper{
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}
.loading {
	display: inline-block;
	position: relative;
	width: 64px;
	height: 64px;
}

.loading div {
	position: absolute;
	top: 27px;
	width: 11px;
	height: 11px;
	border-radius: 50%;
	background: #333;
	animation-timing-function: cubic-bezier(0, 1, 1, 0);
}

.loading div:nth-child(1) {
	left: 6px;
	animation: lds-ellipsis1 0.6s infinite;
}

.loading div:nth-child(2) {
	left: 6px;
	animation: lds-ellipsis2 0.6s infinite;
}

.loading div:nth-child(3) {
	left: 26px;
	animation: lds-ellipsis2 0.6s infinite;
}

.loading div:nth-child(4) {
	left: 45px;
	animation: lds-ellipsis3 0.6s infinite;
}

@keyframes lds-ellipsis1 {
	0% {
		transform: scale(0);
	}
	100% {
		transform: scale(1);
	}
}

@keyframes lds-ellipsis3 {
	0% {
		transform: scale(1);
	}
	100% {
		transform: scale(0);
	}
}

@keyframes lds-ellipsis2 {
	0% {
		transform: translate(0, 0);
	}
	100% {
		transform: translate(19px, 0);
	}
}
</style>
