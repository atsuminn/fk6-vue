<template>
  <v-container fluid>
    <v-layout row wrap>
      <!-- <v-flex xs12 class="text-xs-center" mt-5>
        <h1>My Page</h1>
      </v-flex>
      <v-flex xs12 class="text-xs-center" mt-3>
        <p>This is a user's home page</p>
      </v-flex>
       <v-layout row justify-center> -->
      
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

