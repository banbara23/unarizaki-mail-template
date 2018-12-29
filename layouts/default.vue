<template>
  <v-app>

    <!-- 右ドロワー -->
    <v-navigation-drawer
      v-model="drawerRight"
      fixed
      right
      clipped
      app
    >
      <div class="container fluid pb-0">
        <!-- <v-subheader class="black--text title">入力</v-subheader> -->
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

        <!-- <div>
          <v-text-field
            label="チェックイン"
            placeholder=""
          ></v-text-field>
        </div> -->
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

    <!-- ツールバー -->
    <v-toolbar
      color="blue-grey"
      dark
      fixed
      app
      clipped-right
    >
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title v-text="title" />
      <v-spacer></v-spacer>
      <v-toolbar-side-icon @click.stop="drawerRight = !drawerRight"></v-toolbar-side-icon>
    </v-toolbar>

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
          :prepend-icon="item.action"
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
            @click=""
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

    <!-- <v-navigation-drawer
      v-model="left"
      temporary
      fixed
    ></v-navigation-drawer> -->

    <!-- コンテンツ -->
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>

  </v-app>
</template>

<script>
export default {
  data: () => ({
    drawer: null,
    drawerRight: null,
    // right: false,
    // left: false,
    title: 'うなりざきメールテンプレ',
    items: [
      {
        action: 'local_activity',
        title: '2本目〜',
        active: true,
        items: [
          { title: 'PM～他館' },
          { title: 'PM～宿泊有' },
          { title: 'PM～徒歩圏内宿' }
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
    ],
    valid: false,
    date: new Date().toISOString().substr(0, 10)
    // nameRules: [v => !!v || 'required']
  })
}
</script>
