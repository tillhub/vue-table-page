<template>
  <div
    id="vue-table-page">
    <div class="main">
      <div
        class="header"
        :class="{ 'fixed-header': fixedHeader }">
        <h2 v-if="title">{{ title }}</h2>
        <message-box
          v-if="message"
          :hide-info-btn="hideInfoBtn"
          :message="message"
          :show="show"
          @toggle-show="toggleShow"/>
        <el-row
          type="flex"
          class="header-row"
          justify="space-between">
          <div class="flex">
            <i
              class="el-icon-info"
              v-show="!hideInfoBtn"
              @click="toggleShow"/>
            <slot name="header-left" />
          </div>
          <div class="flex">
            <slot name="header-right" />
          </div>
        </el-row>
      </div>
      <slot name="page-table" />
      <default-table
        v-show="showDefaltTable"
        :table="tableData"
        :headers="headers"
        :page="pageInfo"
        :hide-pagination="hidePagination"
        @table-change="$emit('table-change', $event)"
        @sort-change="$emit('sort-change', $event)" />
    </div>
    <div
      class="page-footer"
      v-show="!hidePagination"
    >
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
    }

  },
  beforeMount () {
    if (this.locale === 'de') {
      ElementLocale.use(deLocale)
    } else {
      ElementLocale.use(enLocale)
    }
  },
  data () {
    return {
      show: this.hideInfoBtn ? true : this.showMessage,
      tableLength: this.tableSize === null ? this.tableData.length : this.tableSize,
      showDefaltTable: !this.$slots['page-table'],
      pageInfo: this.getPage(this.page)
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
  overflow: auto;
}
.header {
  width: 100%;
}

.main {
  padding: 20px 20px 80px 20px;
}

.page-footer {
  height: 80px;
  width: 100%;
  bottom: 0;
  left: 0px;
  position: fixed;
  background-color: #fafafa;
  z-index: 1;
}

.fixed-header {
  position: sticky;
  position: -webkit-sticky;
  top: -1px;
  z-index: 1;
  background-color: white;
}

.el-icon-info {
  color: #269ff6;
  font-size: x-large;
  cursor: pointer;
  margin-top: 10px;
  margin-right: 10px;
}

.el-icon-info:hover {
  color: #1b7abe;
}

.header-row {
  margin-top: 10px;
  margin-bottom: 10px;
}
</style>
