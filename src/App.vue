<template>
  <div id="app">
    <AppTemplate
      applicationName="Skill manager"
      v-bind:navigation="navigation"/>
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
