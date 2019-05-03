<template>
  <div class="footer">
    <span>{{ first }}-{{ last }} of {{ tableLength }} items</span>
    <el-pagination
      layout="sizes, prev, pager, next, jumper"
      :current-page.sync="currentPage"
      :page-sizes="pageSizes"
      :page-size="pageSize"
      :total="tableLength"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </div>
</template>

<script>

export default {
  name: 'PaginationFooter',
  props: {
    tableLength: {
      type: Number,
      required: true
    },
    page: {
      type: Object,
      required: true
    },
    showDefaultTable: {
      type: Boolean,
      required: true
    },
    pageSizes: {
      type: Array,
      default: () => [20, 50, 100]
    }
  },
  mounted () {
    this.getOffSet()
  },
  data () {
    return {
      pageSize: this.page.limit,
      currentPage: (this.page.limit + this.page.offset) / this.page.limit,
      first: 1,
      last: 20,
      limit: this.page.callLimit > this.tableLength ? this.page.callLimit : this.tableLength
    }
  },
  watch: {
    tableLength: function (length) {
      this.handleCurrentChange(1)
    }
  },
  methods: {
    handleSizeChange (val) {
      this.pageSize = val
      this.getOffSet()
    },
    handleCurrentChange (val) {
      this.currentPage = val
      this.getOffSet()
    },
    getOffSet () {
      const offset = this.pageSize * (this.currentPage - 1)
      const pageCalc = this.pageSize * this.currentPage
      let page = {
        limit: this.pageSize,
        currentPage: this.currentPage,
        offset: offset,
        callNext: (this.currentPage + 1) > this.tableLength
      }

      if (!this.showDefaultTable) {
        page.callNext = (offset + 1) > this.limit
        if (page.callNext) {
          this.limit += this.page.callLimit
        }
      }

      this.first = offset + 1
      this.last = pageCalc < this.tableLength ? pageCalc : this.tableLength
      this.$emit('page-change', page)
    }
  }
}
</script>

<style scoped>
.footer {
  height: 60px;
  background-color: #fafafa;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 16px !important;
}

.footer >>> .el-pagination button {
  background-color: transparent!important;
}

.footer >>> .el-pager li {
  background-color: transparent!important;
}
</style>
