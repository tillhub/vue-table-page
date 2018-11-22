<template>
  <el-table
    ref="defaultTable"
    style="width: 100%"
    :data="tablePage"
    @sort-change="sortChange">
    <el-table-column
      v-for="(header, i) in headers"
      :key="i"
      :label="header.label"
      :prop="header.value"
      :sortable="getSortableType(header.sortable)"
      :min-width="header.minWidth || ''"
      :width="header.width || ''" >
      <template slot-scope="scope">
        {{ formatCell(header, scope.row) }}
      </template>
  </el-table-column></el-table>
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
    },
    usingTable: {
      type: Boolean,
      required: true
    },
    locale: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      tablePage: this.table,
      tableData: this.table
    }
  },
  mounted () {
    this.pageChange(this.page)
  },
  watch: {
    table: function (table) {
      this.tableChange(table)
    },
    page: function (pageInfo, old) {
      if (pageInfo.offset === old.offset && pageInfo.limit === old.limit) return
      this.pageChange(pageInfo)
    }
  },
  methods: {
    tableChange (table) {
      if (this.usingTable) {
        this.$refs.defaultTable.clearSort()
      }
      if (this.hidePagination) {
        this.tablePage = table
      } else {
        this.tableData = table
        this.pageChange(this.page)
      }
    },
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
        return this.hidePagination ? true : 'custom'
      }
      return false
    },
    formatCell (header, row) {
      if (header.format) {
        return header.format(row[header.value])
      } else if (header.isDate) {
        return new Date(row[header.value]).toLocaleDateString()
      } else {
        return row[header.value]
      }
    }

  }
}
</script>

<style scoped>
</style>
