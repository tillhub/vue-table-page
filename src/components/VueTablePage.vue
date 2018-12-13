<template>
  <div
    id="vue-table-page">
    <div class="table-page-main">
      <div ref="tablePageHeader">
        <span
          class="header-title"
          v-if="title">{{ title }}</span>
        <message-box
          v-if="message"
          :hide-info-btn="hideInfoBtn"
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
          v-show="showDefaltTable"
          :using-table="showDefaltTable"
          :table="tableData"
          :headers="headers"
          :page="pageInfo"
          :page-sizes="pageSizes"
          :hide-pagination="hidePagination"
          :locale="locale"
          :table-max-height="tableMaxHeight"
          :table-height="tableHeight"
          @table-change="$emit('table-change', $event)"
          @sort-change="$emit('sort-change', $event)" />
      </div>
    </div>
    <div v-show="!hidePagination">
      <pagination-footer
        :table-length="tableLength"
        :show-defalt-table="showDefaltTable"
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

import Vue from 'vue'
import 'typeface-lato'
import 'element-ui/lib/theme-chalk/index.css'
import enLocale from 'element-ui/lib/locale/lang/en'
import deLocale from 'element-ui/lib/locale/lang/de'
import { Table, Pagination, Button, Row, Card } from 'element-ui'

Vue.use(Pagination)
Vue.use(Button)
Vue.use(Row)
Vue.use(Card)
Vue.use(Table)

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
    hideInfoBtn: {
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
    }
  },
  beforeMount () {
    if (this.locale === 'de') {
      ElementLocale.use(deLocale)
    } else {
      ElementLocale.use(enLocale)
    }
  },
  updated () {
    const height = this.$refs.tablePageHeader.clientHeight || 0
    this.height = `calc(100% - ${height}px)`
  },
  data () {
    return {
      show: this.hideInfoBtn ? true : this.showMessage,
      tableLength: this.tableSize === null ? this.tableData.length : this.tableSize,
      showDefaltTable: !this.$slots['page-table'],
      pageInfo: this.getPage(this.page),
      height: '100%',
      showInfoButton: !this.hideInfoBtn && this.message.length
    }
  },
  watch: {
    tableSize: function () {
      this.setTableLength()
    },
    tableData: function () {
      this.setTableLength()
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
  overflow: hidden;
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
