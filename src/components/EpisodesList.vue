<template>
    <div v-if="!this.showEpisode" >
        <div class="grid grid-cols-2 gap-3 place-content-center">
            <div>
                <p class="text-cyan-200">Filter by name</p>
                <input type="text" v-model.trim="filterName" list="names" @keydown.enter="filterByName" />
                <datalist id="names">
                    <option v-for="item in this.names">{{item}}</option>
                </datalist>
            </div>

            <div>
                <p class="text-cyan-200">Filter by Episode</p>
                <input type="text" v-model.trim="filterEpisode" @keydown.enter="filterByEpisode" />
            </div>
            
        </div>
        
        <div class="place-content-center">
            <ul class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 2xl:grid-cols-5">
                <li v-for="(item, index) in this.episodesList" :key="index">
                    <Episode :episode="item" @showMore="fillCharacterList" :showEpisode="this.showEpisode"/>
                </li>
            </ul>
        </div>

        <div class="flex place-content-center">
            <button @click="changePage(false)"
                class="border-2 border-cyan-200 px-3 rounded-md bg-sky-900 text-slate-100">prev</button>
            <p class="m-2 text-center text-xl text-cyan-200">{{ this.page }}</p>
            <button @click="changePage(true)"
                class="border-2 border-cyan-200 px-3 rounded-md bg-sky-900 text-slate-100">Next</button>
        </div>
    </div>
    <div  v-else 
    class="place-content-center">
            <Episode :episode="this.currentEpisode" :showEpisode="this.showEpisode"  @showLess="fillCharacterList"/>
            <p class="flex-auto text-xl font-semibold text-cyan-200">Characters appearing in this episode</p>
            <ul class="text-center grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 2xl:grid-cols-5">
                <li v-for="(item, index) in this.characterList" :key="index">
                    <Character :char="item"/>
                </li>
            </ul>
        </div>
    <div>
        
    </div>
</template>
  
<script>
import axios from 'axios';
import Episode from './Episode.vue';
import Character from './Character.vue';

export default {
    name: 'EpisodesList',
    components: {
    Episode,
    Character
},
    data() {
        return {
            filterName: '',
            filterEpisode: '',
            page: 1,
            episodesList: [],
            characterList:[],
            currentEpisode: {},
            showEpisode: false,
            names: [],
            maxPage: null,
        }
    },
    mounted() {
        this.fillListAll();
        this.episodesNames();
    },
    methods: {
        async fillListAll() {
            try {
                const response = await axios.get(`https://rickandmortyapi.com/api/episode/?page=${this.page}`);
                this.episodesList = response.data.results;
                this.maxPage = response.data.info.pages;
            } catch (error) {
                console.log(error);
            }
        },
        changePage(b) {
            b == true ? this.page++ : this.page--;
            this.page < 1 || this.page > this.maxPage ? this.page = 1 : this.page;
            this.fillListAll();
        },
        async filterByName() {
            try {
                const response = await axios.get(`https://rickandmortyapi.com/api/episode/?name=${this.filterName}`);
                this.episodesList = response.data.results;
                this.filterName = ""; 
            } catch (error) {
                console.log(error);
            }
        },
        async filterByEpisode() {
            try {
                const response = await axios.get(`https://rickandmortyapi.com/api/episode/?episode=${this.filterEpisode}`);
                this.episodesList = response.data.results;
                this.filterEpisode = "";
            } catch (error) {
                console.log(error);
            }
        },
        async fillCharacterList(episode){
            if(episode===null){
                this.showEpisode = false;
            }
            else{
                for(let i=0; i<episode.characters.length; i++){
                    const response = await axios.get(episode.characters[i]);
                    this.characterList.push(response.data);
                }
                this.currentEpisode = episode;
                this.showEpisode = true;
            }
           
        },
        async episodesNames(){ 
            try {
                let list = [];
                const response = await axios.get(`https://rickandmortyapi.com/api/episode/`);
                response.data.results.map(x=>!list.includes(x.name)? list.push(x.name): x);
                for(let i = 2; i<response.data.info.pages +1; i++){
                    const response2 = await axios.get(`https://rickandmortyapi.com/api/episode/?page=${i}`);
                    response2.data.results.map(x=>!list.includes(x.name)? list.push(x.name): x); 
                }
                this.names = list;
            } catch (error) {
                console.log(error);
            }
        },  
    },
    computed: {

    },
}
</script>
  
<style scoped>

</style>