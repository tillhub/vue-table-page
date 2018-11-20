<template>
  <vue-table-page
    title="Example Page Using Default Table"
    :table-data="tableData"
    :headers="tableHeaders"
    :message="message"
    @page-change="changeTablesPage">
    <header-example slot="header-left"/>
    <div slot="header-right">
      <el-button
        type="primary"
        class="sm-margin">
        Export
      </el-button>
    </div>
  </vue-table-page>
</template>

<script>
import VueTablePage from '../../src/components/VueTablePage.vue'
import HeaderExample from './HeaderExample.vue'
import mockTableData from '../data/mockTableData.js'

export default {
  name: 'DefaultTable',
  components: {
    VueTablePage,
    HeaderExample
  },
  data () {
    return {
      tableData: mockTableData,
      tableLength: mockTableData.length,
      tableHeaders: [{
        label: 'Date',
        value: 'date',
        sortable: true
      }, {
        label: 'Name',
        value: 'name',
        sortable: true
      }, {
        label: 'Address',
        value: 'address'
      }],
      message: 'This is an Example Page Using the Built in table and sortable headers for date and name'
    }
  },
  methods: {
    changeTablesPage (page) {
      console.log('example 2: ', page)
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
