<template>
    <div class="place-content-center">
        <ul class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 2xl:grid-cols-5">
            <li v-for="(item, index) in this.list" :key="index">
                <Character :char="item" />
            </li>
        </ul>
    </div>
</template>
  
<script>
import Character from './Character.vue';

export default {
    name: 'Characters',
    components: {
        Character,
    },
    props: {
        list: {
            type: Array,
        },
    },
    data() {
        return {
        }
    },
    mounted() {
    },
    methods: {
    },
    computed: {

    },
}
</script>
  
<style scoped>

</style>
  