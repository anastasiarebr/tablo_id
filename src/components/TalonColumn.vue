<template>
  <div class="body" :style="{width: width + '%'}">
<div class="titles">
      <div class="title">
        <div class="title-talon">Талон</div>
        <div class="title-table">№ стола</div>
      </div>
</div>
  <div class="main">
    <div class="column">
      <div class="column-items" >
        <div class="column-item " v-for="(talon, index) in arrow" :key="index" :class="(index < 5 && index % 2 === 0) || (index > 4 && index % 2 !== 0) ? 'bgdblue' : ''">
          <div class="person" :style="{fontSize: fsztln + 'px'}">{{talon.talon}}</div>
          <div class="arrow">
              <svg width="37" height="64" viewBox="0 0 37 64" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M36.4818 32.0002C36.4818 33.1472 36.0438 34.2941 35.1698 35.1685L7.65094 62.6871C5.9004 64.4376 3.0622 64.4376 1.31237 62.6871C-0.437458 60.9373 -0.437458 58.0996 1.31237 56.349L25.6626 32.0002L1.31322 7.65127C-0.436607 5.90073 -0.436607 3.06338 1.31322 1.31369C3.06306 -0.437698 5.90125 -0.437698 7.65179 1.31369L35.1706 28.8318C36.0448 29.7067 36.4818 30.8536 36.4818 32.0002Z" :fill="chooseColor(talon.status)"/>
              </svg>
          </div>
          <div class="table" :style="{fontSize: fsztbl + 'px'}">{{talon.number_of_table}}</div>
        </div>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  props: ['arrow', 'widthT', 'fszTalon', 'fszTable'],
  data () {
    return {
      width: this.widthT,
      fsztln: this.fszTalon,
      fsztbl: this.fszTable
    }
  },
  methods: {
    chooseColor (status) {
      if (status === 0) {
        return '#EB5757'
      } else if (status === 1) {
        return '#1E9EE6'
      } else {
        return '#27AE60'
      }
    }
  }
}
</script>

<style lang="sass" scoped>
@import '@/sass/style'

.title
  display: flex
  justify-content: space-between
  &-talon, &-table
    width: 445px
    text-align: center
</style>
