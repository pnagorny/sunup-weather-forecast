<template>
    <header class="sticky top-0 bg-primary-color shadow-lg">
        <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
            
            <RouterLink :to="{ name: 'home' }">
                <div class="flex items-center gap-3">
                    <i class="fa-solid fa-sun text-2xl text-amber-600"></i>
                    <p class="text-2xl text-amber-600">SunUp Weather Forecast</p>
                </div>
            </RouterLink>


            <div class="flex gap-3 flex-1 justify-end">
                <i class="fa-solid fa-circle-info text-xl hover:text-secondary-color duration-150 cursor-pointer" @click="changeState"></i>
            </div>

        <BaseModalComp :mActive="mActive" @close-modal="changeState">
            <div class="text-black">
          <h1 class="text-2xl mb-1">About:</h1>
          <p class="mb-4">
            The Local Weather allows you to track the current and
            future weather of cities of your choosing.
          </p>
          <h2 class="text-2xl">How it works:</h2>
          <ol class="list-decimal list-inside mb-4">
            <li>
              Search for your city by entering the name into the
              search bar.
            </li>
            <li>
              Select a city within the results, this will take
              you to the current weather for your selection.
            </li>
          </ol>
        </div>
        </BaseModalComp>
        </nav>
    </header>
</template>

<script setup>
import {ref} from "vue";
import { RouterLink } from 'vue-router';
import BaseModalComp from './BaseModalComp.vue';

const mActive = ref(null);
const changeState = () => {
    mActive.value = !mActive.value;
};

</script>
