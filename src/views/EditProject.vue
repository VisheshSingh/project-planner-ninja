<template>
  <form @submit.prevent="handleSubmit">
    <label for="title">Title</label>
    <input type="text" v-model="title" required />
    <label for="details">Details</label>
    <textarea v-model="details" required />
    <input type="checkbox" v-model="complete" />
    <label for="complete">Complete</label>
    <button type="submit">Edit Project</button>
  </form>
</template>

<script>
export default {
  name: 'EditProject',
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      complete: null,
      uri: `http://localhost:3000/projects/${this.id}`,
    };
  },
  mounted() {
    fetch(this.uri)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
        this.complete = data.complete;
      })
      .catch((err) => console.log(err));
  },
  methods: {
    handleSubmit() {
      const project = {
        title: this.title,
        details: this.details,
        complete: this.complete,
      };

      fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(project),
      })
        .then(() => this.$router.push('/'))
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style>
input[type='checkbox'] {
  width: 16px;
  margin: 10px 10px 0 0;
}

label[for='complete'] {
  display: inline-block;
}
</style>
