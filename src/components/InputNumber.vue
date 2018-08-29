<template>
  <div class="fe-input-number">
    <span role="button" class="fe-input-button fe-mius-button" @click="changeNumber('mius')">-</span>
    <input class="fe-input-box" v-model="number" @blur="onInput" />
    <span role="button" class="fe-input-button fe-plus-button" @click="changeNumber('plus')">+</span>
  </div>
</template>
<script>
export default {
  props: {
    value: {type: Number, required: true},
    min: {type: Number, default: -2^53},
    max: {type: Number, default: 2^53}
  },
  data () {
    return {
      number: this.getValidateNumber(this.value)
    }
  },
  watch: {
    value (newVal) {

    }
  },
  methods: {
    changeNumber (type) {
      this.number = type === 'mius' ? Math.max(--this.number, this.min) : Math.min(++this.number, this.max)
    },
    onInput () {
      if (typeof +this.number === 'number' && !isNaN(+this.number)) {
        this.number = this.getValidateNumber(this.number)
      } else {
        console.log(this.number)
      }
    },
    getValidateNumber (value) {
      return Math.min(Math.max(value, this.min), this.max)
    }
  }
}
</script>
<style lang="scss" scoped>
@import '../scss/colors.scss';

.fe-input-number {
  display: inline-flex;
  height: 36px;
  line-height: 36px;
  border: 1px solid $color-border;
  border-radius: 5px;
}
.fe-input-button {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 36px;
  height: 100%;
  font-size: 25px;
  cursor: pointer;
  user-select: none;
  background: $color-button;
  color: #666;
}
.fe-mius-button {
  border-right: 1px solid $color-border;
}
.fe-plus-button {
  border-left: 1px solid $color-border;
}
.fe-input-box {
  width: 100px;
  border: none;
  outline: none;
  text-align: center;
  font-size: 14px;
  color: #333;
}
</style>