<template>
  <v-container>
    <v-tabs
        v-model="tab"
        align-with-title
    >
      <v-tabs-slider color="yellow"></v-tabs-slider>

      <v-tab
          v-for="item in tabItems"
          :key="item"
          class="ml-0"
      >
        {{ item }}
      </v-tab>
      <v-spacer></v-spacer>
      <v-btn outlined color="#3c2bc7">Add new</v-btn>
      <v-btn color="#3c2bc7" class="ml-1" icon outlined right @click="changeView()">
        <v-icon size="30">mdi-view-{{icon_view == 'list' ? 'grid' : 'list'}}</v-icon>
      </v-btn>
    </v-tabs>
    <v-tabs-items v-model="tab">
      <v-tab-item
          v-for="item in tabItems"
          :key="item"
      >
        <v-row>
          <v-col
              cols="8"
              md="4" class="pa-8">
            <div>
              <v-sheet
                  tile
                  height="54"
                  class="d-flex"
              >
                <v-btn
                    icon
                    class="ma-2"
                    @click="$refs.calendar.prev()"
                >
                  <v-icon>mdi-chevron-left</v-icon>
                </v-btn>
                <v-select
                    v-model="weekday"
                    :items="weekdays"
                    dense
                    outlined
                    hide-details
                    label="weekdays"
                    class="ma-2"
                ></v-select>
                <v-spacer></v-spacer>
                <v-btn
                    icon
                    class="ma-2"
                    @click="$refs.calendar.next()"
                >
                  <v-icon>mdi-chevron-right</v-icon>
                </v-btn>
              </v-sheet>
              <v-sheet height="350">
                <v-calendar
                    ref="calendar"
                    v-model="value"
                    :weekdays="weekday"
                    :type="type"
                    :events="events"
                    :event-overlap-mode="mode"
                    :event-overlap-threshold="30"
                    :event-color="getEventColor"
                    @change="getEvents"
                ></v-calendar>
              </v-sheet>
            </div>
          </v-col>
          <v-col>
            <div class="mt-3" v-for="(item, index) in toDoData" :key="index">
              <h4 class="mb-2">{{item.date}}</h4>
              <grid-layout :layout="item.layout"
                           :col-num="12"
                           :row-height="30"
                           :is-draggable="true"
                           :is-resizable="false"
                           :vertical-compact="true"
                           :use-css-transforms="true"
              >
                <grid-item v-for="(item, index) in item.layout"
                           :x="item.x"
                           :y="item.y"
                           :w="item.w"
                           :h="item.h"
                           :i="item.i"
                           v-bind:key="index"
                >
                  <v-card class="overflow-x-auto" color="transparent" elevation="0">
                    <v-card-title class="text-h6">
                      Note {{index}}
                    </v-card-title>
                    <v-card-subtitle class="pb-0 mt-0">
                      <v-checkbox
                          class="mt-0"
                          label="Lorem ipsum dolor..."
                      ></v-checkbox>
                    </v-card-subtitle>
                    <v-card-actions class="mt-0 pt-0">
                      <v-btn icon><v-icon color="blue darken-1" size="20">mdi-share-variant</v-icon></v-btn>
                      <v-spacer></v-spacer>
                      <v-btn text @click="toDoDialog = !toDoDialog">View</v-btn>
                    </v-card-actions>
                  </v-card>
                  <span class="remove"><v-icon size="20" color="#3c2bc7">mdi-trash-can-outline</v-icon></span>
                </grid-item>
              </grid-layout>
            </div>
          </v-col>
        </v-row>
      </v-tab-item>
    </v-tabs-items>
    <v-dialog
        transition="dialog-bottom-transition"
        max-width="600"
        v-model="toDoDialog"
    >
      <template v-slot:default="dialog">
        <v-card>
          <v-toolbar
              color="primary"
              dark
          >Note 1</v-toolbar>
          <v-toolbar dense elevation="1">
            <v-overflow-btn
                :items="dropdown_font"
                label="Font"
                hide-details
                class="pa-0"
            ></v-overflow-btn>

            <template v-if="$vuetify.breakpoint.mdAndUp">
              <v-divider vertical></v-divider>
              <v-overflow-btn
                  :items="dropdown_edit"
                  editable
                  label="Size"
                  hide-details
                  class="pa-0"
                  overflow
              ></v-overflow-btn>

              <v-divider vertical></v-divider>

              <v-spacer></v-spacer>

              <v-btn-toggle
                  v-model="toggle_multiple"
                  color="primary"
                  dense
                  group
                  multiple
              >
                <v-btn
                    :value="1"
                    text
                >
                  <v-icon>mdi-format-bold</v-icon>
                </v-btn>

                <v-btn
                    :value="2"
                    text
                >
                  <v-icon>mdi-format-italic</v-icon>
                </v-btn>

                <v-btn
                    :value="3"
                    text
                >
                  <v-icon>mdi-format-underline</v-icon>
                </v-btn>
              </v-btn-toggle>

              <div class="mx-4"></div>

              <v-btn-toggle
                  v-model="toggle_exclusive"
                  color="primary"
                  dense
                  group
              >
                <v-btn
                    :value="1"
                    text
                >
                  <v-icon>mdi-format-list-bulleted</v-icon>
                </v-btn>
                <v-btn
                    :value="2"
                    text
                >
                  <v-icon>mdi-format-list-checkbox</v-icon>
                </v-btn>
                <v-btn
                    :value="3"
                    text
                >
                  <v-icon>mdi-format-list-numbered</v-icon>
                </v-btn>
              </v-btn-toggle>
            </template>
          </v-toolbar>
          <v-card-text>
            <v-checkbox
                v-for="i in 5"
                :key="i"
                label="Lorem ipsum dolor sit amet, consectetur adipiscing elit"
            ></v-checkbox>
            <v-btn v-show="!addNewElem" text @click="addNewElem = !addNewElem">+ Add New Element</v-btn>
            <v-text-field
                v-show="addNewElem"
                label="Regular"
                clearable
            ></v-text-field>
          </v-card-text>
          <v-card-actions class="justify-end">
            <v-btn
                text
                color="red darken-1"
                @click="dialog.value = false"
            >Close</v-btn>
            <v-btn
                text
                @click="dialog.value = false"
                color="green darken-1"
            >Save</v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </v-dialog>
  </v-container>
</template>

<script>
import VueGridLayout from 'vue-grid-layout';

  export default {
    name: 'HelloWorld',

    components: {
      GridLayout: VueGridLayout.GridLayout,
      GridItem: VueGridLayout.GridItem
    },
    data: () => ({
      tab: null,
      tabItems: [
        'All', 'Finished', 'Ongoing', 'Shared With Me',
      ],
      icon_view: 'grid',
      toDoDialog: false,
      addNewElem: false,
      dropdown_font: [
        { text: 'Arial' },
        { text: 'Calibri' },
        { text: 'Courier' },
        { text: 'Verdana' },
      ],
      dropdown_edit: [
        { text: '100%' },
        { text: '75%' },
        { text: '50%' },
        { text: '25%' },
        { text: '0%' },
      ],
      toggle_exclusive: 2,
      toggle_multiple: [1, 2, 3],
      type: 'month',
      types: ['month', 'week', 'day', '4day'],
      mode: 'stack',
      modes: ['stack', 'column'],
      weekday: [0, 1, 2, 3, 4, 5, 6],
      weekdays: [
        { text: 'Sun - Sat', value: [0, 1, 2, 3, 4, 5, 6] },
        { text: 'Mon - Sun', value: [1, 2, 3, 4, 5, 6, 0] },
        { text: 'Mon - Fri', value: [1, 2, 3, 4, 5] },
        { text: 'Mon, Wed, Fri', value: [1, 3, 5] },
      ],
      value: '',
      events: [],
      colors: ['blue', 'indigo', 'deep-purple', 'yellow'],
      names: ['Meeting', 'Holiday', 'PTO', 'Travel', 'Event'],
    }),
    computed:{
      toDoData(){
        return {
          obj1:{
            date: 'Today',
            layout: [
              {"x":0,"y":0,"w":4,"h":4,"i":"0"},
              {"x":4,"y":0,"w":4,"h":4,"i":"1"},
            ],
          },
          obj2:{
            date: '05-08-2022',
            layout: [
              {"x":0,"y":0,"w":4,"h":4,"i":"0"},
              {"x":4,"y":0,"w":4,"h":4,"i":"1"},
              {"x":8,"y":0,"w":4,"h":4,"i":"2"},
            ]
          },
          obj3:{
            date: '02-08-2022',
            layout: [
              {"x":0,"y":0,"w":4,"h":4,"i":"0"},
              {"x":4,"y":0,"w":4,"h":4,"i":"1"},
              {"x":8,"y":0,"w":4,"h":4,"i":"2"},
            ]
          }
        }
      }
    },
    methods:{
      changeView(){
          this.icon_view = this.icon_view === 'grid' ? 'list' : 'grid'
      },
      getEvents ({ start, end }) {
        const events = []

        const min = new Date(`${start.date}T00:00:00`)
        const max = new Date(`${end.date}T23:59:59`)
        const days = (max.getTime() - min.getTime()) / 86400000
        const eventCount = this.rnd(days, days + 20)

        for (let i = 0; i < eventCount; i++) {
          const allDay = this.rnd(0, 3) === 0
          const firstTimestamp = this.rnd(min.getTime(), max.getTime())
          const first = new Date(firstTimestamp - (firstTimestamp % 900000))
          const secondTimestamp = this.rnd(2, allDay ? 288 : 8) * 900000
          const second = new Date(first.getTime() + secondTimestamp)

          events.push({
            name: this.names[this.rnd(0, this.names.length - 1)],
            start: first,
            end: second,
            color: this.colors[this.rnd(0, this.colors.length - 1)],
            timed: !allDay,
          })
        }

        this.events = events
      },
      getEventColor (event) {
        return event.color
      },
      rnd (a, b) {
        return Math.floor((b - a + 1) * Math.random()) + a
      },
    }
  }
</script>

<style>
.vue-grid-layout {
  background: #c5a8e329;
}
.vue-grid-item:not(.vue-grid-placeholder) {
  background: #ffffff;
  border: 1px solid #3c2bc7;
  border-radius: 5px;
}
#tools .v-toolbar__content{
  padding: 0 !important;
}

.remove {
  position: absolute;
  right: 2px;
  top: 0;
  cursor: pointer;
}
</style>
