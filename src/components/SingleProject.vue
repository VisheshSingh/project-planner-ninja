<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-icons">
            edit
          </span>
        </router-link>
        <span class="material-icons" @click="handleDelete">
          delete
        </span>
        <span class="material-icons tick" @click="handleComplete">
          done
        </span>
      </div>
    </div>
    <div v-show="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SingleProject',
  props: ['project'],
  data() {
    return {
      showDetails: false,
      uri: `http://localhost:3000/projects/${this.project.id}`,
    };
  },
  methods: {
    handleDelete() {
      fetch(this.uri, { method: 'DELETE' })
        .then(() => this.$emit('delete', this.project.id))
        .catch((err) => console.error(err));
    },
    handleComplete() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => this.$emit('complete', this.project.id))
        .catch((err) => console.error(err));
    },
  },
};
</script>

<style>
.project {
  background: #fff;
  margin: 20px 0 10px 0;
  padding: 20px;
  border-radius: 10px;
  border-left: 4px solid #e90074;
}

.project.complete {
  border-left: 4px solid #00ce89;
}

.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.material-icons {
  color: #bbb;
  margin-right: 10px;
  cursor: pointer;
}

.material-icons:hover {
  color: #777;
}

.project.complete .tick {
  color: #00ce89;
}
</style>
