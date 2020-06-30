<template>
  <div class="">
    <h1 v-if="skill">{{skill.properties.name}}</h1>
    <h1 v-else>Skill</h1>

    <h2>Users with this skill</h2>
    <div
      class="employee"
      v-for="user in sorted_users"
      v-bind:key="user.employee.identity.low">

      <span class="employee_name">{{user.employee.properties.display_name}}</span>


      <progress
        v-bind:value="user.relationship.properties.profeciency"
        min="0"
        max="100"/>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Skill',
  data(){
    return {
      skill: null,
      users_with_this_skill: []
    }
  },
  mounted(){
    this.get_skill()
  },
  methods: {
    get_skill(){
      if(this.$route.query.name){
        let url = `${process.env.VUE_APP_SKILL_MANAGER_URL}/skills/${this.$route.query.name}/employees`

        this.axios.get(url)
        .then( (response) => {
          let first_record = response.data[0]
          this.skill = first_record._fields[first_record._fieldLookup['skill']]

          response.data.forEach((record) => {
            let employee_node = record._fields[record._fieldLookup['employee']]
            let relationship_node = record._fields[record._fieldLookup['relationship']]

            this.users_with_this_skill.push({
              employee: employee_node,
              relationship: relationship_node
            })
          })


        })
        .catch(error => {
          if(error.response) console.log(error.response.data)
          alert(error)
        })
      }
    }
  },
  computed: {
    sorted_users(){
      return this.users_with_this_skill.slice().sort((a, b) => {
        return b.relationship.properties.profeciency - a.relationship.properties.profeciency
      });
    }
  }

}
</script>

<style scoped>
.employee {
  border: 1px solid #dddddd;
  border-radius: 5px;
  display: flex;
  align-items: center;
  margin: 5px;
}

.employee > * {
  margin: 5px;
}

.employee_name {
  flex: 1 1 0;
  color: currentColor;
  text-decoration: none;
}
.employee progress {
  flex: 1 1 0;
}
</style>
