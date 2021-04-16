<template>
  <div class="header">
    <!-- <img src="../assets/logo.png" alt="logo" class="logo" /> -->
    <div class="annotation">
      <div class="annotation-item">
        <div class="annotation-arrow">
          <svg width="28" height="49" viewBox="0 0 28 49" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M27.9314 24.5002C27.9314 25.3783 27.5961 26.2564 26.9268 26.9259L5.85775 47.9948C4.51749 49.3351 2.3445 49.3351 1.00479 47.9948C-0.334928 46.6551 -0.334928 44.4825 1.00479 43.1422L19.6479 24.5002L1.00544 5.85803C-0.334277 4.51777 -0.334277 2.34543 1.00544 1.00582C2.34515 -0.335083 4.51814 -0.335083 5.8584 1.00582L26.9275 22.0744C27.5968 22.7442 27.9314 23.6223 27.9314 24.5002Z" :fill='newColor'/>
          </svg>
        </div>
        <div class="annotation-text">
          Новый
        </div>
      </div>
      <div class="annotation-item">
        <div class="annotation-arrow">
          <svg width="28" height="49" viewBox="0 0 28 49" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M27.9314 24.5002C27.9314 25.3783 27.5961 26.2564 26.9268 26.9259L5.85775 47.9948C4.51749 49.3351 2.3445 49.3351 1.00479 47.9948C-0.334928 46.6551 -0.334928 44.4825 1.00479 43.1422L19.6479 24.5002L1.00544 5.85803C-0.334277 4.51777 -0.334277 2.34543 1.00544 1.00582C2.34515 -0.335083 4.51814 -0.335083 5.8584 1.00582L26.9275 22.0744C27.5968 22.7442 27.9314 23.6223 27.9314 24.5002Z" :fill='repeatColor'/>
          </svg>
        </div>
        <div class="annotation-text">
          Повторный
        </div>
      </div>
      <div class="annotation-item">
        <div class="annotation-arrow">
          <svg width="28" height="49" viewBox="0 0 28 49" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M27.9314 24.5002C27.9314 25.3783 27.5961 26.2564 26.9268 26.9259L5.85775 47.9948C4.51749 49.3351 2.3445 49.3351 1.00479 47.9948C-0.334928 46.6551 -0.334928 44.4825 1.00479 43.1422L19.6479 24.5002L1.00544 5.85803C-0.334277 4.51777 -0.334277 2.34543 1.00544 1.00582C2.34515 -0.335083 4.51814 -0.335083 5.8584 1.00582L26.9275 22.0744C27.5968 22.7442 27.9314 23.6223 27.9314 24.5002Z" :fill='oldColor'/>
          </svg>
        </div>
        <div class="annotation-text">
          Старый
        </div>
      </div>
    </div>
    <div class="title">
      Табло учета достижений
    </div>
    <div class="date-wraper">
      <div class="date-wraper time">{{ hours }}:{{minutes}}</div>
      <div class="date-wraper date">{{ date }}</div>
    </div>
  </div>
</template>

<script>
import { onMounted, onBeforeUnmount, ref } from 'vue'
import { getZeroPad } from './composables/Filters'
import { NEWCOLOR, REPEATCOLOR, OLDCOLOR } from './composables/Colors'
export default {
  setup () {
    const hours = ref('')
    const minutes = ref('')
    const seconds = ref('')
    const date = ref('')
    const newColor = NEWCOLOR
    const repeatColor = REPEATCOLOR
    const oldColor = OLDCOLOR
    let timer = ''

    function updateTime () {
      const Data = new Date()
      hours.value = Data.getHours()
      minutes.value = Data.getMinutes()
      seconds.value = Data.getSeconds()
      if (hours.value < 10) {
        hours.value = '0' + hours.value
      }
      if (minutes.value < 10) {
        minutes.value = '0' + minutes.value
      }
      date.value = getZeroPad(Data.getDate()) + '.' + getZeroPad(Data.getMonth() + 1) + '.' + getZeroPad(Data.getFullYear())
    }

    onMounted(() => {
      timer = setInterval(updateTime, 1000)
    })
    onBeforeUnmount(() => {
      clearInterval(timer)
    })

    return {
      hours,
      minutes,
      seconds,
      date,
      timer,
      newColor,
      repeatColor,
      oldColor
    }
  }
}
</script>

<style lang="sass" scoped>
@import '@/sass/style'
</style>
