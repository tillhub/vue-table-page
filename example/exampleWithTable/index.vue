<template>
  <vue-table-page
    title="Example Page Using Default Table"
    :table-data="tableData"
    :headers="tableHeaders"
    :message="message"
    :page="page">
    <header-example
      slot="header-left"
      @staff-changed="staffChanged"/>
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
      tableHeaders: [{
        label: 'Date',
        value: 'date',
        width: '100',
        sortable: true,
        fixed: 'left',
        isDate: true
      }, {
        label: 'Name',
        value: 'name',
        minWidth: '100',
        sortable: true
      }, {
        label: 'Address',
        width: '500',
        value: 'address'
      }, {
        label: 'Address 2',
        width: '500',
        value: 'address'
      }, {
        label: 'Address 3',
        width: '500',
        value: 'address'
      }, {
        label: 'Address 4',
        width: '500',
        value: 'address'
      }, {
        label: 'Address 5',
        width: '500',
        value: 'address'
      }],
      message: 'This is an Example Page Using the Built in table and sortable headers for date and name. Starting on second page',
      page: { offset: 20, limit: 50 }
    }
  },
  methods: {
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
        this.tableData = [...mockTableData].splice(0, 70)
      } else if (value === 'staffB') {
        this.tableData = [...mockTableData].splice(71)
      }
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

.sm-margin{
  margin: 10px
}
</style>
