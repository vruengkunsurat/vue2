<template>
  <div>
    <v-container class="bg-surface-variant">
      <v-row no-gutters>
        <v-col cols="12" sm="4">
          <v-sheet class="ma-2 pa-2">
            <v-text-field type="date" v-model="eventStartDate" />
            <v-text-field type="time" v-model="eventStartTime" />
            <!-- <v-text-field type="date" v-model="eventEndDate" />
            <v-text-field type="time" v-model="eventEndTime" /> -->
            <v-radio-group v-model="row" row>
              <v-radio label="Start Meeting" value="radio-1"></v-radio>
              <v-radio label="End Meeting" value="radio-2"></v-radio>
            </v-radio-group>
          </v-sheet>

        </v-col>

      </v-row>
    </v-container>
    <v-btn @click="addEvent">Add Event</v-btn>

    <FullCalendar :options="calendarOptions" />
  </div>
</template>

<script>
import FullCalendar from '@fullcalendar/vue'
import dayGridPlugin from '@fullcalendar/daygrid'
import interactionPlugin from '@fullcalendar/interaction'

export default {
  components: {
    FullCalendar
  },
  data() {
    return {
      textTitle: '', 
      eventStartDate: '', 
      eventStartTime: '', 
      eventEndDate: '', 
      eventEndTime: '', 
      addingEnd: false, 
      row: '',
      calendarOptions: {
        plugins: [dayGridPlugin, interactionPlugin],
        initialView: 'dayGridMonth',
        dateClick: this.handleDateClick,
        events: [],
        editable: true,
        eventDrop: this.handleEventDrop
      }
    }
  },
  methods: {
    handleDateClick(info) {
      alert('date click! ' + info.dateStr)
    },
    addEvent() {
      if (this.eventStartDate && this.eventStartTime) {
        const eventStartDateTime = `${this.eventStartDate}T${this.eventStartTime}`; 
        if (this.row === 'radio-1') { // Check if Option 1 is selected
          this.calendarOptions.events.push({ title: this.radioText(), start: eventStartDateTime }); // Add Option 1 event
          if (this.row === 'radio-2') { // Check if Option 2 is selected after Option 1
            this.calendarOptions.events.push({ title: this.radioText(), start: eventStartDateTime }); // Add Option 2 event
          }
        } else {
          alert('Please select Option 1 first.');
        }
        this.eventStartDate = ''; 
        this.eventStartTime = ''; 
        this.addingEnd = true; 
      } else {
        alert('Please select both start date and time.');
      }
    },
    radioText() {
      if (this.row === 'radio-1') {
        this.textTitle = 'Start Meeting';
      } else if (this.row === 'radio-2') {
        this.textTitle = 'End Meeting';
      }
      return this.textTitle; 
    }
  }
}
</script>