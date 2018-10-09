<template>
  <div class="home-page">
    <!-- <InputNumber v-model="num" :min="0" :max="10" @change="onChange"></InputNumber> -->
    <div class="center">checklist demo</div>
    <div>
      <input type="text" placeholder="请选择考场" @focus="openCheckList">
    </div>
    <div class="m-v-10">
      <p v-for="(item, index) in kaochang" :key="index">{{ item.label }}</p>
    </div>
    <CheckList ref="checklist" :max="5" :checkboxLeft="true" :dataList="dataList" @change="onChange"></CheckList>
  </div>
</template>

<script>
import InputNumber from '../components/input-number/InputNumber'
import CheckList from '../components/check-list/CheckList'

export default {
  data () {
    return {
      num: 2,
      dataList: [{
        label: '科目二第07考点马路',
        value: '101',
        address: '上海市宝山区宝安公路2009号'
      },{
        label: '科目二第08考点沪松公路',
        value: '102',
        address: '上海市闵行区沪松公路565弄128号'
      },{
        label: '科目二第09考点七宝',
        value: '103',
        address: '上海市闵行区沪松公路200号'
      },{
        label: '科目二第09考点世纪公园世纪公园',
        value: '104',
        address: ''
      },{
        label: '科目二第09考点世纪公园',
        value: '105',
        address: '上海市浦东新区世纪大道200号'
      },{
        label: '科目二第09考点哈哈哈哈',
        value: '107'
      },{
        label: '科目二第09考点合川路地铁站',
        value: '106',
        address: '上海市合川路地铁站2号出口'
      }],
      kaochang: []
    }
  },
  mounted (){
    this.init()
  },
  methods: {
    init () {},
    onChange (num) {
      console.log(num)
    },
    openCheckList () {
      this.$refs.checklist.show()
    },
    onChange (value) {
      this.kaochang = value
    }
  },
  components: {
    InputNumber,
    CheckList
  }
}
</script>
<style lang="scss">
.center{
  text-align: center;
  font-size: 18px;
}
</style>