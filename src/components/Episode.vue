<template>
    <div>
        <div class="m-5 rounded-lg bg-sky-900 shadow-sm drop-shadow-cyan shadow-cyan-200 hover:opacity-80">
        <div class="sm:flex font-sans backg relative ">
            <div class="p-3 flex-auto static text-center place-self-center">
                <span>
                    <p class="flex-auto text-xl font-semibold text-cyan-200">Name: </p> 
                    <p  class="flex-auto text-xl font-semibold text-stone-200">{{this.episode.name}}</p>
                </span>
                <span>
                    <p class="flex-auto text-xl font-semibold text-cyan-200">Date of issue: </p> 
                    <p  class="flex-auto text-xl font-semibold text-stone-200">{{this.episode.air_date}}</p>
                </span>
                <span>
                    <p class="flex-auto text-xl font-semibold text-cyan-200">Episode: </p> 
                    <p  class="flex-auto text-xl font-semibold text-stone-200">{{this.episode.episode}}</p>
                </span>
                <button v-if="!this.showEpisode" @click="showMore"
                class="border-2 border-cyan-200 px-3 rounded-md bg-sky-900 text-slate-100">Show More</button>
                <button v-else @click="showLess"
                class="border-2 border-cyan-200 px-3 rounded-md bg-sky-900 text-slate-100">Show Less</button>
            </div>
        </div>
    </div>

    <div>

    </div>

    </div>
</template>
    
<script>

export default {
    name: 'Episode',
    props: {
        episode: {
            type: Object,
        },
        showEpisode: {
            type: Boolean,
        }
    },
    emits: ["showMore", "showLess"],
    methods: {
        showMore(){
            this.$emit('showMore', this.episode);
        },
        showLess(){
            this.$emit('showLess', null);
        }
    },
}
</script>
    
<style scoped>

</style>