<template>
    <div v-if="!this.showLocation" >
        <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 place-content-center">
            <div>
                <p class="text-cyan-200">Filter by name</p>
                <input type="text"  v-model.trim="filterName" list="names" @keydown.enter="filterByName" />
                <datalist id="names">
                    <option v-for="item in this.names">{{item}}</option>
                </datalist>
            </div>

            <div>
                <p class="text-cyan-200">Filter by Type</p>
                <input type="text" v-model.trim="filterType" list="types" @keydown.enter="filterByType" />
                <datalist id="types">
                    <option v-for="item in this.types">{{item}}</option>
                </datalist>
            </div>

            <div>
                <p class="text-cyan-200">Filter by Dimension</p>
                <input type="text" v-model.trim="filterDimension" list="dimensions"  @keydown.enter="filterByDimension" />
                <datalist id="dimensions">
                    <option v-for="item in this.dimensions">{{item}}</option>
                </datalist>
            </div>
            
        </div>
        
        <div class="place-content-center">
            <ul class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 2xl:grid-cols-5">
                <li v-for="(item, index) in this.locationsList" :key="index">
                    <Location :location="item" @showMore="fillCharacterList" :showLocation="this.showLocation"/>
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
            <Location :location="this.currentLocation" :showLocation="this.showLocation"  @showLess="fillCharacterList"/>
            <p class="flex-auto text-xl font-semibold text-cyan-200">Residents living in this location</p>
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
import Location from './Location.vue';
import Character from './Character.vue';

export default {
    name: 'LocationList',
    components: {
    Location,
    Character
},
    data() {
        return {
            filterName: '',
            filterType: '',
            filterDimension: '',
            page: 1,
            maxPage: null,
            locationsList: [],
            characterList:[],
            currentlocation: {},
            showLocation: false,
            names: [],
            types: [],
            dimensions: [],
        }
    },
    mounted() {
        this.fillListAll();
        this.locationNames();
        this.locationType();
        this.locationDimension();
    },

    methods: {
        async fillListAll() {
            try {
                const response = await axios.get(`https://rickandmortyapi.com/api/location/?page=${this.page}`);
                this.locationsList = response.data.results;
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
                const response = await axios.get(`https://rickandmortyapi.com/api/location/?name=${this.filterName}`);
                this.locationsList = response.data.results;
                this.filterName = ""; 
            } catch (error) {
                console.log(error);
            }
        },
        async filterByType() {
            try {
                const response = await axios.get(`https://rickandmortyapi.com/api/location/?type=${this.filterType}`);
                this.locationsList = response.data.results;
                this.filterType = "";
            } catch (error) {
                console.log(error);
            }
        },
        async filterByDimension() {
            try {
                const response = await axios.get(`https://rickandmortyapi.com/api/location/?dimension=${this.filterDimension}`);
                this.locationsList = response.data.results;
                this.filterDimension = "";
            } catch (error) {
                console.log(error);
            }
        },
        async fillCharacterList(location){
            if(location===null){
                this.showLocation = false;
            }
            else{
                for(let i=0; i<location.residents.length; i++){
                    const response = await axios.get(location.residents[i]);
                    this.characterList.push(response.data);
                }
                this.currentLocation = location;
                this.showLocation = true;
            }
        },
        async locationNames(){ 
            try {
                let list = [];
                const response = await axios.get(`https://rickandmortyapi.com/api/location/`);
                response.data.results.map(x=>!list.includes(x.name)? list.push(x.name): x);
                for(let i = 2; i<response.data.info.pages +1; i++){
                    const response2 = await axios.get(`https://rickandmortyapi.com/api/location/?page=${i}`);
                    response2.data.results.map(x=>!list.includes(x.name)? list.push(x.name): x); 
                }
                this.names = list;
            } catch (error) {
                console.log(error);
            }
        },  
        async locationType(){ 
            try {
                let list = [];
                const response = await axios.get(`https://rickandmortyapi.com/api/location/`);
                response.data.results.map(x=>!list.includes(x.type)? list.push(x.type): x);
                for(let i = 2; i<response.data.info.pages +1; i++){
                    const response2 = await axios.get(`https://rickandmortyapi.com/api/location/?page=${i}`);
                    response2.data.results.map(x=>!list.includes(x.type)? list.push(x.type): x); 
                }
                this.types = list;
            } catch (error) {
                console.log(error);
            }
        },  
        async locationDimension(){ 
            try {
                let list = [];
                const response = await axios.get(`https://rickandmortyapi.com/api/location/`);
                response.data.results.map(x=>!list.includes(x.dimension)? list.push(x.dimension): x);
                for(let i = 2; i<response.data.info.pages +1; i++){
                    const response2 = await axios.get(`https://rickandmortyapi.com/api/location/?page=${i}`);
                    response2.data.results.map(x=>!list.includes(x.dimension)? list.push(x.dimension): x); 
                }
                this.dimensions = list;
            } catch (error) {
                console.log(error);
            }
        }     
    },
    computed: {

    },
}
</script>
  
<style scoped>

</style>