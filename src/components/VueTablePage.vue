<template>
  <div
    id="vue-table-page">
    <div
      ref="tablePageHeader"
      class="header"
      :style="headerStyle">
      <span
        class="header-title"
        v-if="title">{{ title }}</span>
      <el-row
        type="flex"
        justify="space-between"
        align="middle"
      >
        <el-row
          type="flex"
          align="middle"
        >
          <slot name="message" />
          <slot name="header-left" />
        </el-row>
        <div class="flex">
          <slot name="header-right" />
        </div>
      </el-row>
    </div>
    <el-card
      class="content"
      :body-style="{ height: '100%', padding: '0px' }"
      v-show="!showNoData"
    >
      <slot name="page-table" />
      <default-table
        v-show="showDefaultTable"
        :show-overflow-tooltip="showOverflowTooltip"
        :using-table="showDefaultTable"
        :table="tableData"
        :headers="headers"
        :page="pageInfo"
        :page-sizes="pageSizes"
        :hide-pagination="hidePagination"
        :locale="locale"
        :table-max-height="tableMaxHeight"
        :table-height="tableHeight"
        :empty-display="emptyDisplay"
        :remote-sort="remoteSort"
        :default-sort="defaultSort"
        @table-change="$emit('table-change', $event)"
        @sort-change="$emit('sort-change', $event)"
        :show-summary="showSummary"
        :summary-method="summaryMethod"/>
    </el-card>
    <el-card
      class="content"
      :body-style="{ height: '100%', padding: '0px' }"
      v-show="showNoData"
    >
      <slot name="no-data">
        <div class="no-data-default">
          <div>No data</div>
        </div>
      </slot>
    </el-card>
    <pagination-footer
      v-show="!hidePagination"
      :table-length="tableLength"
      :show-default-table="showDefaultTable"
      :page="pageInfo"
      @page-change="pageChange"
    />
  </div>
</template>

<script>
import ElementLocale from 'element-ui/lib/locale'
import PaginationFooter from './PaginationFooter.vue'
import DefaultTable from './DefaultTable.vue'
import enLocale from 'element-ui/lib/locale/lang/en'
import deLocale from 'element-ui/lib/locale/lang/de'

export default {
  name: 'VueTablePage',
  components: {
    PaginationFooter,
    DefaultTable
  },
  props: {
    locale: {
      type: String,
      default: navigator.language
    },
    tableSize: {
      type: Number | null,
      default: null
    },
    title: {
      type: String,
      default: ''
    },
    fixedHeader: {
      type: Boolean,
      default: false
    },
    hidePagination: {
      type: Boolean,
      default: false
    },
    tableData: {
      type: Array,
      default: () => []
    },
    headers: {
      type: Array,
      default: () => []
    },
    page: {
      type: Object,
      default: () => {
        return {
          offset: 0,
          limit: 20,
          callLimit: 1000
        }
      },
      validator: (page) => {
        return page.limit === 20 || page.limit === 50 || page.limit === 100
      }
    },
    pageSizes: {
      type: Array,
      default: () => [20, 50, 100]
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
      default: '--'
    },
    remoteSort: {
      type: Boolean,
      default: false
    },
    headerStyle: {
      type: Object,
      default: () => {}
    },
    showOverflowTooltip: {
      type: Boolean,
      default: false
    },
    defaultSort: {
      type: Object,
      validator: (obj) => {
        return (obj.prop || obj.prop === null) && (obj.order || obj.order === null)
      },
      default: () => {}
    },
    showSummary: {
      type: Boolean,
      default: false
    },
    summaryMethod: {
      type: Function,
      default: undefined
    }
  },
  beforeMount () {
    this.setLocale(this.locale)
  },
  mounted () {
    this.initialHeight = this.$refs.tablePageHeader.clientHeight || 0
  },
  updated () {
    if (this.show) {
      this.heightWithInfoBox = this.$refs.tablePageHeader.clientHeight || 0
    }
  },
  data () {
    return {
      tableLength: this.tableSize === null ? this.tableData.length : this.tableSize,
      showDefaultTable: !this.$slots['page-table'],
      pageInfo: this.getPage(this.page),
      initialHeight: '100%',
      heightWithInfoBox: '100%'
    }
  },
  watch: {
    tableSize: function () {
      this.setTableLength()
    },
    tableData: function () {
      this.setTableLength()
    },
    locale: function (val) {
      this.setLocale(val)
    }
  },
  computed: {
    height () {
      const currentHeight = this.show ? this.heightWithInfoBox : this.initialHeight
      const marginCalc = this.bodyCard ? 20 : 0
      this.$emit('height-change', `calc(100% - ${currentHeight}px`)
      return `calc(100% - ${currentHeight}px - ${marginCalc}px)`
    },
    showNoData () {
      return !this.tableData.length
    }
  },
  methods: {
    pageChange (page) {
      this.pageInfo = page
      this.$emit('page-change', page)
    },
    toggleShow () {
      this.show = !this.show
    },
    setTableLength () {
      this.tableLength = this.tableSize === null ? this.tableData.length : this.tableSize
    },
    getPage (page) {
      return {
        offset: parseInt(page.offset) || 0,
        limit: parseInt(page.limit) || 20,
        callLimit: parseInt(page.callLimit) || 1000
      }
    },
    setLocale (val) {
      const lang = val ? val.substring(0, 2) : 'en'
      if (lang === 'de') {
        ElementLocale.use(deLocale)
      } else {
        ElementLocale.use(enLocale)
      }
    }
  }
}
</script>

<style scoped>
li,
div,
span {
  font-family: "Lato";
}

.flex {
  display: flex;
}

.header {
  margin: 25px 25px 0;
  display: flex;
  flex-direction: column;
}

.content {
  margin: 25px;
  flex-grow: 1;
}

.footer {
  height: 60px;
  background-color: #fafafa;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 16px !important;
}

#vue-table-page {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
}

.el-icon-info {
  color: #269ff6;
  font-size: larger;
  cursor: pointer;
  margin-right: 10px;
}

.el-icon-info:hover {
  color: #1b7abe;
}

.header-title {
  display: block;
  font-size: 2em;
  font-weight: bold;
  padding: 20px;
}

.el-table__body-wrapper{
  overflow: auto;
}

.no-data-default {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: larger;
}
</style>
