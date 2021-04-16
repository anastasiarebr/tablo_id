<template>
  <div class="fio">
    <FIOColumn :arrow = 'arrow.slice(0, 5)' :widthF = '100' :fszTalon = '65' :fszTable = '120'/>
  </div>
</template>

<script>
// import { computed } from 'vue'
import FIOColumn from './FIOColumn.vue'
export default {
  components: { FIOColumn },
  props: ['arrow']

  // setup () {
  //   const show = arrow.list
  //   const show1 = show.slice(0, 4)
  //   const show2 = show.slice(5, show.length - 1)
  //   const fioLength = computed({
  //     get: () => show.length
  //   })

  //   return {
  //     fioLength,
  //     show1,
  //     show2
  //   }
  // }
}
</script>

<style lang="sass" scoped>
@import '@/sass/style'

</style>
