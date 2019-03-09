<template>
  <div>
    <div class="Chart">    
      <WeightLogChart v-if="loaded" :chartData="weightLogsInLbs" :chartLabels="dateLabels" :key="weightLogChartKey" />
    </div>
    <form @submit.prevent="addWeightLog" :disabled="! weightLog">
      <div class="input-group">
        <div class="row">
          <input v-model="weightLogInLbs" type="text" class="form-control" placeholder="Enter your weight in lbs." autofocus>
          <input v-model="weightLogDate" type="date" class="form-control" default="">
          <span class="input-group-btn">
            <button class="btn btn-default" type="submit" :disabled="! weightLog || !weightLogDate">Add</button>
          </span>
        </div>
      </div>
    </form>
    <div class="container-fluid">
      <WeightLogList :weightLogs="weightLogs" :key="weightLogListKey"/>
    </div>
  </div>
</template>

<script>
import WeightLogChart from './WeightLogChart'
import WeightLogList from './WeightLogList'
var STORAGE_FILE = 'weightLogs.json'

export default {
  name: 'WeightLogForm',
  components: { WeightLogChart, WeightLogList },
  props: ['user'],
  data: () => {
    return {
      blockstack: window.blockstack,
      weightLogs: [],
      weightLog: {},
      weightLogInLbs: '',
      weightLogsInLbs: [],
      weightLogDate: '',
      dateLabels: [],
      uidCount: 0,
      loaded: false,
      weightLogChartKey: 0,
      weightLogListKey: 0
    }
  },
  mounted () {
    this.fetchData()
  },
  watch: {
    weightLogs: {
      handler: function (weightLogs) {
        const blockstack = this.blockstack

        // encryption is now enabled by default
        return blockstack.putFile(STORAGE_FILE, JSON.stringify(weightLogs))
      },
      deep: true
    }
  },
  methods: {
    forceRerender () {
      this.weightLogChartKey += 1
      this.weightLogListKey += 1
    },
    addWeightLog () {
      const weightLogInLbsIsValid = typeof parseFloat(this.weightLogInLbs.trim()) === 'number'
      const weightLogDateIsValid = !!this.weightLogInLbs.trim()
      if (!weightLogInLbsIsValid || !weightLogDateIsValid) {
        console.log('form input is invalid')
        return
      }
      this.weightLogs.unshift({
        id: this.uidCount++,
        text: parseFloat(this.weightLogInLbs.trim()),
        date: this.weightLogDate.trim(),
        completed: false
      })
      this.weightLog = {}
      this.weightLogInLbs = ''
      this.weightLogDate = ''
      this.forceRerender()
    },
    fetchData () {
      const blockstack = this.blockstack
      blockstack
        .getFile(STORAGE_FILE) // decryption is enabled by default
        .then(weightLogsText => {
          var weightLogs = JSON.parse(weightLogsText || '{}')
          weightLogs.forEach(function (weightLog, weightLogInLbs, weightLogDate, index) {
            weightLog.id = index
          })
          this.uidCount = weightLogs.length
          this.weightLogs = weightLogs.sort(function (a, b) {
              // Turn your strings into dates, and then subtract them
              // to get a value that is either negative, positive, or zero.
            return new Date(a.date) - new Date(b.date)
          })
          this.dateLabels = weightLogs.map(weightLog => weightLog.date)
          this.weightLogsInLbs = weightLogs.map(weightLog => weightLog.text)
          this.loaded = true
        })
    }
  }
}
</script>
<style lang="scss" scoped>
input::placeholder {
  color: grey;
}

label {
  margin-bottom: 0;
  // width: 100%;
  cursor: pointer;
  input[type="checkbox"] {
    margin-right: 5px;
  }
}
</style>
