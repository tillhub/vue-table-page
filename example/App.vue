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
        <span>Default Table</span>
      </el-menu-item>
      <el-menu-item
        index="3"
        @click="goTo">
        <i class="el-icon-document"/>
        <span>No Pagination</span>
      </el-menu-item>
      <el-menu-item
        index="4"
        @click="goTo">
        <i class="el-icon-document"/>
        <span>Custom Header</span>
      </el-menu-item>
      <el-menu-item
        index="5"
        @click="goTo">
        <i class="el-icon-document"/>
        <span>No Header</span>
      </el-menu-item>
    </el-menu>
    <basic-example v-if="shouldShow('1')"/>
    <with-table v-if="shouldShow('2')"/>
    <no-pagination v-if="shouldShow('3')"/>
    <custom-header v-if="shouldShow('4')"/>
    <no-header v-if="shouldShow('5')"/>
  </div>
</template>

<script>
import BasicExample from './exampleBasic/index.vue'
import WithTable from './exampleWithTable/index.vue'
import NoPagination from './exampleNoPagination/index.vue'
import CustomHeader from './exampleCustomHeader/index.vue'
import NoHeader from './noHeader/index.vue'
import Vue from 'vue'
import 'element-ui/lib/theme-chalk/index.css'
import enLocale from 'element-ui/lib/locale/lang/en'
import ElementUI from 'element-ui'

Vue.use(ElementUI, { enLocale })

export default {
  name: 'App',
  components: {
    BasicExample,
    WithTable,
    NoPagination,
    CustomHeader,
    NoHeader
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
    shouldShow (index) {
      return this.active === index
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
