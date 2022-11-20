<template>

    <div class=" w-full md:block md:w-auto border border-cyan-200 rounded-lg" id="navbar-default">
        <div class="container justify-between flex flex-wrap mx-auto">
            <a class="flex items-center justify-self-start	">
                <img src="./icons/rickandmorty.png" class="h-9 sm:h-16" alt="Flowbite Logo" />
             </a>      
        <ul
        class="flex justify-self-center flex-row p-3 md:space-x-8  md:text-xl md:font-medium ">
            <li>
                <button @click="selectSection('Characters')"
                    class="block py-2 pl-3 pr-4 text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">
                    Characters</button>
            </li>
            <li>
                <button @click="selectSection('Location')"
                    class="block py-2 pl-3 pr-4 text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">
                    Location</button>
            </li>
            <li>
                <button @click="selectSection('Episodes')"
                    class="block py-2 pl-3 pr-4 text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">
                    Episodes</button>
            </li>
        </ul>
        </div>
        
    </div>

</template>
    
<script>

export default {
    name: 'NavBar',
    props: {
    },
    emits: ["selectSection"],
    methods: {
        selectSection(section){
            this.$emit('selectSection', section);
        }
    },
}
</script>
    
<style scoped>

</style>






