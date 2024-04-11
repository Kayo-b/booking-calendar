<template>
    <div class="main-container">
        <div class="header-container">
            <div class="week-nav">
                <div class="icon-container"  @click="goToPreviousWeek"><ArrowLeft class="arrow"/></div>
               <div class="week-container">
                <div class="week-range" v-for="(item, index) in weekDays">
                    <div id="date-1">{{ index === 0 ? `${item.day.getDate()}.${item.day.getMonth() + 1}.${item.day.getFullYear()}` : '' }}</div>
                    <div id="date-2">{{ index === 6 ? `${item.day.getDate()}.${item.day.getMonth() + 1}.${item.day.getFullYear()}` : '' }}</div>
                </div>
               </div>
                 
                <div class="icon-container" @click="goToNextWeek"><ArrowRight class="arrow"/></div>
            </div>
            <RentalStationSelector :stations="stations" @station-selected="stationSelected" />
        </div>  
        <WeekView :key="renderKey" :startDate="startDate" :selectedStationBookings="selectedStationBookings" @booking-details="bookingDetails" @week-days="updateWeekDays"/>
        <BookingDetailView :booking="booking" :clicked="clicked" :stationName="stationName"/>
    </div>
  </template>
    <script>
  import WeekView from './components/WeekView.vue';
  import RentalStationSelector from './components/RentalStationSelector.vue';
    import BookingDetailView from './components/BookingDetailView.vue';
    import ArrowLeft from './components/icons/arrow-prev-small.svg';
    import ArrowRight from './components/icons/arrow-next-small.svg';
  
  export default {
        components: {
            RentalStationSelector,
            WeekView,
            BookingDetailView,
            ArrowLeft,
            ArrowRight

        },
        data() {
            return {
                startDate: new Date("2021-03-13T22:04:19.032Z"),
                stations: [],
                selectedStationBookings: [],
                booking: {},
                clicked: false,
                stationName: null,
                weekDays: [],
                renderKey: 0
            }
        },

        mounted() {
            this.adjustStartDateToBeginningOfWeek();
            this.fetchStationsBookingData();
            
            /**Increment key value to re-render of WeekView component:
             * The weekDays Obj is generated in WeekView component and 
             * it is not reactive on mount for reasons I counldn't figure out.
             * Because of this I used this workaround to make the 
             * dates show the correct week on the first render. **/
            this.renderKey++;
        },
        methods: {
            // Adjusts the start date to be monday. 
            // without this the calendar will beggin on Saturday ("2021-03-13T22:04:19.032Z")
            adjustStartDateToBeginningOfWeek() {
                const dayOfWeek = this.startDate.getDay();
                console.log(dayOfWeek)
                const diff = this.startDate.getDate() - dayOfWeek + (dayOfWeek === 0 ? -6 : 1); 
                console.log(this.startDate)
                this.startDate.setDate(diff);
            },
            goToPreviousWeek() {
                this.changeWeekBy(-7);
            },
            goToNextWeek() {
                this.changeWeekBy(7);
            },
            changeWeekBy(days) {
                const newDate = new Date(this.startDate.valueOf()); 
                newDate.setDate(this.startDate.getDate() + days); 
                this.startDate = newDate; 
                this.adjustStartDateToBeginningOfWeek();
            },
            fetchStationsBookingData() {
                fetch('https://605c94c36d85de00170da8b4.mockapi.io/stations')
                .then(response => response.json())
                .then(data => {
                    this.stations = data;
                }).catch(error => {
                    console.error("Failed to fetch data", error)
                });
            },

            stationSelected(stationId) {
              const station = this.stations.find(station => station.id === stationId);
              this.selectedStationBookings = station ? station.bookings : [];
              this.stationName = station ? station.name : null;
            //   console.log(station.name, "<station")
            },

            bookingDetails(item) {
                // console.log(item, "<booking")
                this.booking = item ? item : {};
                this.clicked = !this.clicked;
                // console.log(this.clicked, "<clicked")
                // console.log(this.weekDays, "<weekdays")
            },
            updateWeekDays(weekDays) {
                console.log('Received weekDays from WeekView:', weekDays);
                this.weekDays = weekDays ? weekDays : [];
            }

        }
  }
  </script>
  
  <style scoped>
    .main-container {
        /* display: flex; */
        flex-direction: column;
        align-items: center;
        margin-top: 1rem;
        max-width: 100%;
    }
    .week-container {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 10px;
        width: 200px;
    }
    .week-nav {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        margin: 5px;
    }
    /* .week-range {
        display: flex;
        border: 1px;
        gap: 0px;
        width: auto;
  } */
  .header-container {
    display: flex;
    justify-content: space-around;
    align-items: center;
    width: 100%;
    margin-bottom: 1rem;
  }
  #date-1, #date-2{
    color: white;
    margin: 1px;

  }
  .icon-container {
    height: 50px;
    width: 50px;
  }
  .arrow {
    cursor: pointer;
    height: 50px;
    width: 50px;
    /* widows: ; */
    /* background-color: rgba(3, 81, 81, 0); */
    filter: invert(50%);
    padding: -10px;
  }
  </style>