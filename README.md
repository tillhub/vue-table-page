# @tillhub/vue-table-page-pagination [![CircleCI](https://circleci.com/gh/tillhub/vue-table-page-pagination/tree/master.svg?style=svg)](https://circleci.com/gh/tillhub/vue-table-page-pagination/tree/master)
> Vue Table with Pagination for table range selection

## Install

```bash
npm install --save @tillhub/vue-table-page
```

## Demo

```bash

```

## Usage

```html
<template>
  <vue-table-page
    title="Example Page"
    :message="message"
    :table-length="tableLength"
    @page-change="changeTablesPage">
    <header-example slot="page-header"/>
    <table-example
      slot="page-table"
      :table-data="tableData" />
  </vue-table-page>
</template>

<script>
import Vue from 'vue'
import 'element-ui/lib/theme-chalk/index.css'
import ElementUI from 'element-ui'

Vue.use(ElementUI)

import VueTablePage from '@tillhub/vue-table-page'
import '@tillhub/vue-table-page/dist/vue-table-page.css'

export default {
  name: 'App',
  components: {
    VueTablePage
  },
  methods: {
    changeTablesPage(pages){
      console.log(pages)
    }
  },
}
</script>
```
### CSS
import '@tillhub/vue-table-page/dist/vue-table-page.css'

### Slots
There are three slots at the moment. The "header-left",  "header-right", and the "page-table". 

| Slot         | required | location                                                                                           |
|:------------ |:---------|:-------------------------------------------------------------------------------------------------- |
| header-left  | no       | Top of page. Below the Title and Message box if present. All content will be aligned on the left.  |
| header-right | no       | Top of page. Below the Title and Message box if present. All content will be aligned on the right. |
| page-table   | no       | Middle of the page below the header and above the pagination footer.                               |

### Properties

#### The Properties for the component.

| Property        | type    | required | example            | default | description                                                                                         |
|:--------------- |:--------|:---------|:------------------ |:------- |:--------------------------------------------------------------------------------------------------- |
| locale          | string  | no       | "de" or "en"       | "en"    | Currently only German and English is supported. Only 'de' and 'en '                                 |
| table-size      | number  | no       | 100                | null    | Need the total size of table to calculate the pagination if not usting table-data                   |
| table-data      | Array   | no       | [{key: value}]     | []      | Must provide if you wish to use the @table-chagne event. Needs to be an Array of objects            |
| hide-pagination | boolean | no       | true               | false   | Will hide pagination footer                                                                         |
| page            | object  | no       | { offset: 20,<br/> limit: 50,<br/> callLimit: 500}    | { offset: 0,<br/> limit: 20,<br/> callLimit: 1000}   | The offset and limit to provide if need to specify location of pagination.  The callLimit provides when the next set of data should be called     |
| page-Sizes      | Array   | no       | [100, 200]         | [20, 50, 100] | Sets the page size option in footer                                                           |
| call-limit      | number  | no       | 500                | 1000          | Will hide pagination footer                                                                   |
| header-style    | css Obj | no       | { padding: '10px'} | {}            | Will update style for header                                                                  |
| body-card       | bollean | no       | true               | false         | Will apply a card style around body                                                           |

#### The additional properties for header.

| Property        | type    | required | example        | default | description                                                                                         |
|:--------------- |:--------|:---------|:-------------- |:------- |:--------------------------------------------------------------------------------------------------- |
| title           | string  | no       | "Products"     | ""      | The title will appear in the top left of header in h2                                               |
| message         | string  | no       | "Info Here"    | ""      | If provided a pop up box with info button will appear with message below the title if present       |
| show-message    | boolean | no       | true           | false   | on load will show or hide the message box                                                           |


#### The additional properties for using Default Table. Note the default table will be used when a 'page-table' slot is not provided 
| Property         | type          | required | example                                                                      | default   | description                                                                                                                                                                                                                      |
|------------------|---------------|----------|------------------------------------------------------------------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| table-data       | Array         | Yes      | [{key: value}]                                                               | []        | Must provide if you wish to use the default table. Needs to be an Array of objects                                                                                                                                               |
| table-max-height | Number/string | No       | 1000                                                                         | 'auto'    | You can apply a custom max-height to make the header stationary and table scrollable                                                                                                                                             |
| table-height     | Number/string | No       | 1000                                                                         | '100%'    | You can apply a custom height                                                                                                                                                                                                    |
| empty-display    | String        | No       | 'N/A'                                                                        | '--'      | Apply a custom default display when the value of cell is null                                                                                                                                                                    |
| headers          | Array         | Yes      | [{label: 'Lable',<br/> value: 'key',<br/> minWidth: 10,<br/>sortable: true}] | []        | Must provide the headers for the table if using default table. The lable and value (keys from the 'table-data' items) must be provided. Other options are providing the minWidth, or width of column, and if column is sourtable |
| remote-sort      | Boolean       | No       | true                                                                         | false     | If provided the sorting will be disabled and instead an event called 'sort-change' will be emitted.                                                                                                                              |
| show-summary     | Boolean       | No       | true                                                                         | false     | whether to display a summary row; columns sum every number in that column up and display them                                                                                                                                                                        |
| summary-method   | Function      | No       | ({ columns, data }) => { // return some array }                              | undefined | pass a method to summary-method, which returns an array, and each element of the returned array will be displayed in the columns of the summary row                                                                              |

Table headers parameters option for the 'headers' prop

| parameter         | type           | required  | example                         | description                                                                            |
|:---------------   |:-------------- |:--------- |:--------------------------------|:-------------------------------------------------------------------------------------- |
| label             | string         | yes       | "Product Name"                  | This is what will be shown in the column header                                        |
| value             | string         | yes       | "product_name"                  | This will reference the unique key in the table options                                |
| minWidth          | string         | no        | "10px"                          | The column width will be dynamic. This is the minimum width the column will be.        |
| width             | string         | no        | "10px"                          | This will set the column width. Without it the column width will be dynamic.           |
| type              | string         | no        | "date", "currency"              | This will format the value according to the local for either date or currency          |
| fixed             | string/Boolean | no        | "left", "right", "top", "bottom"| This will make the entire column fixed.                                                |
| format            | Function       | no        | item=> !item ? 'n/a' : item     | You can pass in a function that auto formats the cell content                          |

### Event

The Pagination will retrun and object with the offSet and size (page limit). This should allow the use to calculate what data is in view of the table

| Event        | type    | example       | default | description                                                                                             |
|:------------ |:------- |:------------- |:--------|:--------------------------------------------------------------------------------------                  |
| @page-change | fuction | (pageObj)=>{} | n/a     | Passes the offset and size in an object on pagination change { offset: 20, limit: 100, callNext: false}. The offset is the place the visable table begins. The limit is the number of item in visable table. the callNext is the |
| @table-change| fuction | (pageObj)=>{} | n/a     | When the full table is provided in 'table-data' prop, this event will provided the data for the visable data |
| @height-change| number | 235 | n/a     | This will emit the height the body is after calculating header changes. this allows for dynamic sizing     |

## License

MIT © [jmy-kellogg](https://github.com/jmy-kellogg)
