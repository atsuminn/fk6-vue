<template>
  <div>
    <v-toolbar color="cyan" dark tabs>

      <v-toolbar-title>Menu</v-toolbar-title>
      <v-tabs
        slot="extension"
        v-model="tab"
        color="cyan"
        align-with-title
      >
        <v-tabs-slider color="yellow"></v-tabs-slider>

        <v-tab v-for="item in items" :key="item">
          {{ item }}
        </v-tab>
      </v-tabs>
    </v-toolbar>

    <v-tabs-items v-model="tab">
      <v-tab-item v-for="item in contents" :key="item">
        <v-card flat>
          <form-dialog />
          <v-card-text>{{ item }}</v-card-text>
        </v-card>
      </v-tab-item>
    </v-tabs-items>
  </div>
</template>

<script>
import FormDialog from './FormDialog.vue'
export default {
  components: {FormDialog},
  data () {
    return {
      tab: null,
      items: [
        'Task', 'Create Form'
      ],
      contents: [
        'aaaaa', 'bbbbb'
      ]
    }
  }
}
</script>