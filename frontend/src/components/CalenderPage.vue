<template>
  <div>
    <v-sheet height="6vh" class="d-flex align-center">
      <v-btn outlined small class="ma-4" @click="setToday">今日へジャンプ</v-btn>
      <v-btn icon @click="$refs.calender.prev()">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-btn icon @click="$refs.calender.next()">
        <v-icon>mdi-chevron-right</v-icon>
      </v-btn>
      <v-toolbar-title>{{ title }}</v-toolbar-title>
    </v-sheet>
    <v-sheet height="94vh">
      <v-calendar
        ref="calender"
        v-model="value"
        :events="events"
        @change="fetchEvents"
        locale="ja-jp"
        :day-format="(timestamp) => new Date(timestamp.date).getDate()"
        :month-format="(timestamp) => new Date(timestamp.date).getMonth() + 1 + ' /'"
        @click:event="showEvent"
        @click:day="initEvent"
      ></v-calendar>
    </v-sheet>
    <v-dialog :value="event !== null" @click:outside="closeDialog" width="600">
      <EventDetailDialog v-if="event !== null && !isEditMode" />
      <EventFormDialog v-if="event !== null && isEditMode" />
    </v-dialog>
  </div>
</template>

<script>
import { format } from "date-fns";
import { mapGetters, mapActions } from "vuex";
import EventDetailDialog from "./EventDetailDialog.vue";
import EventFormDialog from "./EventFormDialog.vue";
export default {
  name: "CalendarPage",
  components: { EventDetailDialog, EventFormDialog },
  data: () => ({
    // value: format(new Date(), "yyyy/MM/dd"),
    value: new Date("2021/07/01"),
  }),
  computed: {
    ...mapGetters("events", ["events", "event", "isEditMode"]),
    title() {
      return format(new Date(this.value), "yyyy年 M月");
    },
  },
  methods: {
    ...mapActions("events", ["fetchEvents", "setEvent", "setEditMode"]),
    setToday() {
      this.value = format(new Date(), "yyyy/MM/dd");
    },
    showEvent({ nativeEvent, event }) {
      this.setEvent(event);
      nativeEvent.stopPropagation();
    },
    closeDialog() {
      this.setEvent(null);
      this.setEditMode(false);
    },
    initEvent({ date }) {
      date = date.replace(/-/g, "/");
      const start = format(new Date(date), "yyyy/MM/dd 00:00:00");
      const end = format(new Date(date), "yyyy/MM/dd 01:00:00");
      this.setEvent({ name: "", start, end, timed: true });
      this.setEditMode(true);
    },
  },
};
</script>
