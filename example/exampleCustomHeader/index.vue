<template>
  <vue-table-page
    fixed-header
    show-overflow-tooltip
    :table-data="tableData"
    :headers="tableHeaders"
    :page="page">
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
  </vue-table-page>
</template>

<script>
import VueTablePage from '../../src/components/VueTablePage.vue'
import HeaderExample from './HeaderExample.vue'
import mockTableData from '../data/mockTableData.js'

export default {
  name: 'CustomHeader',
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

        sortable: true,
        type: 'date'
      }, {
        label: 'Name',
        value: 'firstname',

        sortable: true,
        format: this.formatName
      }, {
        label: 'Address',
        value: 'address'
      }],
      showMessage: true,
      message: 'This is an Example Page Using the Built in table and sortable headers for date and name. Starting on second page',
      page: { offset: 20, limit: 100 }
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
    },
    formatName (row) {
      return `${row.firstname ? row.firstname : '--'} ${row.lastname ? row.lastname : ' --'}`
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
