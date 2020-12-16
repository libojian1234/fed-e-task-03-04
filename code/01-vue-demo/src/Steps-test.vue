<template>
  <div>
      <steps :count="count" :active="active"></steps>
      <button @click="next">下一步</button>
  </div>
</template>

<script>
import steps from './Steps.vue'
export default {
    components: {
        steps
    },
    data () {
        return {
            count: 3,
            active: 0
        }
    },
    methods: {
        next () {
            this.active++
            if (this.active > this.count) {
                this.active = 0
            }
        }
    }
}
</script>

<style>

</style>