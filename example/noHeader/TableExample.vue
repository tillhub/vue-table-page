<template>
  <div>
    <el-table
      :data="tableData"
      style="width: 100%"
      ref="exampleBasic"
      @sort-change="$emit('sort-change', $event)">
      <el-table-column
        sortable="custom"
        :sort-orders="['ascending', 'descending']"
        prop="date"
        label="Date"
        :formatter="formatDate"
        width="180" />
      <el-table-column
        sortable="custom"
        :sort-orders="['ascending', 'descending']"
        prop="firstname"
        label="Name"
        :formatter="formatName"
        width="180" />
      <el-table-column
        prop="address"
        label="Address" />
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'NoHeader',
  props: {
    tableData: {
      type: Array,
      required: true
    },
    toggleSort: {
      type: Boolean || null,
      default: null
    }
  },
  methods: {
    formatName (row, column, cellValue, index) {
      return `${row.firstname ? row.firstname : '--'} ${row.lastname ? row.lastname : ' --'}`
    },
    formatDate (row, column, cellValue, index) {
      return cellValue ? new Date(cellValue).toLocaleDateString('en-US') : '--'
    }
  },
  watch: {
    toggleSort: function () {
      this.$refs.exampleBasic.clearSort()
    }
  }
}
</script>

<style scoped>
</style>
