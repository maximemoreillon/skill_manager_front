<template>
  <div class="home">

    <template v-if="user.properties">
      <h1>Skills of {{user.properties.display_name}}</h1>
      <p>
        <a :href="`${employee_manager_front_url}/?id=${user.identity.low}`">{{user.properties.display_name}}'s profile</a>
      </p>

    </template>
    <h1 v-else>Skills</h1>

    <template v-if="user_is_current_user">
      <h2>新しいスキル</h2>

      <form class="new_skill_form" v-on:submit.prevent="create_skill()">
        <input
          class="new_skill_input"
          type="search"
          list="skills"
          v-model="new_skill.name"
          placeholder="新しいスキル / New skill">

        <input
          type="range"
          class="profeciency_range"
          v-model="new_skill.profeciency">

        <input
          :disabled="new_skill.name === ''"
          class="create_button"
          type="submit">

        <datalist id="skills">
          <option
            v-for="skill in all_skills"
            v-bind:value="skill.properties.name"
            v-bind:key="skill.properties.name"/>
        </datalist>

      </form>
    </template>


    <div class="skills_wrapper">

      <h2>あるスキル</h2>

      <Loader v-if="loading" >Loading skills</Loader>
      <template v-if="!loading">
        <transition-group name="flip-list" tag="div">

          <Skill
            class="skill"
            v-bind:editable="user_is_current_user"
            v-for="skill in sorted_skills"
            v-bind:key="skill.skill.identity.low"
            v-bind:skill="skill.skill"
            v-bind:relationship="skill.relationship"
            v-on:deleted="delete_relationship_to_skill(skill.skill)"
            v-on:update="update_profeciency(skill)"
            v-on:cancel="get_skills_of_user()"/>
        </transition-group>
      </template>

    </div>


  </div>
</template>

<script>
// @ is an alias to /src
import Skill from '@/components/Skill.vue'
import Loader from '@moreillon/vue_loader'

export default {
  name: 'Home',
  components: {
    Skill,
    Loader,
  },
  data(){
    return {

      user: {},

      all_skills: [],

      skills: [],
      loading: false,

      new_skill: {
        name: '',
        profeciency: 50,
      },

      employee_manager_front_url: process.env.VUE_APP_EMPLOYEE_MANAGER_FRONT_URL



    }
  },
  mounted(){
    this.get_employee_information()
    this.get_all_skills()
    this.get_skills_of_user()
  },
  beforeRouteUpdate (to, _, next) {

    // not sure why this is needed here
    this.skills.splice(0,this.skills.length)

    next()

    this.$nextTick().then(() => {
      this.get_employee_information()
      this.get_all_skills()
      this.get_skills_of_user()
    })
  },
  methods: {
    get_all_skills(){
      this.axios.get(`${process.env.VUE_APP_SKILL_MANAGER_URL}/skills`)
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
      let user_id = this.$route.query.id || 'self'
      this.axios.get(`${process.env.VUE_APP_EMPLOYEE_MANAGER_URL}/employees/${user_id}`)
      .then(response => {
        let record = response.data[0]
        this.user = record._fields[record._fieldLookup['employee']]
      })
      .catch(error => alert(error))
      .finally(() => {this.user.loading = false})
    },

    get_skills_of_user(){
      let user_id = this.$route.query.id || 'self'
      let url = `${process.env.VUE_APP_SKILL_MANAGER_URL}/employees/${user_id}/skills`

      this.loading = true;

      this.axios.get(url)
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
        this.skills.sort((a, b) => {
          return b.relationship.properties.profeciency - a.relationship.properties.profeciency
        })

      })
      .catch(error => alert(error))
      .finally(() => this.loading = false)
    },
    create_skill(){
      let user_id = this.$route.query.id || 'self'
      let url = `${process.env.VUE_APP_SKILL_MANAGER_URL}/employees/${user_id}/skills`

      this.axios.post(url, {
        skill_name: this.new_skill.name,
        profeciency: this.new_skill.profeciency,
      })
      .then( () => {
        this.get_all_skills()
        this.get_skills_of_user()

        // Reset inputs
        this.new_skill.name = ''
        this.new_skill.profeciency = 50

      })
      .catch(error => {
        if(error.response) console.log(error.response.data)
        alert(error)
      })
    },

    update_profeciency(skill){
      let user_id = this.$route.query.id || 'self'
      let url = `${process.env.VUE_APP_SKILL_MANAGER_URL}/employees/${user_id}/skills`

      this.axios.post(url, {
        skill_name: skill.skill.properties.name,
        profeciency: skill.relationship.properties.profeciency,
      })
      .then( () => {
        //this.get_skills_of_user()
      })
      .catch(error => alert(error))
    },

    delete_relationship_to_skill(skill){
      let user_id = this.$route.query.id || 'self'
      let url = `${process.env.VUE_APP_SKILL_MANAGER_URL}/employees/${user_id}/skills/${skill.identity.low}`

      this.axios.delete(url)
      .then( () => {
        this.get_all_skills()
        this.get_skills_of_user()
      })
      .catch(error =>{
        if(error.response) console.log(error.response.data)
         alert(error)
       })
    }
  },
  computed: {
    user_is_current_user(){
      return !this.$route.query.id
    },
    sorted_skills(){
      return this.skills.slice().sort((a, b) => {
        return b.relationship.properties.profeciency - a.relationship.properties.profeciency
      });
    }
  }
}
</script>

<style scoped>
a {
  text-decoration: none;
  font-weight: bold;
  color: #c00000;
}
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
