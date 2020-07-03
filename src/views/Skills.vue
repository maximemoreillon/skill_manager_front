<template>
  <div class="home">

    <h1>Skills</h1>

    <div class="skills_wrapper">
      <Skill
        class="skill"
        v-for="skill in sorted_skills"
        v-bind:key="skill.identity.low"
        v-bind:skill="skill"/>
    </div>


  </div>
</template>

<script>
// @ is an alias to /src
import Skill from '@/components/Skill.vue'

export default {
  name: 'Skills',
  components: {
    Skill,
  },
  data(){
    return {
      skills: [],
    }
  },
  mounted(){
    this.get_all_skills()
  },
  methods: {
    get_all_skills(){
      this.axios.get(`${process.env.VUE_APP_SKILL_MANAGER_URL}/skills`)
      .then(response => {
        this.skills.splice(0,this.skills.length)
        response.data.forEach((record) => {
          let skill = record._fields[record._fieldLookup['skill']]
          let employee_count = record._fields[record._fieldLookup['employee_count']]
          skill.employee_count = employee_count
          this.skills.push(skill)
        });
      })
      .catch(error => alert(error))
    },

  },
  computed: {
    sorted_skills(){
      return this.skills.slice().sort((a, b) => {
        return b.employee_count.low - a.employee_count.low
      });
    }
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
