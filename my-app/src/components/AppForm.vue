<template>
  <div>
    <v-toolbar flat color="white">
      <v-toolbar-title>就職活動申請・報告</v-toolbar-title>
      <v-divider
        class="mx-2"
        inset
        verticala
      ></v-divider>
      <v-spacer></v-spacer>
      <v-dialog v-model="dialog" max-width="500px">
        <v-btn slot="activator" color="primary" dark class="mb-2">就職活動申請・報告書作成</v-btn>
        <v-card>
          <v-card-title>
            <span class="headline">{{ formTitle }}</span>
          </v-card-title>
          <v-card-text>
            <v-container grid-list-md>
              <v-layout wrap>
                <v-flex xs12 sm6 md12>
                  <v-text-field v-model="editedItem.Number" label="学籍番号"></v-text-field>
                </v-flex>
      <!--カレンダー開始日-->
       <v-flex xs12 lg6>
        <v-menu
          ref="menu1"
          :close-on-content-click="false"
          v-model="menu1"
          :nudge-right="40"
          lazy
          transition="scale-transition"
          offset-y
          full-width
          max-width="290px"
          min-width="290px"
        >
          <v-text-field
            slot="activator"
            v-model="editedItem.date1"
            label="開始日"
            prepend-icon="event"
          ></v-text-field>
          <v-date-picker v-model="editedItem.date1" no-title @input="menu1 = false"></v-date-picker>
        </v-menu>
      </v-flex>
     <!--開始時間-->
      <v-flex xs11 lg6>
      <v-menu
        ref="menu2"
        :close-on-content-click="false"
        v-model="menu2"
        :nudge-right="40"
        :return-value.sync="time"
        lazy
        transition="scale-transition"
        offset-y
        full-width
        max-width="290px"
        min-width="290px"
      >
        <v-text-field
          slot="activator"
          v-model="time"
          label="開始時間"
          prepend-icon="access_time"
          readonly
        ></v-text-field>
        <v-time-picker
          v-if="menu2"
          v-model="time"
          full-width
          @change="$refs.menu.save(time)"
        ></v-time-picker>
      </v-menu>
    </v-flex>
<!--カレンダー終了日-->
      <v-flex xs12 lg6>
        <v-menu
          ref="menu3"
          :close-on-content-click="false"
          v-model="menu3"
          :nudge-right="40"
          lazy
          transition="scale-transition"
          offset-y
          full-width
          max-width="290px"
          min-width="290px"
        >
          <v-text-field
            slot="activator"
            v-model="editedItem.date2"
            label="終了日"
            prepend-icon="event"
          ></v-text-field>
          <v-date-picker v-model="editedItem.date2" no-title @input="menu3 = false"></v-date-picker>
        </v-menu>
      </v-flex>
<!--終了時間-->
      <v-flex xs11 lg6>
      <v-menu
        ref="menu4"
        :close-on-content-click="false"
        v-model="menu4"
        :nudge-right="40"
        :return-value.sync="time"
        lazy
        transition="scale-transition"
        offset-y
        full-width
        max-width="290px"
        min-width="290px"
      >
        <v-text-field
          slot="activator"
          v-model="time"
          label="終了時間"
          prepend-icon="access_time"
          readonly
        ></v-text-field>
        <v-time-picker
          v-if="menu4"
          v-model="time"
          full-width
          @change="$refs.menu.save(time)"
        ></v-time-picker>
      </v-menu>
    </v-flex>
        <v-flex xs12 sm6 md12>
          <v-text-field v-model="editedItem.place" label="場所"></v-text-field>
        </v-flex><br>
        <v-flex xs12 sm6 md12>
          <v-text-field v-model="editedItem.content" label="内容"></v-text-field>
        </v-flex>
        <v-flex xs12 sm6 md12>
          <v-text-field v-model="editedItem.company" label="会社名"></v-text-field>
        </v-flex>
        <v-flex xs12 sm6 md12>
          <Calendar />
          <v-text-field v-model="editedItem.status" label="欠席・早退・遅刻"></v-text-field>
        </v-flex>
          </v-layout>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" flat @click.native="close">キャンセル</v-btn>
        <v-btn color="blue darken-1" flat @click.native="save">保存</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</v-toolbar>
    
     <v-card>
    <v-card-title>
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="search"
        label="Search"
        single-line
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="desserts"
      
      :search="search"
    >
      <template slot="items" slot-scope="props">
        <td>{{ props.item.Number }}</td>
        <td>{{ props.item.date1 }}</td>
        <td>{{ props.item.date2 }}</td>
        <td>{{ props.item.place }}</td>
        <td>{{ props.item.content }}</td>
        <td>{{ props.item.company }}</td>
        <td>{{ props.item.status }}</td>
          <v-icon
            small
            class="mr-2"
            @click="editItem(props.item)"
          >
          edit
          </v-icon>
           <v-icon
            small
            @click="deleteItem(props.item)"
          >
          delete
          </v-icon>
      </template>
      <template slot="pageText" slot-scope="props">
      page {{ props.pageStart }} - {{ props.pageStop }} ページ数{{ props.itemsLength }}
    </template>
      <template slot="no-data">
        <v-btn color="primary" @click="initialize">Reset</v-btn>
      </template>
      <v-alert slot="no-results" :value="true" color="error" icon="warning">
        Your search for "{{ search }}" found no results.
      </v-alert>
    </v-data-table>
  </v-card>
  </div>
</template>
<script>
export default {
  data: () => ({
    return: {
      time: null,
      menu4: false
    },
    date1: new Date().toISOString().substr(0, 10),
    date2: new Date().toISOString().substr(0, 10),
    menu1: false,
    menu2: false,
    menu3: false,
    dialog: false,
    search: '',
    headers: [
      { text: '学生番号', value: 'Number' },
      { text: '開始日時', value: 'date1' },
      { text: '終了日時', value: 'date2' },
      { text: '場所', value: 'place' },
      { text: '内容', value: 'content' },
      { text: '会社', value: 'company' },
      { text: '遅刻・早退・欠席', value: 'status' },
      { text: '編集・削除' }
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      Number: '',
      date1: '',
      date2: '',
      place: '',
      content: '',
      company: '',
      status: ''
    },
    defaultItem: {
      Number: '',
      date1: '',
      date2: '',
      place: '',
      content: '',
      company: '',
      status: ''
    }
  }),
  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    }
  },
  watch: {
    dialog (val) {
      val || this.close()
    },
    date1 (val) {
      this.date1 = this.date1
    },
    date2 (val) {
      this.date2 = this.date2
    }
  },
  created () {
    this.initialize()
  },
  methods: {
    initialize () {
      this.desserts = [
        {
          Number: '114514',
          StartDate: '2018/04/33',
          EndDate: '2018/04/99',
          place: '北海道情報専門学校４F',
          content: '学校見学',
          company: '電子開発学園',
          status: ''
        },
        {
          Number: '114514',
          StartDate: '2018/04/33',
          EndDate: '2018/04/99',
          place: '北海道情報専門学校４F',
          content: '学校見学',
          company: '電子開発学園',
          status: ''
        },
        {
          date: '201',
          place: '',
          content: 'ueo',
          company: 'aooaoa',
          status: 'de'
        },
        {
          date: '233',
          place: 'msi',
          content: 'eo',
          company: 'aoaoa',
          status: 'ne'
        }
      ]
    },
    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },
    deleteItem (item) {
      const index = this.desserts.indexOf(item)
      confirm('削除しても大丈夫ですか？') && this.desserts.splice(index, 1)
    },
    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },
    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    }
  }
}
</script>