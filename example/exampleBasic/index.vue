<template>
  <vue-table-page
    title="Example Page"
    :message="message"
    :table-data="tableData"
    @table-change="tableChange"
    @page-change="changeTablesPage">
    <header-example
      slot="header-left"
      @staff-changed="staffChanged"/>
    <div slot="header-right">
      <el-button
        type="primary"
        class="sm-margin">
        Export
      </el-button>
    </div>
    <table-example
      slot="page-table"
      :table-data="tablePage" />
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
      tablePage: mockTableData,
      tableLength: mockTableData.length,
      message: 'This is an Example Page. There are over a hundred lines of data',
      page: { offset: 0, limit: 20 }
    }
  },
  methods: {
    changeTablesPage (page) {
      console.log('example 1: ', page)
    },
    handleOpen (key, keyPath) {
      console.log(key, keyPath)
    },
    handleClose (key, keyPath) {
      console.log(key, keyPath)
    },
    staffChanged (value) {
      if (value === 'allStaff') {
        this.tableData = mockTableData
      } else if (value === 'staffA') {
        this.tableData = [...mockTableData].splice(0, 90)
      } else if (value === 'staffB') {
        this.tableData = [...mockTableData].splice(91)
      }
      this.tableLength = this.tableData.length
      this.changeTablesPage(this.page)
    },
    tableChange (table) {
      this.tablePage = table
      console.log(table)
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
