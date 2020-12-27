<template>
  <FilterNav @filterChange="updateFilter" :current="current" />
  <div v-for="project in filteredPorjects" :key="project.id">
    <SingleProject
      :project="project"
      @delete="removeProject"
      @complete="updateComplete"
    />
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject';
import FilterNav from '../components/FilterNav';

export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav,
  },
  data() {
    return {
      projects: [],
      current: 'all',
    };
  },
  mounted() {
    fetch(`http://localhost:3000/projects`)
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.error(err));
  },
  methods: {
    removeProject(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },
    updateComplete(id) {
      let p = this.projects.find((project) => project.id === id);
      p.complete = !p.complete;
    },
    updateFilter($event) {
      this.current = $event;
    },
  },
  computed: {
    filteredPorjects() {
      return this.projects.filter((project) => {
        if (this.current === 'completed') {
          return project.complete;
        } else if (this.current === 'ongoing') {
          return !project.complete;
        } else {
          return project;
        }
      });
    },
  },
};
</script>
