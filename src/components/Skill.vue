<template>
  <div class="skill">

    <router-link
    class="skill_name"
      :to="{ name: 'skill', query: {id: skill.identity.low} }">
      {{skill.properties.name}}
    </router-link>

    <template v-if="relationship">
      <template v-if="!editable">
        <progress
          v-bind:value="relationship.properties.profeciency"
          min="0"
          max="100"/>
      </template>

      <template v-else>
        <input
          type="range"
          class="profeciency_range"
          v-model="profeciency_temp"
          v-on:change="update_profeciency()">

        <button
          type="button"
          v-on:click="$emit('deleted')">
          Delete
        </button>

      </template>
    </template>

    <template v-if="skill.employee_count">
      <account-icon />
      <span>{{skill.employee_count.low}}</span>
    </template>



  </div>
</template>

<script>
import AccountIcon from 'vue-material-design-icons/Account.vue';

export default {
  name: 'Skill',
  components: {
    AccountIcon
  },
  props: {
    skill: Object,
    relationship: Object,
    editable: Boolean,
  },
  data(){
    return {
      profeciency_temp: (this.relationship ? this.relationship.properties.profeciency : null)
    }
  },
  methods: {
    update_profeciency(){
      this.relationship.properties.profeciency = this.profeciency_temp
      this.$emit('update')
    },
    cancel_editing(){
      this.editing = false;
      this.$emit('cancel')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.skill {
  border: 1px solid #dddddd;
  border-radius: 5px;
  display: flex;
  align-items: center;
  margin: 5px;
}

.skill:hover {
  background-color: #eeeeee;
}

.skill > * {
  margin: 5px;
}

.skill_name {
  flex: 1 1 0;
  color: currentColor;
  text-decoration: none;
}

progress, input[type="range"] {
  flex: 1 1 0;
}

.skill button {
  flex: 0 0 10%;
}
</style>
