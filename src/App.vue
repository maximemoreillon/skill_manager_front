<template>
  <div id="app">
    <AppTemplate
      authenticate
      applicationName="Skill manager">

      <template v-slot:navigation>
        <router-link :to="{ name: 'home'}">My skills</router-link>
        <router-link :to="{ name: 'skills'}">Skill list</router-link>
        <router-link :to="{ name: 'about'}">About</router-link>
      </template>

    </AppTemplate>


  </div>
</template>
<script>
import AppTemplate from '@moreillon/vue_application_template_flex'

export default {
  name: 'app',
  components: {
    AppTemplate
  },
  data(){
    return{
      navigation : [
        {route: '/', label: 'My skills', icon: 'account-details'},
        {route: '/skills', label: 'Skills', icon: 'account-details'},
      ]
    }
  }
}
</script>

<style>
.flip-list-move {
  transition: transform 1s;
}
</style>
