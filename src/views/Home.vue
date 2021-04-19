<template>
  <div class="home">
    <cascader :list="list"></cascader>
  </div>
</template>

<script>
import Cascader from './Cascader'

export default {
  name: 'Home',
  components: { Cascader },
  data () {
    return {
      testOne: '',
      list: [
        {
          name: 'child_1',
          children: []
        },
        {
          name: 'child_2',
          children: [
            {
              name: 'child_2_1',
              children: []
            },
            {
              name: 'child_2_2',
              children: []
            }
          ]
        },
        {
          name: 'child_3',
          children: [
            {
              name: 'child_3_1',
              children: [
                {
                  name: 'child_3_1_1',
                  children: []
                },
                {
                  name: 'child_3_1_2',
                  children: [
                    {
                      name: 'child_3_1_2_1',
                      children: []
                    },
                    {
                      name: 'child_3_1_2_2',
                      children: []
                    }
                  ]
                }
              ]
            },
            {
              name: 'child_3_2',
              children: []
            }
          ]
        }
      ]
    }
  },
  methods: {}
}
</script>
<style lang="scss" scoped>
.home {
  width: 400px;
}
</style>
