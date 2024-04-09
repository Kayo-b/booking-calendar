<template>
    <div id="calendar-header">
A
    </div>
    <div class="calendar-grid">
      <div class="day" v-for="day in 7" :key="day">
        <div class="day-name">{{ getDayName(day) }}</div>
        <!-- Placeholder for time slots or events -->
        <!-- <div class="time-slots">
          <div class="time-slot" v-for="n in 24" :key="n">{{ n }}:00</div>
        </div> -->
      </div>
    </div>
  </template>
  
  <script>
  export default {
    methods: {
      getDayName(dayIndex) {
        // Simple method to return day names, customize as needed
        const dayNames = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
        return dayNames[dayIndex - 1];
      }
    }
  }
  </script>
  
  <style scoped>
  #calendar-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    height: 60px;
    border: 1px solid red;
}
  .calendar-grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr); /* 7 columns for each day */
    gap: 10px;
    padding: 20px;
    height: 100vh;
  }
  
  .day {
    display: flex;
    flex-direction: column;
    background-color: #2e2e2e;
    padding: 10px;
  }
  
  .day-name {
    text-align: center;
    margin-bottom: 10px;
    border-bottom: 1px solid red; /* Space between day name and time slots */
  }
  
  .time-slots .time-slot {
    border-top: 1px solid #e0e0e0; /* Visual separation for each hour */
    padding: 5px 0;
  }
  </style>
  