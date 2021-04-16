<template>
  <div class="fio-talon">
    <FIOColumn :arrow = 'arrowF' :widthF = '65' :fszTalon = '45' :fszTable = '100' />
    <TalonColumn :arrow = 'arrowT' :widthT = '35' :fszTalon = '80' :fszTable = '100' />
  </div>
</template>

<script>
import TalonColumn from './TalonColumn.vue'
import FIOColumn from './FIOColumn.vue'
export default {
  components: {
    TalonColumn,
    FIOColumn
  },
  props: ['arrowF', 'arrowT']

}
</script>

<style lang="sass" scoped>
@import '@/sass/style'
.fio-talon
  display: flex
  justify-content: space-between
  .title
    &-table
      width: 48%
.column
  &-item
    width: 1735px
</style>
