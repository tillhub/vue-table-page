<template>
  <div class="footer-content">
    <div class="flex">
      <span class="page-count">{{ first }}-{{ last }} of {{ tableLength }} items</span>
      <el-pagination
        layout="sizes, prev, pager, next, jumper"
        :current-page.sync="currentPage"
        :page-sizes="[20, 50, 100]"
        :page-size="pageSize"
        :total="tableLength"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
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
    showDefaltTable: {
      type: Boolean,
      required: true
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
        offset: offset,
        callNext: (this.currentPage + 1) > this.tableLength
      }

      if (!this.showDefaltTable) {
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

<style>
.footer-content {
  width: 100%;
  display: flex;
  margin: 25px 0px;
  justify-content: center;
}

.page-count {
  margin: 5px;
  font-size: 20px;
}

.el-pagination > ul > li,
.el-pagination .btn-next,
.el-pagination .btn-prev {
  background-color: #fafafa !important;
  font-size: 20px !important;
}

.el-pagination .btn-next .el-icon,
.el-pagination .btn-prev .el-icon {
  font-size: 20px !important;
}
</style>
