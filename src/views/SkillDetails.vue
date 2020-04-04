<template>
  <div class="skill_details">

  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'SkillDetails',
  data(){
    return {
      users_with_this_skill: []
    }
  },
  mounted(){
    this.get_skill()
  },
  methods: {
    get_skill(){
      if(this.$route.query.name){

      }
    }
  }

}
</script>
