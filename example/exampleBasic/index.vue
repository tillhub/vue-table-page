<template>
  <vue-table-page
    title="Example Page"
    :message="message"
    :table-size="tableLength"
    @page-change="changeTablesPage">
    <header-example slot="header-left"/>
    <div slot="header-right">
      <el-button
        type="primary"
        class="sm-margin">
        Export
      </el-button>
    </div>
    <table-example
      slot="page-table"
      :table-data="tableData" />
  </vue-table-page>
</template>

<script>
import VueTablePage from '../../src/components/VueTablePage.vue'
import HeaderExample from './HeaderExample.vue'
import TableExample from './TableExample.vue'
import mockTableData from '../data/mockTableData.js'

export default {
  name: 'Basic',
  components: {
    VueTablePage,
    HeaderExample,
    TableExample
  },
  data () {
    return {
      tableData: mockTableData,
      tableLength: mockTableData.length,
      message: 'This is an Example Page. There are over a hundred lines of data'
    }
  },
  methods: {
    changeTablesPage (page) {
      const array = [...mockTableData]
      const newTable = array.splice(page.offset, page.limit)
      this.tableData = newTable
      console.log('example 1: ', page)
    },
    handleOpen (key, keyPath) {
      console.log(key, keyPath)
    },
    handleClose (key, keyPath) {
      console.log(key, keyPath)
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
</style>
