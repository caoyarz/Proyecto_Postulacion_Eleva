<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <welcome-example :algo="unaVariable"></welcome-example>
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// Este es un componente que viene de muestra cuando se crea un nuevo proyecto con Vue,
// lo dejaré comentado porque hay links que te pueden servir...
// import HelloWorld from '@/components/HelloWorld.vue';

// Así se importa un componente:
import WelcomeExample from '../components/WelcomeExample.vue';

export default {
  name: 'home',
  components: {
    // HelloWorld,
    WelcomeExample,
  },
  data() {
    return {
      unaVariable: 'una cosa cualquiera',
    };
  },
};
</script>
