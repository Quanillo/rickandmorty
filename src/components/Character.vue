<template>
    <div class="m-5 rounded-lg bg-sky-900 shadow-sm drop-shadow-cyan shadow-cyan-200 hover:opacity-80">
        <div class="sm:flex font-sans backg relative ">
            <div class="p-3  flex-none static ">
                <img :src="char.image" @error="" loading="lazy"
                class="object-top object-cover w-28 h-28 rounded-full mx-auto border-2 border-cyan-200">
            </div>
            <div class="p-3 flex-auto static text-center place-self-center">
                <span class="flex-auto text-xl font-semibold text-stone-200">{{ char.name }}</span>
                <span class="text-lg font-semibold text-cyan-200"> '{{ char.species }}'</span> 
            </div>
            <div>

            </div>
        </div>
    </div>
</template>
    
<script>

export default {
    name: 'Character',
    props: {
        char: {
            type: Object,
        },
    },
    methods: {
    },
}
</script>
    
<style scoped>

</style>