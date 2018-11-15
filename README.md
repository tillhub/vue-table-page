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
import VueTablePage from '@tillhub/vue-table-page'
import '@tillhub/vue-table-page/vue-table-page.css'

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
### Slots
There are three slots at the moment. The "header-left",  "header-right", and the "page-table". 

| Slot         | required | location                                                                                           |
|:------------ |:---------|:-------------------------------------------------------------------------------------------------- |
| header-left  | no       | Top of page. Below the Title and Message box if present. All content will be aligned on the left.  |
| header-right | no       | Top of page. Below the Title and Message box if present. All content will be aligned on the right. |
| page-table   | no       | Middle of the page below the header and above the pagination footer.                               |

### Properties
The components accepts optional properties.

| Property        | type    | required | example        | default | description                                                                                   |
|:--------------- |:--------|:---------|:-------------- |:------- |:--------------------------------------------------------------------------------------------- |
| locale          | string  | no       | "de" or "en"   | "en"    | Currently only German and English is supported. Only 'de' and 'en '                           |
| table-length    | number  | yes      | 100            | n/a     | Need to pass in how many items in table to calculate the pagination                           |
| init-page-size  | number  | no       | 20, 50, or 100 | 20      | Will default page limit to this input on load. note the only options are 20, 50 and 100       |
| init-page       | number  | no       | 20, 50, or 100 | 20      | Will default page number to this input on load.                                               |
| title           | string  | no       | "Products"     | ""      | The title will appear in the top left of header in h2                                         |
| fixed-header    | boolean | no       | true           | false   | Fixes the header at the top when scrolling                                                    |
| message         | string  | no       | "Info Here"    | ""      | If provided a pop up box with info button will appear with message below the title if present |
| show-message    | boolean | no       | true           | false   | on load will show or hide the message box                                                     |
| hide-info-btn   | boolean | no       | true           | false   | Info button will not appear, and the message box will be present on load                      |

### Event

The Pagination will retrun and object with the offSet and size (page limit). This should allow the use to calculate what data is in view of the table

| Event        | type    | example       | default | description                                                                            |
|:------------ |:------- |:------------- |:--------|:-------------------------------------------------------------------------------------- |
| @page-change | fuction | (pageObj)=>{} | n/a     | Passes the offset and size in an object on pagination change { offSet: 20, size: 100}  |

## License

MIT © [jmy-kellogg](https://github.com/jmy-kellogg)
