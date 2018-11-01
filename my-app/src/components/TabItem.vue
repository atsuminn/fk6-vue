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
        <v-card flat v-if="tab === 0">
          <v-card-text>{{ tab }}</v-card-text>
          <avdulla />
        </v-card>
        <v-card flat v-if="tab === 1">
          <v-card-text>{{ item }}</v-card-text>
          <app-form />
        </v-card>
        <v-card flat v-if="tab === 2">
          <v-card-text>{{ item }}</v-card-text>
          <report-form />
        </v-card>
          
      </v-tab-item>
    </v-tabs-items>
  </div>
</template>

<script>
import ReportForm from './ReportForm.vue'
import Avdulla from './Avdulla.vue'
import AppForm from './AppForm.vue'

export default {
  components: {Avdulla, ReportForm, AppForm},
  data () {
    return {
      tab: null,
      items: [
        'All View', 'Create Form', 'Create Form'
      ],
      contents: [
        'aaaaa', 'bbbbb', 'ccccc'
      ]
    }
  }
}
</script>