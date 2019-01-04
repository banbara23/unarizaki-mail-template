<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <!-- 右ドロワー -->
    <v-navigation-drawer
      fixed
      right
      clipped
      app
    >
      <div class="container fluid pb-0">
        <p>入力</p>
        <div>
          <v-text-field
            label="担当者名"
            placeholder=""
            v-model="sender_name"
          ></v-text-field>
        </div>

        <div>
          <v-text-field
            label="名前"
            placeholder=""
          ></v-text-field>
        </div>

        <div>
          <v-menu
            :close-on-content-click="false"
            :nudge-right="40"
            lazy
            transition="scale-transition"
            offset-y
            full-width
            v-model="checkin_menu"
            min-width="290px"
          >
            <v-text-field
              slot="activator"
              v-model="checkin_date"
              label="チェックイン日"
              prepend-icon="event"
              readonly
            ></v-text-field>
            <v-date-picker
              :day-format="date => new Date(date).getDate()"
              v-model="checkin_date"
              @input="checkin_menu = false"
              locale="jp"
            ></v-date-picker>
          </v-menu>
        </div>

        <div>
          <v-menu
            :close-on-content-click="false"
            :nudge-right="40"
            lazy
            transition="scale-transition"
            offset-y
            full-width
            v-model="checkout_menu"
            min-width="290px"
          >
            <v-text-field
              slot="activator"
              v-model="checkout_date"
              label="チェックアウト"
              prepend-icon="event"
              readonly
            ></v-text-field>
            <v-date-picker
              v-model="checkout_date"
              @input="checkout_menu = false"
              :day-format="date => new Date(date).getDate()"
              locale="jp"
            ></v-date-picker>
          </v-menu>
        </div>

        <div>
          <v-select
            :items="items"
            box
            label="ご宿泊先"
          ></v-select>
        </div>

        <div>
          <v-select
            :items="items"
            box
            label="お部屋タイプ"
          ></v-select>
        </div>

      </div>

    </v-navigation-drawer>
    <!-- 左ナビゲーション -->
    <v-navigation-drawer
      fixed
      app
      clipped
    >
      <v-subheader>テンプレート</v-subheader>
      <v-list :expand=true>
        <v-list-group
          v-for="item in items"
          v-model="item.active"
          :key="item.title"
          no-action
        >
          <v-list-tile slot="activator">
            <v-list-tile-content>
              <v-list-tile-title>{{ item.title }}</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>

          <v-list-tile
            v-for="subItem in item.items"
            :key="subItem.title"
            @click="selected_code = subItem.code, title=createTtile(item.title, subItem.title)"
          >
            <v-list-tile-content>
              <v-list-tile-title>{{ subItem.title }}</v-list-tile-title>
            </v-list-tile-content>

            <v-list-tile-action>
              <v-icon>{{ subItem.action }}</v-icon>
            </v-list-tile-action>
          </v-list-tile>
        </v-list-group>
      </v-list>
    </v-navigation-drawer>

    <pm2-other-hotel
      v-if="selected_code == '2pm_other_hotel'"
      :title="title"
      :text="mail_temp"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
    ></pm2-other-hotel>

    <pm2-other-near-hotel
      v-if="selected_code == '2pm_other_near_hotel'"
      :title="title"
      :text="mail_temp"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
    >
    </pm2-other-near-hotel>

    <pm2-my-hotel
      v-if="selected_code == '2pm_my_hotel'"
      :title="title"
      :text="mail_temp"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
    ></pm2-my-hotel>

  </v-layout>
</template>

<script>
import MailTemplate from '~/components/MailTemplate.vue'
import Pm2OtherHotel from '~/components/2pm/2pmOtherHotel.vue'
import Pm2OtherNearHotel from '~/components/2pm/2pmOtherNearHotel.vue'
import Pm2MyHotel from '~/components/2pm/2pmMyHotel.vue'

export default {
  components: {
    MailTemplate,
    Pm2OtherHotel,
    Pm2OtherNearHotel,
    Pm2MyHotel
  },
  data: () => ({
    sender_name: '',
    selected_code: '',
    mail_temp: '',
    title: '',
    valid: false,
    checkin_menu: false,
    checkout_menu: false,
    checkin_date: new Date().toISOString().substr(0, 10),
    checkout_date: new Date().toISOString().substr(0, 10),
    items: [
      {
        title: '2本目〜',
        active: true,
        items: [
          { title: 'PM～他館', code: '2pm_other_hotel' },
          { title: 'PM～宿泊有', code: '2pm_my_hotel' },
          { title: 'PM～徒歩圏内宿', code: '2pm_other_near_hotel' }
        ]
      },
      {
        title: '3本目〜',
        active: true,
        items: [
          { title: 'PM～他館', code: '3pm_other_hotel' },
          { title: 'PM～宿泊有', code: '3pm_my_hotel' },
          { title: 'PM～徒歩圏内宿', code: '3pm_other_near_hotel' }
        ]
      },
      {
        title: 'Education',
        active: true,
        items: [{ title: 'List Item' }]
      },
      {
        title: 'Family',
        active: true,
        items: [{ title: 'List Item' }]
      },
      {
        title: 'Health',
        active: true,
        items: [{ title: 'List Item' }]
      },
      {
        title: 'Office',
        active: true,
        items: [{ title: 'List Item' }]
      },
      {
        title: 'Promotions',
        active: true,
        items: [{ title: 'List Item' }]
      }
    ]
  }),
  // watch: {
  //   sender_name: function(newName, oldName) {
  //     if (!newName) return
  //     if (oldName == '') {
  //       oldName = 'sender_name'
  //     }
  //     this.mail_temp = this.mail_temp.replace(oldName, newName)
  //   },
  //   checkin_date: function(oldVal, newVal) {
  //     this.mail_temp = this.mail_temp.replace(oldVal, newVal)
  //   },
  //   checkout_date: function(oldVal, newVal) {
  //     this.mail_temp = this.mail_temp.replace(oldVal, newVal)
  //   }
  // },
  methods: {
    createTtile: function(title, subTitle) {
      return `${title} / ${subTitle}`
    }
  }
}
</script>
