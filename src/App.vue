<template>
 
  <div class="container">
    <h1>covid data for bangladesh</h1>

  <h2>positive count</h2>
    <div v-if="arrComfirmed.length > 0">
    <LineChart
          :chartData="arrComfirmed"
          :options="chartOptions"
          :chartColors="positiveChartColors"
          label="Positive"
        />
  </div>
<h2>death count</h2>
<div v-if="arrDeath.length > 0">
  <LineChart
          :chartData="arrDeath"
          :options="chartOptions"
          :chartColors="deathColour"
          label="death"
        />
        </div>
      
  </div>
</template>

<script>
 import axios from 'axios'
 import moment  from 'moment'
 import LineChart from "./components/LineChart";
export default {
 
 components: {
    LineChart
  },
 
 data () {
    return {
      arrComfirmed:[],
      arrDeath:[],
      arrRecovered:[],
      arrActive:[],

      positiveChartColors: {
        borderColor: "#077187",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "#AFD6AC",
        backgroundColor: "#74A57F"
      },
       deathColour: {
        borderColor: "#077187",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "#AFD6AC",
        backgroundColor: "#E14D4D"
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
        
      }
    }
  },
  async created () {
    const  {data}=await axios.get('https://api.covid19api.com/live/country/bangladesh/status/confirmed')
    // this.date=data[1].Date
  
   
  let pastcount=0
  let presentcount=0
  let pastdeath=0
  let presendtdeath=0
  data.forEach(
    d=>{
      const date=moment(d.Date,"YYYYMMDD").format("MM/DD")
      const {Confirmed,Deaths,Recovered,Active}=d
       presentcount=Confirmed-pastcount
        pastcount=Confirmed
       
      this.arrComfirmed.push({date,total:Confirmed,count:presentcount})

      presendtdeath=Deaths-pastdeath
      pastdeath=Deaths
      this.arrDeath.push({date,total:Deaths,count:presendtdeath})

      this.arrRecovered.push({date,total:Recovered})
      this.arrActive.push({date,total:Active})


    }

  )

  this.arrComfirmed.shift();
  this.arrDeath.shift();
    
  },

  name: 'App',
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
