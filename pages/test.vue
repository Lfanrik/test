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

<script setup lang="ts">
	import Main from '~/layouts/Main.vue';
	import { onMounted, ref, watch } from 'vue';

	const sticker = ref<string>('')
	const isAnimated = ref<boolean>(false)
	const isCooldown = ref<boolean>(false)

	const triggerAnimation = (): void => {
		if (!isAnimated.value && !isCooldown.value) {
			isAnimated.value = true
			isCooldown.value = true
			
			setTimeout(() => {
				isCooldown.value = false
			}, 5500)
		}
	}

	watch(isAnimated, (newVal: boolean) => {
		if (newVal) {
			setTimeout(() => {
				isAnimated.value = false
			}, 5100)
		}
	})

	onMounted(async (): Promise<void> => {
		const geterLink: string = "https://cataas.com/cat"
		const options: RequestInit = {
			method: "GET",
			headers: {
				"Content-Type": "application/json",
				"Accept": "application/json",
			},
		}
		
		try {
			const response: Response = await fetch(geterLink, options)
			if (response.ok) {
				sticker.value = response.url
			}
			} catch(e: unknown) {
			if (e instanceof Error) {
				console.log(e.message)
			}
		}
	})
</script>