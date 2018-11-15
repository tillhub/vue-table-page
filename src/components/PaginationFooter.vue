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
    initPageSize: {
      type: Number,
      default: 20
    },
    initPage: {
      type: Number,
      default: 1
    }
  },
  mounted () {
    this.getOffSet()
  },
  data () {
    return {
      pageSize: this.initPageSize,
      currentPage: this.initPage,
      first: 1,
      last: 20
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
      const offSet = this.pageSize * (this.currentPage - 1)
      const pageCalc = this.pageSize * this.currentPage
      const page = {
        offSet: offSet,
        size: this.pageSize
      }
      this.first = offSet + 1
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
