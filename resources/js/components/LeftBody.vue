<template>
  <div id="left">
    <img src="../images/hamburger.png" />

    <div class="profile">
      <h1>Hi User</h1>
      <h2>Let's make project more amazing</h2>
    </div>

    <div class="search">
      <input placeholder="Search your project" />
      <img src="../images/search.png" />
    </div>

    <div class="project">
      <h3>Projects<span>(18)</span></h3>
      <div class="projects">
        <div class="a-project">
          <div class="box-color">
            <img src="../images/overlay.png" />
            <span>PT</span>
          </div>

          <h6>Development</h6>
        </div>

        <div class="a-project">
          <div class="box-color">
            <img src="../images/overlay.png" />
            <span>PT</span>
          </div>

          <h6>Design</h6>
        </div>

        <div class="a-project">
          <div class="box-color">
            <img src="../images/overlay.png" />
            <span>PT</span>
          </div>

          <h6>Testing</h6>
        </div>

        <div class="a-project">
          <div class="box-color">
            <img src="../images/overlay.png" />
            <span>PT</span>
          </div>

          <h6>Medium Draft</h6>
        </div>

        <div class="a-project">
          <div class="box-color">
            <img src="../images/overlay.png" />
            <span>PT</span>
          </div>

          <h6>Landing Page</h6>
        </div>

        <div class="a-project">
          <div class="box-color">
            <img src="../images/plus.png" />
            <span>PT</span>
          </div>

          <h6>Add Project</h6>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
    };
  },
  methods: {
    getUsers() {
      axios.get("api/users").then((response) => {
        this.users = response.data;
      });
    },
    mounted() {
      this.getUsers();
    },
  },
};
</script>

<style>
</style>