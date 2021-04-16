<template>
  <div v-if="number.fio <= 5 && number.talon <= 5 && number.fio != 0 && number.talon != 0">
  <FIOTalon :arrowF = 'arrow.showF[0]' :arrowT = 'arrow.showT[0]' />
  </div>
  <div v-if="number.fio > 5 || number.talon > 5 || number.fio === 0 || number.talon === 0" >
  <div v-for="i of arrow.showT.length"  :key="i">
    <transition name="slide-fade">
      <Talon :arrow = 'arrow.showT[i-1]' v-if="i === slider.active" class="list"/>
    </transition>
  </div>
  <div v-for="i of arrow.showF.length"  :key="i">
    <transition name="slide-fade">
    <FIO :arrow = 'arrow.showF[i-1]' v-if="i + arrow.showT.length === slider.active" class="list"/>
    </transition>
  </div>
  </div>
  <!-- <button @click="add()">ADD</button> -->
    <transition name="fade">
    <div v-if="addNewItem === 1" class="newItem">
      Абитуриент с талоном <span>{{newItem.talon}}</span> подойдите к столу <span>{{newItem.number_of_table}}</span>
  </div>
  </transition>
</template>

<script>
import { reactive, onMounted, ref } from 'vue'
// import { onMounted, ref, reactive, computed } from 'vue'
// import { NEWCOLOR, REPEATCOLOR, OLDCOLOR } from './composables/Colors'
import { SHOW_TALON, DELAY_ADD_TALON, DELAY_UPDATE_LIST } from './composables/Time'
import FIO from './FIO.vue'
import Talon from './Talon.vue'
import FIOTalon from './FIOTalon.vue'

export default {
  components: {
    FIO,
    Talon,
    FIOTalon
  },
  setup () {
    const arrow = reactive({
      // list: [
      //   {
      //     talon: 'Юмаев Р.А.',
      //     number_of_table: '30',
      //     status: 0
      //   },
      //   {
      //     talon: 'Б15',
      //     number_of_table: '11',
      //     status: 2
      //   },
      //   {
      //     talon: 'Б10',
      //     number_of_table: '8',
      //     status: 0
      //   },
      //   {
      //     talon: 'Б10',
      //     number_of_table: '8',
      //     status: 0
      //   },
      //   {
      //     talon: 'Б16',
      //     number_of_table: '1',
      //     status: 0
      //   },
      //   {
      //     talon: 'M12',
      //     number_of_table: '20',
      //     status: 0
      //   },
      //   {
      //     talon: 'M12',
      //     number_of_table: '20',
      //     status: 0
      //   },
      //   {
      //     talon: 'Юмаев Р.А.',
      //     number_of_table: '30',
      //     status: 0
      //   },
      //   {
      //     talon: 'Юмаев Р.А.',
      //     number_of_table: '30',
      //     status: 0
      //   },
      //   {
      //     talon: 'Юмаев Р.А.',
      //     number_of_table: '30',
      //     status: 0
      //   },
      //   {
      //     talon: 'Юмаев Р.А.',
      //     number_of_table: '30',
      //     status: 0
      //   },
      //   {
      //     talon: 'Б17',
      //     number_of_table: '10',
      //     status: 0
      //   },
      //   {
      //     talon: 'Б8',
      //     number_of_table: '9',
      //     status: 0
      //   },
      //   {
      //     talon: 'M9',
      //     number_of_table: '10',
      //     status: 0
      //   },
      //   {
      //     talon: 'Юмаев Р.А.',
      //     number_of_table: '30',
      //     status: 1
      //   },
      //   {
      //     talon: 'Б15',
      //     number_of_table: '11',
      //     status: 2
      //   }
      //   // {
      //   //   talon: 'M9',
      //   //   number_of_table: '10',
      //   //   status: 0
      //   // },
      //   // {
      //   //   talon: 'Юмаев Р.А.',
      //   //   number_of_table: '30',
      //   //   status: 1
      //   // },
      //   // {
      //   //   talon: 'Б15',
      //   //   number_of_table: '11',
      //   //   status: 2
      //   // },
      //   // {
      //   //   talon: 'Юмаев Р.А.',
      //   //   number_of_table: '30',
      //   //   status: 1
      //   // },
      //   // {
      //   //   talon: 'Б15',
      //   //   number_of_table: '11',
      //   //   status: 2
      //   // },
      //   // {
      //   //   talon: 'M9',
      //   //   number_of_table: '10',
      //   //   status: 0
      //   // },
      //   // {
      //   //   talon: 'Юмаев Р.А.',
      //   //   number_of_table: '30',
      //   //   status: 1
      //   // },
      //   // {
      //   //   talon: 'Б15',
      //   //   number_of_table: '11',
      //   //   status: 2
      //   // },
      //   // {
      //   //   talon: 'Б15',
      //   //   number_of_table: '11',
      //   //   status: 2
      //   // },
      //   // {
      //   //   talon: 'Б10',
      //   //   number_of_table: '8',
      //   //   status: 0
      //   // }
      // ],
      newList: [],
      preShowF: [],
      preShowT: [],
      showF: [],
      showT: [],
      showMessage: [],
      activeMessage: {}
    })
    const number = reactive({
      fio: 0,
      talon: 0,
      tabloF: 0,
      tabloT: 0,
      allTablo: 0
    })
    const slider = reactive({
      count: 0,
      active: 1
    })
    const addNewItem = ref(0)
    const newItem = ref('')
    const data = ref('')
    const axios = require('axios')
    // https://tablo-83a92-default-rtdb.firebaseio.com/queue.json
    // https://api.sdo.mpgu.org/queue-ia
    const url = 'https://api.sdo.mpgu.org/queue-ia'
    const audio = new Audio(require('../sound/sound.mp3'))
    const username = 'adminsdotest'
    const password = 'Fjjj76654ghhd43'
    // const show = ref([])
    // const newNew = ref([])
    // const fioLength = computed({
    //   get: () => arrow.list.length
    // })
    function timeDelay () {
      return arrow.showMessage.length * 5000
    }
    function toArray (obj) {
      let i = 0
      for (i in obj) {
        if (obj[i]) {
          arrow.newList.push(obj[i])
        }
      }
    }
    function toArrays (data) {
      const reg = new RegExp(/[A-Za-zА-Яа-я]{2,}/)
      for (let i = 0; i < data.length; i++) {
        if (data[i].status === 1) {
          arrow.showMessage.push(data[i])
        }
        if (reg.test(data[i].talon)) {
          arrow.preShowF.push(data[i])
        } else {
          arrow.preShowT.push(data[i])
        }
      }
      number.fio = arrow.preShowF.length
      number.tabloF = Math.ceil(number.fio / 5)
      number.talon = arrow.preShowT.length
      number.tabloT = Math.ceil(number.talon / 10)
      number.allTablo = number.tabloF + number.tabloT
    }

    function divideArray (array, toArray, length) {
      const num = (array === arrow.preShowF) ? number.tabloF : number.tabloT
      for (let i = 0; i < num; i++) {
        toArray.push(array.slice(0, length))
        array.splice(0, length)
      }
    }
    function fillList (i) {
      if (i < arrow.showMessage.length) {
        setTimeout(() => {
          add(arrow.showMessage[i]).then(() => {
            console.log(arrow.showMessage[i])
            fillList(i + 1)
          })
        }, DELAY_ADD_TALON)
      }
    }
    async function add (item) {
      audio.play()
      addNewItem.value = 1
      newItem.value = item
      await new Promise(resolve => {
        setTimeout(() => {
          addNewItem.value = 0
          resolve('done')
        }, SHOW_TALON)
      })
    }

    // , {
    //     auth: {
    //       username: username,
    //       password: password
    //     }
    //   }

    function clearAll () {
      arrow.newList = []
      arrow.showF = []
      arrow.showT = []
      arrow.showMessage = []
    }

    onMounted(() => {
      let sec = 0
      setInterval(() => {
        console.log(sec)
        sec++
      }, 1000)
      function request () {
        let delay = DELAY_UPDATE_LIST + timeDelay()
        console.log('delay1', delay)
        clearAll()
        axios.get(url, {
          auth: {
            username: username,
            password: password
          }
        }).then(response => {
          data.value = response.data
          toArray(data.value)
          toArrays(arrow.newList)
          divideArray(arrow.preShowF, arrow.showF, 5)
          divideArray(arrow.preShowT, arrow.showT, 10)
          fillList(0)
          delay = DELAY_UPDATE_LIST + timeDelay()
          console.log('delay2', delay)
          setTimeout(request, delay)
        })
      }
      setTimeout(request, DELAY_UPDATE_LIST + timeDelay())
      setInterval(() => {
        slider.count = number.allTablo
        if (slider.active >= slider.count) {
          slider.active = 1
        } else {
          slider.active++
        }
      }, 10000)
    })
    return {
      arrow,
      number,
      add,
      slider,
      addNewItem,
      newItem,
      timeDelay
    }
  }
}
</script>

<style lang='sass' scoped>
@import '@/sass/style'
.list
  position: absolute
</style>
