<template>
  <NavBar  @selectSection="selectSection" />
  <div>
    <div v-if="section=='Characters'">
      <CharacterList />
    </div>
    <div v-else-if="section=='Location'">
      <LocationList />
    </div>
    <div v-else-if="section=='Episodes'">
      <EpisodesList />
  </div>
  </div>
  
</template>

<script>
import CharacterList from './components/CharacterList.vue';
import EpisodesList from './components/EpisodesList.vue';
import LocationList from './components/LocationList.vue';
import NavBar from './components/NavBar.vue';
export default {
    name: 'App',
    components: {
    CharacterList,
    NavBar,
    EpisodesList,
    LocationList
},
    data() {
        return {
          section: 'Characters'
        }
    },
    methods: {
      selectSection(sec){
        this.section=sec;
      }
    },
    computed: {

    },
}
</script>

<style scoped>

</style>
