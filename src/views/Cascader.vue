<template>
  <div
    class="cascader"
    @click="() => toggleDropDownVisible(true)"
    v-clickoutside="() => toggleDropDownVisible(false)"
    @mouseenter="inputHover = true"
    @mouseleave="inputHover = false"
  >
    <el-input
      class="el-select el-select--small"
      v-model.trim="inputValue"
      readonly
    >
      <template slot="suffix">
        <i
          v-if="clearBtnVisible"
          key="clear"
          class="el-input__icon el-icon-circle-close"
          @click.stop="handleClear"
        ></i>
        <i
          v-else
          key="arrow-down"
          :class="['el-input__icon', 'el-icon-arrow-down']"
          @click.stop="toggleDropDownVisible()"
        ></i>
      </template>
    </el-input>
    <transition name="el-zoom-in-top">
      <div v-show="dropDownVisible" class="show-dropdoown">
        <Child
          v-for="(item, index) in childList"
          :key="index"
          :index="index"
          :list="formatList(index)"
          @openChild="openChild"
        ></Child>
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
      childList: ['0'] // 每个子级下标
    }
  },
  mounted () {},
  methods: {
    toggleDropDownVisible (visible) {
      if (visible !== this.dropDownVisible) {
        this.dropDownVisible = visible
        if (visible) {
          this.childList = ['0']
          this.$set(this.childList, '0', 0) // 打开下拉列表，展示最外级数据
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
      return this.recursiveFun(this.list, index)
    },
    // 递归列表数据
    recursiveFun (list, index) {
      console.log(this.childList.length, 'length======')
      const fn = (list, index) => {
        if (this.childList.length > 1) {
          console.log(222222222222, list.children)
          fn(list.children.length ? list.children : [])
        } else {
          return list[this.childList[index]]
        }
      }
      console.log(list, 'list-------------')
      this.list = list
      return this.list
    },
    openChild (itemIndex, index) {
      // itemIndex  选中面板某项的下标
      // index  选中面板的下标
      this.$set(this.childList, index, itemIndex)
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
