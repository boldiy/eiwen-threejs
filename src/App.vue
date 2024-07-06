<template>
  <div id="app">
    <Man/>
    <Building/>
  </div>
</template>

<script>
import Building from './components/building.vue'
import Man from './components/man.vue'

export default {
  name: 'App',
  components: {
    Building,Man
  }
}
</script>
