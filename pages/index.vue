<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <!-- 右ドロワー -->
    <v-navigation-drawer
      v-model="drawerRight"
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
            v-model="menu2"
            :nudge-right="40"
            lazy
            transition="scale-transition"
            offset-y
            full-width
            min-width="290px"
          >
            <v-text-field
              slot="activator"
              v-model="date"
              label="チェックイン日"
              prepend-icon="event"
              hint="YYYY/MM/DD format"
              readonly
            ></v-text-field>
            <v-date-picker
              v-model="date"
              @input="menu2 = false"
              locale="jp"
            ></v-date-picker>
          </v-menu>
        </div>

        <div>
          <v-menu
            :close-on-content-click="false"
            v-model="menu2"
            :nudge-right="40"
            lazy
            transition="scale-transition"
            offset-y
            full-width
            min-width="290px"
          >
            <v-text-field
              slot="activator"
              v-model="date"
              label="チェックアウト"
              prepend-icon="event"
              hint="YYYY/MM/DD format"
              readonly
            ></v-text-field>
            <v-date-picker
              v-model="date"
              @input="menu2 = false"
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
      v-model="drawer"
      fixed
      app
    >
      <v-subheader>テンプレート</v-subheader>
      <v-list expand="true">
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
            @click="key=subItem.key"
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
    <v-flex
      xs12
      sm8
      md6
    >

      <v-card>
        <v-card-title class="headline">{{key}}2本目〜/PM〜他館</v-card-title>
        <v-card-text>
          <p>Vuetify is a progressive Material Design component framework for Vue.js. It was designed to empower developers to create amazing applications.</p>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            color="primary"
            flat
            nuxt
          >Copy to Clip</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  data: () => ({
    items: [
      {
        action: 'local_activity',
        title: '2本目〜',
        active: true,
        items: [
          { title: 'PM～他館', code: '2pm_other_hotel' },
          { title: 'PM～宿泊有', code: '2pm_my_hotel' },
          { title: 'PM～徒歩圏内宿', code: '2pm_other_near_hotel' }
        ]
      },
      {
        action: 'restaurant',
        title: '3本目〜',
        active: true,
        items: [
          { title: 'PM～他館' },
          { title: 'PM～宿泊有' },
          { title: 'PM～徒歩圏内宿' }
        ]
      },
      {
        action: 'school',
        title: 'Education',
        active: true,
        items: [{ title: 'List Item' }]
      },
      {
        action: 'directions_run',
        title: 'Family',
        active: true,
        items: [{ title: 'List Item' }]
      },
      {
        action: 'healing',
        title: 'Health',
        active: true,
        items: [{ title: 'List Item' }]
      },
      {
        action: 'content_cut',
        title: 'Office',
        active: true,
        items: [{ title: 'List Item' }]
      },
      {
        action: 'local_offer',
        title: 'Promotions',
        active: true,
        items: [{ title: 'List Item' }]
      }
    ]
  }),
  components: {
    Logo,
    VuetifyLogo
  },
  props: ['key']
}
</script>
