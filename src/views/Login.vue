<template>
    <div class="max-w-screen-sm mx-auto px-4 py-10">
        <!-- Error Handling -->
        <div v-if="errorMessage" class="mb-10 p-4 rounded-md md-light-grey shadow-lg">
            <p class="text-red-500">{{ errorMessage }}</p>
        </div>

        <form @submit.prevent="login" class="p-8 flex flex-col bg-light-grey rounded-md shadow-lg">
            <h1 class="text-3xl text-at-light-green mb-4">Login</h1>
            <div class="flex flex-col mb-2">
                <label for="email" class="mb-1 text-sm text-at-light-green">Email</label>
                <input type="email" v-model="email" require class="p-2 text-gray-500 focus:outline-none" id="email">
            </div>
            <div class="flex flex-col mb-2">
                <label for="password" class="mb-1 text-sm text-at-light-green">Password</label>
                <input type="password" v-model="password" require class="p-2 text-gray-500 focus:outline-none"
                    id="password">
            </div>

            <button type="submit"
                class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green">Login</button>
            <router-link class="text-sm mt-6 text-center" :to="{
                name: 'Register'
            }">Não tem uma conta? <span class="text-at-light-green">Registar-se</span>
            </router-link>
        </form>
    </div>
</template>
<script >
import { ref } from 'vue';
import { supabase } from '../supabase/init';
import { useRouter } from 'vue-router';


export default {
    name: "Login",
    setup() {

        const router = useRouter();
        const email = ref(null);
        const password = ref(null);
        const errorMessage = ref(null);

        const login = async () => {
            try {
                const { data, error } = await supabase.auth.signInWithPassword({
                    email: email.value,
                    password: password.value
                });

                if (error) throw error;

                if (data) {
                    router.push({ name: 'Home' })
                }
            } catch (error) {
                errorMessage.value = `Error:  ${error.message}`;

                setTimeout(() => {
                    errorMessage.value = null;
                }, 5000);
            }
        }
        return { email, password, errorMessage, login };
    }
}
</script>
