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
    </div>
    <div class="page-footer">
      <pagination-footer
        :table-length="tableLength"
        :init-page-size="initPageSize"
        :init-page="initPage"
        @page-change="pageChange"/>
    </div>
  </div>
</template>

<script>
import ElementLocale from 'element-ui/lib/locale'
import MessageBox from './MessageBox.vue'
import PaginationFooter from './PaginationFooter.vue'

import Vue from 'vue'
import 'typeface-lato'
import 'element-ui/lib/theme-chalk/index.css'
import enLocale from 'element-ui/lib/locale/lang/en'
import deLocale from 'element-ui/lib/locale/lang/de'
import { Pagination, Button, Row, Card } from 'element-ui'

Vue.use(Pagination)
Vue.use(Button)
Vue.use(Row)
Vue.use(Card)

export default {
  name: 'VueTablePage',
  components: {
    MessageBox,
    PaginationFooter
  },
  props: {
    locale: {
      type: String,
      default: navigator.language
    },
    tableLength: {
      type: Number,
      required: true
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
    initPageSize: {
      type: Number,
      default: 20,
      validator: value => {
        return value === 20 || value === 50 || value === 100
      }
    },
    initPage: {
      type: Number,
      default: 1
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
      show: this.hideInfoBtn ? true : this.showMessage
    }
  },
  methods: {
    pageChange (page) {
      this.$emit('page-change', page)
    },
    toggleShow () {
      this.show = !this.show
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
