<template>
    <header class="bg-at-light-green text-white">
        <nav class="container py-5 px-4 flex flex-col gap-4 items-center sm:flex-row">

            <div class="flex items-center gap-x-4">
                <img src="../assets/images/dumbbell-light.png" class="w-14" alt="Workou Logo">
                <h1 class="text-xl">Active Tracker</h1>
            </div>

            <ul class="flex flex-1 justify-end gap-x-10">
                <router-link class="cursor-pointer" :to="{ name: 'Home' }">Home</router-link>
                <router-link class="cursor-pointer" :to="{ name: 'Create' }">Create</router-link>
                <router-link class="cursor-pointer" :to="{ name: 'Login' }">Login</router-link>
                <li class="cursor-pointer">Logout</li>
            </ul>

        </nav>
    </header>
</template>

<script setup>
import { defineComponent } from 'vue'
import { RouterLink } from 'vue-router'
import Home from '../views/Home.vue';

defineComponent({
    RouterLink
})
</script>