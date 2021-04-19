<template>
  <div class="cas-child">
    <el-scrollbar tag="ul">
      <li v-for="(item, index) in list"
          :key="index"
          @click="open(index)">
        {{ item.name }}
      </li>
    </el-scrollbar>
  </div>
</template>

<script>
export default {
  name: 'Child',
  props: {
    index: {
      type: Number,
      default: 0
    },
    list: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {}
  },
  methods: {
    open (index) {
      // console.log(this.index, index, '子this.index-----------')
      this.$emit('openChild', index, this.index + 1)
    }
  }
}
</script>
<style lang="scss" scopd>
.cas-child {
  width: 180px;
  border: 1px solid #ccc;
  li {
    text-align: left;
    list-style-type: none;
    line-height: 36px;
    margin: 0 10px;
    cursor: pointer;
    &:hover {
      color: red;
    }
  }
}
</style>
