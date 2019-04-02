<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Keeper</span>
        <span class="font-weight-light">QUARENTENA</span>
      </v-toolbar-title> 
      <v-spacer>
      </v-spacer>
      <v-btn @click="home()">
        Voltar
      </v-btn>
    </v-toolbar>

    <v-content>          
      <router-view/>    
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  methods: {
    home() {
      this.$router.push({name: 'home'});
    }
  },
  data () {
    return {
      //
    }
  }
}
</script>
