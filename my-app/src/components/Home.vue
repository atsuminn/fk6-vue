<template>
  <v-container fluid>
    <v-layout row wrap>
      <form-dialog />
      <tab-item />

  </v-layout>
  </v-container>
</template>

<script>
import TabItem from './TabItem.vue'
import FormDialog from './FormDialog.vue'
export default {
  components: { FormDialog, TabItem },
  data () {
    return {
      dialog: false
    }
  }
}
</script>

