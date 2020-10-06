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
}
</script>

<style>
.flip-list-move {
  transition: transform 1s;
}
</style>
