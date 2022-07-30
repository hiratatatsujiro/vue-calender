<template>
  <v-card class="pb-12">
    <v-card-actions class="d-flex justify-end pa-2">
      <v-btn icon @click="closeDialog">
        <v-icon size="20px">mdi-close</v-icon>
      </v-btn>
    </v-card-actions>
    <v-card-text>
      <DialogSections icon="mdi-square" :color="event.color">
        <v-text-field v-model="name" label="title"></v-text-field>
      </DialogSections>
      <DialogSections icon="mdi-clock-time-three-outline">
        <DateForm v-model="startDate" />
        <DateForm v-model="endDate" />
      </DialogSections>
    </v-card-text>
    <v-card-actions class="v-flex justify-end">
      <v-btn @click="submit">保存</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import DialogSections from "./DialogSection.vue";
import DateForm from "./DateForm.vue";

export default {
  name: "EventFormDialog",
  components: { DialogSections, DateForm },
  data: () => ({
    name: "",
    startDate: null,
    endDate: null,
  }),
  computed: {
    ...mapGetters("events", ["event"]),
  },
  created() {
    this.startDate = this.event.startDate;
    this.endDate = this.event.endDate;
  },
  methods: {
    ...mapActions("events", ["setEvent", "setEventMode", "createEvent"]),
    closeDialog() {
      this.setEventMode(false);
      this.setEvent(null);
    },
    submit() {
      const params = {
        name: this.name,
        start: this.startDate,
        end: this.endDate,
      };
      this.createEvent(params);
      this.closeDialog();
    },
  },
};
</script>

<style></style>
