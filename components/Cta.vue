<script setup>
let btn_pdf = ref(null)
let btn_subscribe = ref(null)

const { data } = await useAsyncData('cta', () => queryContent('/cta').findOne())

await useAsyncData('', () => queryContent('/global').find())
.then(resp => {
	btn_pdf.value = resp.data.value[0].btn_pdf
	btn_subscribe.value = resp.data.value[0].btn_subscribe
})

const emit = defineEmits(['redirect'])

const setRedirection = () => {
	emit('redirect', true)
	setTimeout(() => window.location.href = btn_subscribe.value.link, 2000)
}
</script>


<template>
<section id="cta">
	<h3>{{ data.title }}</h3>
	<p>{{ data.p1 }}</p>

	<div class="grid justify-center	gap-x-6 gap-y-4 mt-8 sm:flex">
		<div>
			<Button 
				:text="btn_pdf.content" 
				el="link" 
				:href="btn_pdf.link" 
				:target_blank="true" />
		</div>

		<div>
			<span @click="setRedirection">
				<Button :text="btn_subscribe.content" />
			</span>
		</div>
	</div>
</section>
</template>


<style scoped>
#cta{
	@apply border-y border-white px-10 pt-6 pb-8 lg:px-40;
}

h3{
	@apply text-3xl leading-none font-bold text-center md:text-4xl;
}

p{
	@apply text-base text-center leading-tight mt-4;
}
</style>



<style>
#cta .btn{
	@apply w-full;
}
</style>