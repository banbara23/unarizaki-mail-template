<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <!-- 左ドロワー -->
    <v-navigation-drawer
      fixed
      app
      permanent
    >
      <v-subheader>うなりざきメールテンプレ</v-subheader>
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

    <!-- 右ドロワー -->
    <v-navigation-drawer
      fixed
      right
      clipped
      app
      permanent
    >
      <div class="container fluid pb-0">
        <p>値を入力</p>

        <div>
          <v-text-field
            label="名前"
            v-model="receiver_name"
          ></v-text-field>
        </div>

        <div>
          <v-text-field
            label="担当者名"
            placeholder=""
            v-model="sender_name"
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
            v-model="hotelType"
            :items="['イルマーレ', 'ヴィラ']"
            box
            label="ご宿泊先"
          ></v-select>
        </div>

        <div>
          <v-select
            v-model="roomType"
            :items="['禁煙', '喫煙']"
            box
            label="お部屋タイプ"
          ></v-select>
        </div>

      </div>

    </v-navigation-drawer>

    <pm2-other-hotel
      v-if="selected_code == '2pm_other_hotel'"
      :title="title"
      :receiver_name="receiver_name"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
      :checkout_date="checkout_date"
    ></pm2-other-hotel>

    <pm2-other-near-hotel
      v-if="selected_code == '2pm_other_near_hotel'"
      :title="title"
      :receiver_name="receiver_name"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
      :checkout_date="checkout_date"
    ></pm2-other-near-hotel>

    <pm2-my-hotel
      v-if="selected_code == '2pm_my_hotel'"
      :title="title"
      :receiver_name="receiver_name"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
      :hotelType="hotelType"
      :roomType="roomType"
    ></pm2-my-hotel>

    <dive3-my-hotel-winter
      v-if="selected_code == '3dive_my_hotel_winter'"
      :title="title"
      :receiver_name="receiver_name"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
      :checkout_date="checkout_date"
      :hotelType="hotelType"
      :roomType="roomType"
    ></dive3-my-hotel-winter>

    <dive3-my-hotel-summer
      v-if="selected_code == '3dive_my_hotel_summer'"
      :title="title"
      :receiver_name="receiver_name"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
      :checkout_date="checkout_date"
      :hotelType="hotelType"
      :roomType="roomType"
    ></dive3-my-hotel-summer>

    <dive3-my-hotel-summer-no-schedule
      v-if="selected_code == '3dive_my_hotel_summer_no_schedule'"
      :title="title"
      :receiver_name="receiver_name"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
      :checkout_date="checkout_date"
      :hotelType="hotelType"
      :roomType="roomType"
    ></dive3-my-hotel-summer-no-schedule>

    <dive3-other-hotel
      v-if="selected_code == '3dive_other_hotel'"
      :title="title"
      :receiver_name="receiver_name"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
      :checkout_date="checkout_date"
    ></dive3-other-hotel>

    <dive3-other-near-hotel
      v-if="selected_code == '3dive_other_near_hotel'"
      :title="title"
      :receiver_name="receiver_name"
      :sender_name="sender_name"
      :checkin_date="checkin_date"
      :checkout_date="checkout_date"
    ></dive3-other-near-hotel>

  </v-layout>
</template>

<script>
//左メニューは外部JSから読み込む
import leftMenuValue from './leftMenuValue.js'

// 2PM
import Pm2OtherHotel from '~/components/2pm/2pmOtherHotel.vue'
import Pm2OtherNearHotel from '~/components/2pm/2pmOtherNearHotel.vue'
import Pm2MyHotel from '~/components/2pm/2pmMyHotel.vue'
// 3DIVE
import Dive3MyHotelWinter from '~/components/dive3/Dive3MyHotelWinter.vue'
import Dive3MyHotelSummer from '~/components/dive3/Dive3MyHotelSummer.vue'
import Dive3MyHotelSummerNoSchedule from '~/components/dive3/Dive3MyHotelSummerNoSchedule.vue'
import Dive3OtherHotel from '~/components/dive3/Dive3OtherHotel.vue'
import Dive3OtherNearHotel from '~/components/dive3/Dive3OtherNearHotel.vue'

export default {
  components: {
    Pm2OtherHotel,
    Pm2OtherNearHotel,
    Pm2MyHotel,

    Dive3OtherHotel,
    Dive3OtherNearHotel,
    Dive3MyHotelSummer,
    Dive3MyHotelSummerNoSchedule,
    Dive3MyHotelWinter
  },
  data: () => ({
    selected_code: '',
    receiver_name: '',
    sender_name: '',
    mail_temp: '',
    title: '',
    valid: false,
    checkin_menu: false,
    checkout_menu: false,
    checkin_date: new Date().toISOString().substr(0, 10),
    checkout_date: new Date().toISOString().substr(0, 10),
    items: leftMenuValue,
    hotelType: '',
    roomType: ''
  }),
  methods: {
    createTtile: function(title, subTitle) {
      return `${title} / ${subTitle}`
    }
  }
}
</script>
