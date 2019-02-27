<template>
  <el-table
    ref="defaultTable"
    :height="tableHeight"
    :data="tablePage"
    :max-height="tableMaxHeight"
    @sort-change="sortChange">
    <el-table-column
      v-for="(header, i) in headers"
      :show-overflow-tooltip="showOverflowTooltip"
      :fixed="header.fixed || false"
      :key="i"
      :label="header.label"
      :prop="header.value"
      :sortable="getSortableType(header.sortable)"
      :min-width="header.minWidth || ''"
      :width="header.width || ''" >
      <template slot-scope="scope">
        {{ formatCell(header, scope.row) }}
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import safeGet from 'just-safe-get'
const currencyFormatter = require('currency-formatter')

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
    },
    tableMaxHeight: {
      type: Number | String,
      default: 'auto'
    },
    tableHeight: {
      type: Number | String,
      default: '100%'
    },
    emptyDisplay: {
      type: String,
      required: true
    },
    remoteSort: {
      type: Boolean,
      default: false
    },
    showOverflowTooltip: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      tablePage: this.table,
      tableData: this.table,
      info: {}
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
      this.info = info

      if (this.remoteSort) {
        return this.$emit('sort-change', info)
      } else {
        if (this.hidePagination) return
        if (!info.order) {
          this.tableData = [...this.table]
        } else {
          this.tableData = [...this.table].sort(this.compareRow)
        }
        this.pageChange(this.page)
      }
    },
    compareRow (a, b) {
      let A = typeof a[this.info.prop] === 'string' ? a[this.info.prop].toLowerCase() : a[this.info.prop]
      let B = typeof b[this.info.prop] === 'string' ? b[this.info.prop].toLowerCase() : b[this.info.prop]
      if (A === B) return 0
      if (A === null) return 1
      if (B === null) return -1
      if (this.info.order === 'ascending') {
        return A > B ? 1 : -1
      } else if (this.info.order === 'descending') {
        return A < B ? 1 : -1
      }
    },
    getSortableType (sortable) {
      if (sortable) {
        return this.hidePagination ? true : 'custom'
      }
      return false
    },
    formatCell (header, row) {
      const cellValue = safeGet(row, header.value)

      if (header.format) {
        return header.format(row)
      } else if (cellValue === null || cellValue === undefined) {
        return this.emptyDisplay
      } else if (header.type === 'date') {
        return new Date(cellValue).toLocaleDateString(this.locale)
      } else if (header.type === 'currency') {
        const value = parseFloat(cellValue)
        return currencyFormatter.format(value, {
          code: row.currency || null
        })
      } else {
        return cellValue
      }
    }
  }
}
</script>

<style scoped>
.el-table >>> td,
.el-table >>> th{
  padding: 10px 0px !important;
}

.el-table >>> th .cell{
  white-space: unset;
}
</style>
