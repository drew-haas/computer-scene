<template>
  <div class="page-container home">
		<ComputerScene/>
    <!-- <div class="section hero-copy">
      <h1>Front-End <br>Web Development</h1>
      <p>My name is Drew, a Front-End Web Developer based out of San Francisco.</p>
    </div>

    <div class="section">
      <h2>Projects</h2>
    </div> -->
  </div>
</template>

<script>
// @ is an alias to /src
import ComputerScene from '@/components/ComputerScene.vue'

export default {
  name: 'Home',
  components: {
		ComputerScene
  }
}
</script>

<style scoped lang="scss">
.hero-copy {
  position: relative;
  padding-top: 130px;
}
</style>