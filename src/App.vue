<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700");
</style>

<template>
  <div v-if="appReady" class="min-h-full font-Poppins box-border">
    <Navigation />
    <RouterView />
  </div>
</template>

<script>
import { RouterView } from 'vue-router';
import Navigation from './components/Navigation.vue';
import { ref } from 'vue'
import { supabase } from './supabase/init'
import authStore from './stores/auth';

export default {
  components: { Navigation },
  setup() {
    const appReady = ref(null);

    const { user } = supabase.auth.getUser()

    if (!user) {
      appReady.value = true;
    }

    supabase.auth.onAuthStateChange((event, session) => {
      if (event === "SIGNED_IN") {
        authStore.methods.setUser(session);
        appReady.value = true;
      }
    })

    return { appReady }
  }
}
</script>


