<template>        
    <!-- <div class="week-range" v-for="(item, index) in weekDays">
      <div>{{ index === 0 ? `${item.day.getDate()}.${item.day.getMonth() + 1}.${item.day.getYear()}` : '' }}</div>
        <div>{{ index === 6 ? `${item.day.getDate()}.${item.day.getMonth() + 1}.${item.day.getYear()}` : '' }}</div>
    </div>   -->
    <div class="week">
     
        <!-- <div class="day" v-for="day in 7" :key="day">
        <div class="day-name">{{ getDayName(day) }}</div>
        </div> -->
      <div class="day" v-for="(item, index) in weekDays" :key="index">
        <div class="date-first-row"><span>{{item.day.toLocaleDateString('en-US', {weekday: 'long'})}}</span><span>{{ item.day.getDate() }}</span></div>
       
      </div>
       <div class="booking-column" v-for="(item) in weekDays">
           <div class="date" v-for="(item) in item.pickupArray">
                <div class="booking-pickup" @click="bookingDetails(item)">{{ item?.customerName }}</div>

           </div> 
           <div v-for="(item) in item.dropoffArray">
                <div class="booking-dropoff" @click="bookingDetails(item)">{{ item?.customerName }}</div>

           </div>
        </div>
    </div>
  </template>
  
  <script>
import { watch } from 'vue';

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
            let pickupArray = [];
            let dropoffArray = [];
            // day = day.toISOString().slice(0, 10);
            //   console.log(day, "<")
            //   console.log(this.startDate.getDay(), "<2")
            let allBookings = this.selectedStationBookings.length;
            for(let i = 0; i < allBookings; i++) {
                // if (this.isDateWithinRange(day, this.selectedStationBookings[i]?.startDate, this.selectedStationBookings[i]?.endDate)) {
                //     console.log("check")
                //     // return this.selectedStationBookings[0]?.customerName;
                //     bookedDay = this.selectedStationBookings[i]?.customerName;
                //     costumerArray.push(bookedDay);
                //     console.log(costumerArray, "<Name")
                // }
                if(day.toISOString().slice(0,10) === this.selectedStationBookings[i]?.startDate.slice(0,10)) {
      
                  bookedDay = this.selectedStationBookings[i];
                    console.log(this.selectedStationBookings, "<3 selected statin")
                    pickupArray.push(bookedDay);
                    // console.log(costumerArray, "<Name")
                }
                if(day.toISOString().slice(0,10) === this.selectedStationBookings[i]?.endDate.slice(0,10)) {
                    bookedDay = this.selectedStationBookings[i];
                    dropoffArray.push(bookedDay);
                    // console.log(costumerArray, "<Name")
                }
            }
            let test = day.toISOString().slice(0,10);
            // console.log(test.slice(0, 10), "<4")
            // console.log(this.selectedStationBookings[0]?.startDate.slice(0,10), "<3")
            // console.log(this.selectedStationBookings[0]?.endDate.slice(0,10), "<3")
            console.log(day.toLocaleDateString('en-US', {weekday: 'long'}), "DAY")
            return {day, pickupArray, dropoffArray};
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
        },
        bookingDetails(item) {
            this.$emit('booking-details', item);
        },
        emitWeekdays() {
            // console.log(this.weekDays.day.getYear(), "<Year !@!@!@")
            this.$emit('week-days', this.weekDays);
        },

    },
    watch: {
    // Watching startDate because weekDays depends on it
    startDate: {
      immediate: true, // Ensures the handler runs immediately on component mount
      handler() {
              console.log('startDate watcher triggered', this.weekDays);

        this.$emit('week-days', this.weekDays); // Emitting weekDays back to App.vue
      },
    },
  },
    mounted() {
        this.emitWeekdays();
        console.log(this.weekDays, "<weekdays MOUNTED!!!!!!!!!!!")
    }
    
  }

  </script>
  
  <style scoped>
  .week {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 2px;
  }
  /* .week-range {
    background-color: #383839;
    display: inline-block;
    border: 1px;
    gap: 10px;
    width: 20px;

  } */
  .day {
    padding: 10px;
    background-color: #383838;
  }
  .booking-column {
    padding:2px;
    background-color: #383838;
    min-height: 200px;
  }
  .booking-pickup {
    color:green;
    background-color: #383838;
    border: 1px solid black;
    margin: 2px;
  }
  .booking-dropoff {
    color: red;
    background-color: #383838;
    border: 1px solid black;
    margin: 2px;
  }
  .date-first-row {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  } 
  </style>
  