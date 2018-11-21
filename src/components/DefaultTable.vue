<template>
  <el-table
    :data="tablePage"
    style="width: 100%"
    @sort-change="sortChange">
    <el-table-column
      v-for="(header, i) in headers"
      :key="i"
      :label="header.label"
      :prop="header.value"
      :sortable="getSortableType(header.sortable)"
      :min-width="header.minWidth || ''"
      :width="header.width || ''" />
  </el-table>
</template>
<script>

export default {
  name: 'Table',
  props: {
    headers: {
      type: Array,
      validator: function (array) {
        return array.every(item => {
          return item.label && item.value
        })
      },
      required: true
    },
    table: {
      type: Array,
      required: true
    },
    page: {
      type: Object,
      required: true
    },
    hidePagination: {
      type: Boolean,
      required: true
    }
  },
  data () {
    return {
      tablePage: this.table,
      tableData: this.table
    }
  },
  watch: {
    table: function (table) {
      this.tableData = table
    },
    page: function (pageInfo) {
      this.pageChange(pageInfo)
    }
  },
  methods: {
    pageChange (page) {
      if (this.hidePagination) return
      const array = [...this.tableData]
      const newTable = array.splice(page.offset, page.limit)
      this.tablePage = newTable
      this.$emit('table-change', newTable)
    },
    sortChange (info) {
      this.$emit('sort-change', info)
      if (this.hidePagination) return
      const compareRow = function (a, b) {
        if (info.order === 'ascending') {
          return a[info.prop] > b[info.prop] ? 1 : -1
        } else {
          return a[info.prop] < b[info.prop] ? 1 : -1
        }
      }
      if (!info.order) {
        this.tableData = [...this.table]
      } else {
        this.tableData = [...this.table].sort(compareRow)
      }
      this.pageChange(this.page)
    },
    getSortableType (sortable) {
      if (sortable) {
        return this.hidePagination ? true : 'customer'
      }
      return false
    }
  }
}
</script>

<style scoped>
</style>
