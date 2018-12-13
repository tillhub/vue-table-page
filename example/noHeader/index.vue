<template>
  <vue-table-page
    :table-data="tableData"
    @table-change="tableChange"
    @page-change="changeTablesPage">
    <table-example
      slot="page-table"
      :toggle-sort="toggleSort"
      :table-data="tablePage"
      @sort-change="sortChange"/>
  </vue-table-page>
</template>

<script>
import VueTablePage from '../../src/components/VueTablePage.vue'
import TableExample from './TableExample.vue'
import mockTableData from '../data/mockTableData.js'

export default {
  name: 'NoHeader',
  components: {
    VueTablePage,
    TableExample
  },
  data () {
    return {
      tableData: mockTableData,
      tablePage: mockTableData,
      tableLength: mockTableData.length,
      toggleSort: null,
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
      this.toggleSort = !this.toggleSort
      this.changeTablesPage(this.page)
    },
    tableChange (table) {
      console.log(table)
      this.tablePage = table
    },
    sortChange (info) {
      const compareRow = function (a, b) {
        if (info.order === 'ascending') {
          return a[info.prop] > b[info.prop] ? 1 : -1
        } else {
          return a[info.prop] < b[info.prop] ? 1 : -1
        }
      }
      this.tableData = [...this.tableData].sort(compareRow)
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
