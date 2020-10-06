<template>
  <div id="app">
    <AppTemplate
      authenticate
      applicationName="Skill manager">

      <template v-slot:navigation>
        <router-link :to="{ name: 'home'}">
          <SchoolIcon />
          <span>My skills</span>
        </router-link>
        <router-link :to="{ name: 'skills'}">
          <FormatListBulletedIcon />
          <span>Skill list</span>

        </router-link>
        <router-link :to="{ name: 'about'}">
          <InformationOutlineIcon />
          <span>About</span>

        </router-link>
      </template>

    </AppTemplate>


  </div>
</template>
<script>
import AppTemplate from '@moreillon/vue_application_template_flex'

import SchoolIcon from 'vue-material-design-icons/School.vue';
import FormatListBulletedIcon from 'vue-material-design-icons/FormatListBulleted.vue';
import InformationOutlineIcon from 'vue-material-design-icons/InformationOutline.vue';


export default {
  name: 'app',
  components: {
    AppTemplate,
    SchoolIcon,
    FormatListBulletedIcon,
    InformationOutlineIcon,
  },
}
</script>

<style>
.flip-list-move {
  transition: transform 1s;
}
</style>
