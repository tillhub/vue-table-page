<template>
  <div
    id="vue-table-page">
    <div class="table-page-main">
      <div
        ref="tablePageHeader"
        :style="headerStyle">
        <span
          class="header-title"
          v-if="title">{{ title }}</span>
        <message-box
          v-if="message"
          :message="message"
          :show="show"
          @toggle-show="toggleShow"/>
        <el-row
          type="flex"
          justify="space-between">
          <div class="flex">
            <i
              class="el-icon-info"
              v-show="showInfoButton"
              @click="toggleShow"/>
            <slot name="header-left" />
          </div>
          <div class="flex">
            <slot name="header-right" />
          </div>
        </el-row>
      </div>
      <div
        class="table-page-body"
        :style="{ height }">
        <slot name="page-table" />
        <default-table
          v-show="showDefaultTable"
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
          @table-change="$emit('table-change', $event)"
          @sort-change="$emit('sort-change', $event)" />
      </div>
    </div>
    <div v-show="!hidePagination">
      <pagination-footer
        :table-length="tableLength"
        :show-default-table="showDefaultTable"
        :page="pageInfo"
        @page-change="pageChange"/>
    </div>
  </div>
</template>

<script>
import ElementLocale from 'element-ui/lib/locale'
import MessageBox from './MessageBox.vue'
import PaginationFooter from './PaginationFooter.vue'
import DefaultTable from './DefaultTable.vue'
import enLocale from 'element-ui/lib/locale/lang/en'
import deLocale from 'element-ui/lib/locale/lang/de'

export default {
  name: 'VueTablePage',
  components: {
    MessageBox,
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
    message: {
      type: String,
      default: ''
    },
    showMessage: {
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
      show: this.message.length ? this.showMessage : false,
      tableLength: this.tableSize === null ? this.tableData.length : this.tableSize,
      showDefaultTable: !this.$slots['page-table'],
      pageInfo: this.getPage(this.page),
      initialHeight: '100%',
      heightWithInfoBox: '100%',
      showInfoButton: this.message.length
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
      return `calc(100% - ${currentHeight}px)`
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

<style>
li,
div,
span {
  font-family: "Lato";
}

.flex {
  display: flex;
}

#vue-table-page {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
}

.table-page-main {
  height: 100%;
  overflow: auto;
}

.el-icon-info {
  color: #269ff6;
  font-size: x-large;
  cursor: pointer;
  margin: 10px;

}

.el-icon-info:hover {
  color: #1b7abe;
}

.table-page-body {
  overflow: auto
}

.header-title {
    display: block;
    font-size: 2em;
    font-weight: bold;
    padding: 20px;
}
</style>
