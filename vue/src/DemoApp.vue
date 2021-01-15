<script>
import FullCalendar from '@fullcalendar/vue'
import dayGridPlugin from '@fullcalendar/daygrid'
import '@fullcalendar/core/main.css'
import '@fullcalendar/daygrid/main.css'

export default {

  components: {
    FullCalendar
  },

  data: function() {
    return {
      calendarPlugins: [ dayGridPlugin ],
      isoEvents: []
    }
  },

  computed: {
    eventData () {
      return this.isoEvents.map(event => {
        const newEventObj = {}
        newEventObj.start = event.date
        newEventObj.title = event.title
        newEventObj.description = this.richToHtml(event.descriptionRich)
        newEventObj.textColor = event.isolationCategory.fields?.colorCode
        newEventObj.backgroundColor = 'transparent'
        newEventObj.borderColor = 'transparent'
        return newEventObj
      })
    }
  },

  methods: {
    eventRenderFunc (info) { // eslint-disable-next-line
      new Tooltip(info.el, {
        title: `<h4>${info.event.title}</h4> ${info.event.extendedProps.description}`,
        placement: 'top',
        trigger: 'hover',
        container: '.calendar',
        html: true
      })
    },
  }
}
</script>

<template>
  <div class='demo-app'>
    <div class='demo-app-main'>
      <FullCalendar
        defaultView="dayGridMonth"
        :plugins="calendarPlugins"
        :events="eventData"
        :eventRender="eventRenderFunc"
        :weekends="false"
        :displayEventTime="false"
        :fixedWeekCount="false"
      />
    </div>
  </div>
</template>
