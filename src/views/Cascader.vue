<template>
  <div class="cascader"
       @click="toggleDropDownVisible(true)"
       v-clickoutside="() => toggleDropDownVisible(false)"
       @mouseenter="inputHover = true"
       @mouseleave="inputHover = false">
    <el-input class="el-select el-select--small"
              v-model.trim="inputValue"
              readonly>
      <template slot="suffix">
        <i v-if="clearBtnVisible && inputValue"
           key="clear"
           class="el-input__icon el-icon-circle-close"
           @click.stop="handleClear"></i>
        <i v-else
           key="arrow-down"
           :class="['el-input__icon', 'el-icon-arrow-down']"
           @click.stop="toggleDropDownVisible()"></i>
      </template>
    </el-input>
    <transition name="el-zoom-in-top">
      <div v-show="dropDownVisible"
           class="show-dropdoown">
        <Child v-for="(item, index) in childList"
               :key="index"
               :index="index"
               :list="formatList(index)"
               @openChild="openChild"></Child>
      </div>
    </transition>
  </div>
</template>
<script>
import Clickoutside from 'element-ui/src/utils/clickoutside'
import Child from './Child'
export default {
  directives: { Clickoutside },
  name: 'Cascader',
  components: { Child },
  props: {
    list: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      dropDownVisible: false, // 是否显示菜单层级
      inputValue: null,
      inputHover: false,
      childList: [] // 每个子级下标
    }
  },
  mounted () {
  },
  methods: {
    toggleDropDownVisible (visible) {
      if (visible !== this.dropDownVisible) {
        this.dropDownVisible = visible
        if (visible) {
          this.childList = ['0']

        }
      }
    },
    clearBtnVisible () {
      if (this.inputValue && !this.inputHover) return false
    },
    handleClear () {
      this.inputValue = null
    },
    formatList (index) {
      // 根据显示的层级，返回符合层级的列表数据

      return this.recursiveChildList(this.list, index)
    },
    // 递归列表数据
    recursiveChildList (list, index) {
      let _arr = []
      const fn = (arr, i) => {
        if (i < index) {
          fn(arr[this.childList[i]].children, i + 1)
        }
        else if (i === index) {
          _arr = arr[this.childList[i]].children
        } else {
          _arr = arr
        }

      }
      fn(list, 1)
      return _arr
    },
    openChild (itemIndex, index) {
      // itemIndex  选中面板某项的下标
      // index  选中面板的下标

      let _childList = JSON.parse(JSON.stringify(this.childList))
      _childList = _childList.splice(0, index)
      _childList.push(itemIndex)
      this.childList = _childList
    }
  }
}
</script>
<style lang="scss" scopd>
.cascader {
  .show-dropdoown {
    display: flex;
  }
}
</style>
