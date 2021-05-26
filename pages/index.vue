<template>
<div class="container">
  <div>
      <weather />
      <apod />
      
  </div>
  <photos />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({})
</script>

<style>
.container {
  margin: 0 auto;
  padding: 5px;
  display: flex;
  justify-content: center;
  text-align: center;
  max-width: 1200px;
}

</style>
