<template>
    <div class="place-content-center">
    <div>
        <p class="text-cyan-200">Filter by name</p>
        <input type="text" v-model.trim="filter" list="names" @keydown.enter="filterByName" />
        <datalist id="names">
            <option v-for="item in this.names">{{item}}</option>
        </datalist>
    </div>
    <div class="place-content-center">
        <ul class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 2xl:grid-cols-5">
            <li v-for="(item, index) in this.list" :key="index">
                <Character :char="item" />
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
</template>
  
<script>
import axios from 'axios';
import Character from './Character.vue';

export default {
    name: 'CharacterList',
    components: {
        Character,
    },
    props: {
        episode: {
            type: Object,
        },
    },
    data() {
        return {
            filter: null,
            page: 1,
            maxPage: null,
            list: [],
            names: [],
        }
    },
    mounted() {
        this.fillListAll();
        this.characterNames();
    },
    methods: {
        async fillListAll() {
            try {
                const response = await axios.get(`https://rickandmortyapi.com/api/character/?page=${this.page}`);
                this.list = response.data.results;
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
                const response = await axios.get(`https://rickandmortyapi.com/api/character/?name=${this.filter}`);
                this.list = response.data.results;
                console.log(this.list);
            } catch (error) {
                console.log(error);
            }
        },
        async characterNames(){ 
            try {
                let list = [];
                const response = await axios.get(`https://rickandmortyapi.com/api/character/`);
                response.data.results.map(x=>!list.includes(x.name)? list.push(x.name): x);
                for(let i = 2; i<response.data.info.pages +1; i++){
                    const response2 = await axios.get(`https://rickandmortyapi.com/api/character/?page=${i}`);
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
  