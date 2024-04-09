<template>        
    <div class="week-range" v-for="(item, index) in weekDays">
        <div>{{ index === 0 ? `${item.day.getDate()}.${item.day.getMonth() + 1}` : '' }}</div>
        <div>{{ index === 6 ? `${item.day.getDate()}.${item.day.getMonth() + 1}` : '' }}</div>
    </div>  
    <div class="week">
     
        <div class="day" v-for="day in 7" :key="day">
        <div class="day-name">{{ getDayName(day) }}</div>
        </div>
      <div class="day" v-for="(item, index) in weekDays" :key="index">
        <div class="date">{{ item.day.getDate() }}</div>
        <div class="date">{{ item.bookedDay}}</div>
      </div>
    </div>
  </template>
  
  <script>
export default {
    props: {
      startDate: Date,
      selectedStationBookings: Array
    },
    
    computed: {
      weekDays() {
        return Array.from({ length: 7 }).map((_, index) => {
            const day = new Date(this.startDate);
            day.setDate(day.getDate() + index);
            let bookedDay;
            // day = day.toISOString().slice(0, 10);
            //   console.log(day, "<")
            //   console.log(this.startDate.getDay(), "<2")
            let allBookings = this.selectedStationBookings.length;
            for(let i = 0; i < allBookings; i++) {
                if (this.isDateWithinRange(day, this.selectedStationBookings[i]?.startDate, this.selectedStationBookings[i]?.endDate)) {
                    console.log("check")
                    // return this.selectedStationBookings[0]?.customerName;
                    bookedDay = this.selectedStationBookings[i]?.customerName;
                }
            }
            // if (this.isDateWithinRange(day, this.selectedStationBookings[0]?.startDate, this.selectedStationBookings[0]?.endDate)) {
            //     console.log("check")
            //     // return this.selectedStationBookings[0]?.customerName;
            //     const bookedDay = this.selectedStationBookings[0]?.customerName;
            //     return bookedDay;
            // }
            let test = day.toISOString().slice(0,10);
            // console.log(test.slice(0, 10), "<4")
            // console.log(this.selectedStationBookings[0]?.startDate.slice(0,10), "<3")
            // console.log(this.selectedStationBookings[0]?.endDate.slice(0,10), "<3")
            return {day, bookedDay};
        });
      }
    },
    methods: {
    getDayName(dayIndex) {
      const dayNames = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
      return dayNames[dayIndex - 1];
    },
    isDateWithinRange(calendarDate, beginDate, endDate) {
      const formattedCalendarDate = calendarDate.toISOString().slice(0, 10);
      const formattedBeginDate = beginDate?.slice(0, 10);
      const formattedEndDate =  endDate?.slice(0, 10);
      return formattedCalendarDate >= formattedBeginDate && formattedCalendarDate <= formattedEndDate;
    }
  }
  }
  </script>
  
  <style scoped>
  .week {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 10px;
  }
  .week-range {
    background-color: #383839;
    display: inline-block;
    border: 1px;
    gap: 10px;
    width: 20px;

  }
  .day {
    padding: 20px;
    background-color: #383838;
  }
  </style>
  