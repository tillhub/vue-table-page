<template>
  <div class="examples">
    <el-menu
      default-active="1"
      class="full-height top-index"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b">
      <el-menu-item
        index="1"
        @click="goTo">
        <i class="el-icon-document"/>
        <span>Basic Example</span>
      </el-menu-item>
      <el-menu-item
        index="2"
        @click="goTo">
        <i class="el-icon-document"/>
        <span>Example Two</span>
      </el-menu-item>
    </el-menu>
    <basic-example v-show="isBasicExample()"/>
  </div>
</template>

<script>
import BasicExample from './basicExample/index.vue'
import Vue from 'vue'

import 'element-ui/lib/theme-chalk/index.css'
import enLocale from 'element-ui/lib/locale/lang/en'
import ElementUI from 'element-ui'

Vue.use(ElementUI, { enLocale })

export default {
  name: 'App',
  components: {
    BasicExample
  },
  data () {
    return {
      active: '1'
    }
  },
  methods: {
    goTo (el) {
      this.active = el.index
    },
    isBasicExample () {
      return this.active === '1'
    }
  }
}
</script>

<style>
body {
  margin: 0px;
}

.examples {
  height: 100vh;
  width: 100vw;
  display: flex;
  overflow: hidden;
}

.full-height {
  height: 100vh;
}

.top-index {
  z-index: 100;
}
</style>
