<template>
  <div class="skill">

    <div class="skill_name">
      {{skill.properties.name}}
    </div>

    <template v-if="!editing">
      <progress
        v-bind:value="relationship.properties.profeciency"
        min="0"
        max="100"/>

      <button
        type="button"
        v-on:click="editing = true">
        Edit
      </button>
    </template>

    <template v-else>
      <input
        type="range"
        class="profeciency_range"
        v-model="relationship.properties.profeciency">

      <button
        type="button"
        v-on:click="update_profeciency()">
        Save
      </button>

      <button
        type="button"
        v-on:click="cancel_editing()">
        Cancel
      </button>

    </template>

    <button
      type="button"
      v-on:click="$emit('deleted')">
      bye
    </button>

  </div>
</template>

<script>
export default {
  name: 'Skill',
  props: {
    skill: Object,
    relationship: Object,
  },
  data(){
    return {
      editing: false,
    }
  },
  methods: {
    update_profeciency(){
      this.editing = false;
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

.skill > * {
  margin: 5px;
}

.skill_name {
  flex: 1 1 0;
}

.skill progress {
  flex: 1 1 0;
}

.skill button {
  flex: 0 0 10%;
}
</style>
