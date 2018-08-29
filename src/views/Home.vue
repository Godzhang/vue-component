<template>
  <div class="home-page">
    <InputNumber v-model="num" :min="0" :max="10"></InputNumber>
  </div>
</template>

<script>
import InputNumber from '../components/InputNumber'

export default {
  data () {
    return {
      num: 2
    }
  },
  created (){
    
  },
  methods: {
    init () {}
  },
  components: {
    InputNumber
  }
}
</script>
