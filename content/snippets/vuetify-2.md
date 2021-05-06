---
title: Vuetify 2
---

# Introduction



<br />

## layout

### Breakpoints

```javascript
$vuetify.breakpoint.mdAndDown
```
<br />

## form layouts

### Dialog proxy

```vue
<template>
  <dialog-layout
    v-model="innerDialog"
    :disabled="loadingSubmit"
    @submit="submit"
    title="Dialog proxy example"
  >
    <vee-text-field
        name="Example data"
        rules="required"
        :items="options"
        chips
        small-chips
        v-model="application.example"
    />
    <v-row
      v-else
    >
      <v-col>
        <h3>Are you sure you want to revoke this application?</h3>
      </v-col>
    </v-row>
    <error-toaster
      v-model="error"
    />
  </dialog-layout>
</template>

<script>
import DialogLayout from "@/components/forms/DialogLayout";
import ErrorToaster from '@/components/ErrorToaster'

export default {
  name: 'DeclineForm',
  components: {
    DialogLayout,
    ErrorToaster
  },
  props: {
    dialog: { // outer dialog value
      type: Boolean,
      required: true
    },
  },
  data() {
    return {
      application: {
        example: ''
      },
      options: [],
      error: {},
      loadingSubmit: false,
      innerDialog: false,
    }
  },
  created() {
    this.customCreate()
  },
  methods: {
    submit: async function (isValid) {
      this.loadingSubmit = true
      try {
        if (isValid) {
          // send data and set the dialog to false
          this.innerDialog = false
          this.$emit('onSubmit')
        }
      } catch (e) {
        this.error = e
      }
      this.loadingSubmit= false
    },
    customCreate: async function () {
      try {
        // Set form data
      } catch (e) {
        this.error = e
      }
    },
    reset: function () {
      // reset form data
    },
  },
  watch: {
    innerDialog: function () {
      if (!this.innerDialog) {
        this.reset()
      }
      this.$emit('update:dialog', this.innerDialog)
    },
    dialog: function () {
      if (!this.dialog) {
        this.reset()
      } else if (this.dialog && this.isEditMode) {
        this.customCreate()
      }
      this.innerDialog = this.dialog
    },
  }
}
</script>

<style scoped>

</style>
```

<br />

## Inputs

### clearable input

```vue
 <v-text-field
         full-width
         filled
         solo
         label="Search..."
         v-model="search"
         hide-details
         :append-icon="search !== '' ? 'mdi-close' : ''"
         @click:append="search = '';"
     />
```
<br />

### Advance file input (validation of document type)

Common [Mime types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types)

```vue
<template>
    <vee-file
        name="File"
        :rules="`required|mimes:${typeSelected.value}`"
        v-model="application.file"
        :accept="typeSelected.accept"
        :suffix="typeSelected.text"
    >
      <template v-slot:append-outer>
        <v-menu
            offset-y
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
                v-bind="attrs"
                v-on="on"
            >
              <v-icon left>
                mdi-menu
              </v-icon>
              Change type
            </v-btn>
          </template>
          <v-list>
            <v-list-item-group v-model="typeSelectedIndex">
              <v-list-item
                  v-for="(type) in fileTypes"
                  text
                  :key="type.value"
                  large
              >
                <v-list-item-title>{{ type.text }}</v-list-item-title>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-menu>
      </template>
    </vee-file>
</template>

<script>
import VeeFile from "@/components/forms/VeeFile";


// COMMON MIME TYPES
// 

export default {
  name: 'FileInputExample',
  components: {
    VeeFile,
  },
  data() {
    return {
      application: {
        file: null,
      },
      typeSelectedIndex: 0,
      fileTypes: [
        {
          text: 'PDF',
          value: '.pdf',
          accept: 'application/pdf'
        },
        {
          text: 'Microsoft Word',
          value: '.doc',
          accept: 'application/msword'
        },
        {
          text: 'Microsoft Word (OpenXML)',
          value: '.docx',
          accept: 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'
        },
        {
          text: 'Microsoft Excel',
          value: '.xls',
          accept: 'application/vnd.ms-excel'
        },
        {
          text: 'Microsoft Excel (OpenXML)',
          value: '.xlsx',
          accept: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'
        },
        {
          text: 'Text',
          value: '.txt',
          accept: 'text/plain'
        }
      ],
      error: {},
      loadingSubmit: false,
    }
  },
  computed: {
    typeSelected: function () {
      return (this.typeSelectedIndex >= 0 && this.typeSelectedIndex < this.fileTypes.length)
        ? this.fileTypes[this.typeSelectedIndex]
        : {}
    },
  },
  methods: {
    submit: async function (isValid) {
      this.loadingSubmit = true
      try {
        if (isValid) {
          //
        }
      } catch (e) {
        this.error = e
      }
      this.loadingSubmit= false
    },
    reset: function () {
      this.application.file = nul
    },
  },
}
</script>

```


<br />

## Datatables

### Hidden table column
When using a column that you dont want to get displayed, use this column.
*works only on md-up*
```javascript
const headerColumn = {
  text: 'Column name text (the title)',
  value: 'column_name',
  align: ' d-none' // ' d-none' hides the column but keeps the search ability **note the leading space**
}
```

<br />

### Setting width in headers

```javascript
const headerColumn = {
  value: 'phone',
  text: 'Phone',
  width: '20%'
}
```

<br />


### align in headers 

```javascript
const headerColumn = {
  text: 'Actions',
  value: 'actions',
  align: 'right',
}
```

## Lists

### Clickable list (no link)

```vue
 <v-list>
  <v-list-item-group>
    <v-list-item
        v-for="(document) in documents"
        :key="document.item_id"
        inactive
    >
      <v-list-item-content>
        <v-list-item-title v-text="document.file_name"></v-list-item-title>
      </v-list-item-content>
      <v-list-item-icon>
        <tooltip-button
            label="Download"
            bottom
            icon
            @click="() => { fetchDocument(document.file_name) }"
        >
          <v-icon>mdi-download</v-icon>
        </tooltip-button>
      </v-list-item-icon>
    </v-list-item>
  </v-list-item-group>
</v-list>
```

<br />
