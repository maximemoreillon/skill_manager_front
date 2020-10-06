<template>
  <div class="about">
    <h1>Skill manager</h1>
    <p>
      Version {{version}}
    </p>
    <p>An application to get insight about the skills of users</p>
    <p>
      Developped and maintained by <a href="https://maximemoreillon.com">Maxime MOREILLON</a>
    </p>

    <h2>Technology stack</h2>
    <ul>
      <li>Neo4J</li>
      <li>Node.js</li>
      <li>Vue.js</li>
      <li>Docker</li>
      <li>Kubernetes</li>
    </ul>
    <h2>Source code</h2>
    <p>This application is open source, its source code is available on GitLab</p>
  </div>
</template>

<script>
// @ is an alias to /src


import pjson from '../../package.json'

export default {
  name: 'Home',
  components: {

  },
  data(){
    return {
      version: pjson.version




    }
  },
  mounted(){

  },

  methods: {

  },

}
</script>

<style scoped>


</style>
