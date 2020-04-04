<template>
  <div class="home">

    <div class="skills_wrapper">
      <div class="" v-for="skill in skills" v-bind:key="skill.skill.identity.low">
        <div class="">
          Skill: {{skill.skill.properties.name}}
        </div>

        <div class="">
          Profeciency: {{skill.relationship.properties.profeciency}}
        </div>

        <div class="">
          <button type="button" v-on:click="delete_relationship_to_skill(skill.skill)">bye</button>
        </div>

      </div>

    </div>

    <form class="" v-on:submit.prevent="create_skill()">
      <input type="search" list="skills" v-model="new_skill.properties.name" placeholder="skill">
      <!-- make this a number maybe -->
      <input type="text" v-model="new_skill_relationship.properties.profeciency" placeholder="profeciency">
      <input type="submit" name="">

      <datalist id="skills">
        <option
          v-for="skill in all_skills"
          v-bind:value="skill.properties.name"
          v-bind:key="skill.properties.name"/>
      </datalist>

      <div
        v-for="skill in all_skills"
        v-html="skill.properties.name"
        v-bind:key="skill.identity.low"/>
    </form>
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {

  },
  data(){
    return {

      all_skills: [],

      skills: [],

      new_skill: {
        properties: {
          name: ''
        }
      },
      new_skill_relationship: {
        properties: {
          profeciency: ''
        }
      }
    }
  },
  mounted(){
    this.get_all_skills()
    this.get_skills_of_user()
  },
  methods: {
    get_all_skills(){
      this.axios.get(`${process.env.VUE_APP_SKILL_MANAGER_URL}/all_skills`,{
        params: {
          employee_id: this.$route.query.id,
    }})
      .then(response => {
        this.all_skills.splice(0,this.all_skills.length)
        response.data.forEach((record) => {
          let skill = record._fields[record._fieldLookup['skill']]
          this.all_skills.push(skill)
        });
      })
      .catch(error => alert(error))
    },
    get_skills_of_user(){
      this.axios.get(`${process.env.VUE_APP_SKILL_MANAGER_URL}/skills_of_user`,{
        params: {
          employee_id: this.$route.query.id,
    }})
      .then(response => {
        this.skills.splice(0,this.skills.length)
        response.data.forEach((record) => {
          let skill = record._fields[record._fieldLookup['skill']]
          let relationship = record._fields[record._fieldLookup['relationship']]
          this.skills.push({
            skill: skill,
            relationship: relationship,
          })
        });

      })
      .catch(error => alert(error))
    },
    create_skill(){
      this.axios.post(`${process.env.VUE_APP_SKILL_MANAGER_URL}/create_skill`, {
        skill: this.new_skill,
        relationship: this.new_skill_relationship,
      })
      .then( () => {
        this.get_all_skills()
      })
      .catch(error => alert(error))
    },
    delete_relationship_to_skill(skill){
      this.axios.post(`${process.env.VUE_APP_SKILL_MANAGER_URL}/delete_relationship_to_skill`, {
        employee_id: this.$route.query.id,
        skill_id: skill.identity.low,
      })
      .then( () => {
        this.get_all_skills()
      })
      .catch(error => alert(error))
    }
  }
}
</script>
