<template>
	<Main
		class="h-[1200px]"
	>
		<div class="flex w-full justify-end h-screen items-center">
			<div 
				class="size-[200px] aspect-square relative group"
				 :class="{'animate-cat-come': isAnimated}"
        		@mouseenter="triggerAnimation"
			>
				<span 
					class="absolute inset-0 bg-gray-400"
					:class="{'animate-cat-come-opacity': isAnimated}"
				></span>
				<img 
					:src="sticker" 
					alt="cat"
					class="object-cover h-full"
				>
			</div>
		</div>
		<div class="h-[200px] grid items-center justify-center">
			<span class="text-xl font-bold">Спасибо за просмотр</span>
		</div>
	</Main>
</template>

<script setup>
	import Main from '~/layouts/Main.vue';
	import { onMounted, ref, watch } from 'vue';

	const sticker = ref('')
	const isAnimated = ref(false)

	const isCooldown = ref(false)

	const triggerAnimation = () => {
		if (!isAnimated.value && !isCooldown.value) {
			isAnimated.value = true
			isCooldown.value = true
			
			setTimeout(() => {
				isCooldown.value = false
			}, 5500)
		}
	}

	watch(isAnimated, (newVal) => {
		if (newVal) {
			setTimeout(() => {
				isAnimated.value = false
			}, 5100)
		}
	}, { deep: true })

	onMounted(async () => {
		const geterLink = "https://cataas.com/cat"
		const options = {
			method: "GET",
			mode: "cors",
			headers: {
				"Content-Type": "application/json",
				"Accept": "application/json",
			},
		}
		try {
			const {
				url
			} = await fetch(geterLink, options)
			if (url) sticker.value = url
		} catch(e) {
			console.log(e);
		}
	})
</script>

<style scoped>
	@keyframes catCome {
		from {
			transform: translateX(0);
		}
		to {
			transform: translateX(-200px);
		}
	}

	.cat-come {
		animation: catCome 0.5s ease-in-out forwards;
	}
</style>