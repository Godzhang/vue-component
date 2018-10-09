<template>
  <div class="cl-checklist">
    <div class="checklist" :class="{'show': isOpen}">
      <div class="topbar">
        <div class="cancel" @click="hide">取消</div>
        <div class="title">选择考场</div>
        <div class="confirm" @click="onConfirm">确定</div>
      </div>
      <div class="desc">您已选中{{ checkboxValue.length }}个, 最多可选{{ max }}个</div>
      <div class="list" ref="list">
        <div class="line-wrapper" v-for="(item, index) in dataList" :key="index" :data-val="item.value">
          <label :for="index" class="line border-1px" :class="{'checkbox-left': checkboxLeft}">
            <div class="l">
              <div class="title">{{ item.label }}</div>
              <div class="address" v-if="item.address">{{ item.address }}</div>
            </div>
            <div class="r"></div>
          </label>
          <input type="checkbox" :id="index" @click="selectItem($event)" v-model="checkboxValue" style="display: none;" :value="item.label + '|' + item.value">
        </div>
      </div>
    </div>
    <div class="checklist-overlay" v-if="isOpen" @click="hide"></div>
  </div>
</template>
<script>
export default {
  props: {
    checkboxLeft: {type: Boolean, default: false},
    maxHeight: {type: Number, default: 300},
    max: {type: Number, default: 0},
    dataList: {type: Array, required: true, default: () => []}
  },
  data () {
    return {
      checkboxValue: [],
      isOpen: false
    }
  },
  watch: {
    checkboxValue (val) {
      let listDom = this.$refs.list
      let lines = listDom.querySelectorAll('.line-wrapper')
      if (val.length === this.max){
        let item = null
        for (let i = 0, len = lines.length; i < len; i++) {
          item = lines[i]
          let dataVal = +lines[i].dataset.val
          if (val.indexOf(dataVal) === -1) {
            item.children[0].classList.add('disabled')
            item.querySelector('input[type="checkbox"]').setAttribute('disabled', 'disabled')
          }
        }
      } else {
        let item = null
        for (let i = 0, len = lines.length; i < len; i++) {
          item = lines[i]
          if (item.children[0].classList.contains('disabled')) {
            item.children[0].classList.remove('disabled')
            item.querySelector('input[type="checkbox"]').removeAttribute('disabled')
          }
        }
      }
    }
  },
  methods: {
    show () {
      document.activeElement.blur()
      this.isOpen = true
    },
    hide () {
      this.isOpen = false
    },
    selectItem (event) {
      let labelNode = event.target.previousElementSibling
      let classList = labelNode.classList
      classList.contains('selected') ? classList.remove('selected') : classList.add('selected')
    },
    onConfirm () {
      this.isOpen = false
      let checkboxValue = this.checkboxValue
      let res = []
      for (let i = 0, len = checkboxValue.length; i < len; i++) {
        let resObj = {}
        let item = checkboxValue[i].split('|')
        resObj.label = item[0]
        resObj.value = item[1]
        res.push(resObj)
      }
      this.$emit('change', res)
    }
  }
}
</script>
<style lang="scss" scoped>
.cl-checklist {
  overflow: hidden;
  .checklist {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    z-index: 2000;
    background-color: #fff;
    transform: translateY(100%);
    transition: all .5s;
    &.show {
      transform: translateY(0%);
    }
  }
  .checklist-overlay {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    z-index: 1000;
    background: rgba(0, 0, 0, .5);
    transition: all .5s;
  }
}

.topbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 45px;
  font-size: 16px;
  padding: 0 13px;
  border-bottom: 1px solid rgba(217, 217, 217, 1);
  .cancel {
    color: rgb(159, 159, 159);
  }
  .confirm {
    color: rgb(46, 166, 242);
  }
}
.desc {
  height: 30px;
  line-height: 30px;
  padding-right: 10px;
  font-size: 14px;
  text-align: right;
  color: rgb(159, 159, 159);
}
.list {
  max-height: 300px;
  font-size: 14px;
  padding: 10px 13px;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  overflow-scrolling: touch;
  .line {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 50px;
    .l {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: flex-start;
      width: 90%;
      .address {
        position: relative;
        padding-left: 15px;
        margin-top: 5px;
        color: rgb(159, 159, 159);
        &::before {
          content: ' ';
          position: absolute;
          width: 15px;
          height: 15px;
          top: 0;
          left: 0;
          background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAbCAYAAAB836/YAAABu0lEQVRIiaXUzU8TQRjH8U9fpAkXIZqItHghxEgietuD/gP84R420QSDcCDxIq6KaCoJYCuaeJhdu93OlILfpEnnmWd++7zMM63d3V0RuljHQ9xFr7SP8QNfUOBPlmUzB5tsYBtLkb0eHpS/JzjEcd2hs7W1Vf1v4TkeoxMLu0EHa0VRLBdFcTIYDEC75vCsjO6mbGCnWlSCg1uKVTzK83xAqGFHqFmMX3iP7+X6HjbF67ud5/nnLvomXaxziVcY1WxDfMQLLDf8e+i3sZaIbq8hVjHC28SZtTZWIhs/TdKM8S3xsZW2eLrjOWIVMcFeO2Ik1Kc1R6xltoYI1+Y8Yl8Sxi7Funinz9s4SRzaEa/vKp4mzpx0hSHfjGzewUt8EpoA98voUuUoujgT7tdqxKEl3NN+QqDOMMuys6opRwscuI4jJrP8VYjytgxLjanX5uA/BA+qh7YuOBQadFOKLMv+Zde82Ae4uoHYlUZmTcGx8KwvymGWZVNjGhu9DzhdQOy09J0iNct75qd+VfrMkBIcYX+O4L74a5MUJHT8OGI/Nuc2zBOEd7iorS9KW5LrBH/jjZDeCK9LW5K/QatiGcsSFOsAAAAASUVORK5CYII=");
          background-repeat: no-repeat;
          background-size: contain;
          background-position: 0;
        }
      }
    }
    .r {
      position: relative;
      width: 20px;
      height: 20px;
      margin: 0 5px;
      border-radius: 50%;
      border: 1px solid #9e9e9e;
      background-color: #fff;
      z-index: 0;
    }
    &.selected {
      .l .title {
        color: #1799fa;
      }
      .r {
        border: 1px solid #1799fa;
        background-color: #1799fa;
        &::before {
          content: ' ';
          position: absolute;
          top: 4px;
          left: 4px;
          width: 12px;
          height: 12px;
          background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABUAAAAPCAYAAAALWoRrAAAA90lEQVQ4ja3TMSuFURgH8GeQRGIwy6BkUbIYlHwBu0Umi8VkMVlMJoMvIcNdDAYlJuULWCSESBSLwc9we/P2eu715t6nznKe//Or0zknEF1YS3jAHRa7Aa7iy0/ddAquVUC47ARcT8APLPwX3PC73jGPCPRiGSvoqwFuJuAb5opMoFFqnmCwDbiVgK+YLecCn5XQGYYScDsBXzBTzQb2k/A5hkvBnSTzhOnsRIEBHCdDFxjBbtJ7xFQGFmigH0fJ8HOyd4/JVmAZDc2bP0yQct1ioh1YRYvn1cg0XGP8LzBDC/igAl5hrA7YCg30YE/zl5xitC6I+AYJmBaJbbKurAAAAABJRU5ErkJggg==");
          background-repeat: no-repeat;
          background-size: contain;
          background-position: center center;
          z-index: 1;
        }
      }
    }
    &.disabled {
      .l .title {
        color: #9e9e9e;
      }
      .r {
        border: 1px solid #9d9e9e;
        background-color: #9e9e9e;
      }
    }
    &.checkbox-left {
      flex-direction: row-reverse;
    }
  }
}

.border-1px {
  position: relative;
  &::after {
    content: ' ';
    display: block;
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    border-bottom: 1px solid rgb(217, 217, 217);
    @media (-webkit-min-device-pixel-ratio: 1.5), (min-device-pixel-ratio: 1.5) {
      -webkit-transform: scaleY(0.7);
      transform: scaleY(0.7);
    }
    @media (-webkit-min-device-pixel-ratio: 2), (min-device-pixel-ratio: 2) {
      -webkit-transform: scaleY(0.5);
      transform: scaleY(0.5);
    }
  }
}

</style>