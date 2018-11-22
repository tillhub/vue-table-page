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
        width="180" />
      <el-table-column
        sortable="custom"
        :sort-orders="['ascending', 'descending']"
        prop="name"
        label="Name"
        width="180" />
      <el-table-column
        prop="address"
        label="Address" />
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'TableExample',
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
  watch: {
    toggleSort: function () {
      this.$refs.exampleBasic.clearSort()
    }
  }
}
</script>

<style scoped>
</style>
