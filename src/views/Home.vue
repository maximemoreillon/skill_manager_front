<template>
  <div class="home">

    <div class="" v-if="user.properties">
      Showing skills of {{user.properties.name_kanji}}
    </div>

    <div class="skills_wrapper">

      <Skill
        class="skill"
        v-for="skill in skills"
        v-bind:key="skill.skill.identity.low"
        v-bind:skill="skill.skill"
        v-bind:relationship="skill.relationship"
        v-on:deleted="delete_relationship_to_skill(skill.skill)"
        v-on:update="update_profeciency(skill)"
        v-on:cancel="get_skills_of_user()"/>

    </div>

    <form class="new_skill_form" v-on:submit.prevent="create_skill()">
      <input
        class="new_skill_input"
        type="search"
        list="skills"
        v-model="new_skill.properties.name"
        placeholder="skill">

      <input
        type="range"
        class="profeciency_range"
        v-model="new_skill_relationship.properties.profeciency">

      <!-- make this a number maybe -->

      <input
        class="create_button"
        type="submit">

      <datalist id="skills">
        <option
          v-for="skill in all_skills"
          v-bind:value="skill.properties.name"
          v-bind:key="skill.properties.name"/>
      </datalist>

    </form>
  </div>
</template>

<script>
// @ is an alias to /src
import Skill from '@/components/Skill.vue'

export default {
  name: 'Home',
  components: {
    Skill,
  },
  data(){
    return {

      user: {},

      all_skills: [],

      skills: [],

      new_skill: {
        properties: {
          name: ''
        }
      },
      new_skill_relationship: {
        properties: {
          profeciency: 0
        }
      }
    }
  },
  mounted(){
    this.get_employee_information()
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
    get_employee_information(){
      this.user.loader = true;
      this.axios.post(`${process.env.VUE_APP_EMPLOYEE_MANAGER_URL}/get_employee`, {
        employee_id: this.$route.query.id,
      })
      .then(response => {
        this.user = response.data
      })
      .catch(error => alert(error))
      .finally(() => {this.user.loading = false})
    },

    get_skills_of_user(){
      this.axios.get(`${process.env.VUE_APP_SKILL_MANAGER_URL}/skills_of_user`,{
        params: {
          employee_id: this.$route.query.id,
        }})
      .then(response => {
        // delete all skills
        this.skills.splice(0,this.skills.length)

        response.data.forEach((record) => {
          let skill = record._fields[record._fieldLookup['skill']]
          let relationship = record._fields[record._fieldLookup['relationship']]
          this.skills.push({
            skill: skill,
            relationship: relationship,
          })
        });

        // ordering
        this.skills.sort((a, b) => {return b.relationship.properties.profeciency - a.relationship.properties.profeciency});

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
        this.get_skills_of_user()
      })
      .catch(error => alert(error))
    },

    update_profeciency(skill){
      this.axios.post(`${process.env.VUE_APP_SKILL_MANAGER_URL}/create_skill`, {
        skill: skill.skill,
        relationship: skill.relationship,
      })
      .then( () => {
        this.get_all_skills()
        this.get_skills_of_user()
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
        this.get_skills_of_user()
      })
      .catch(error => alert(error))
    }
  },
  computed: {

  }
}
</script>

<style scoped>
.skill, form{
  border: 1px solid #dddddd;
  border-radius: 5px;
  display: flex;
  align-items: center;
  margin: 5px;
}

.skill > *, form > * {
  margin: 5px;
}

.skill_name, .new_skill_input {
  flex: 1 1 0;
}

.skill meter, form .profeciency_range {
  flex: 1 1 0;
}

.skill button, form .create_button {
  flex: 0 0 10%;
}

</style>
