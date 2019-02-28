<template>
  <div>
    <ul class="list-group">
      <li v-for="weightLog in weightLogs" class="list-group-item" :key="weightLog.id">
        <label>
          {{ weightLog.text }} lbs. ----
          Date: {{ weightLog.date }}
        </label>
        <a @click.prevent="weightLogs.splice(weightLogs.indexOf(weightLog), 1)" class="delete pull-right" href="#">X</a>
      </li>
    </ul>
  </div>
</template>
<script>

var STORAGE_FILE = 'weightLogs.json'

export default {
  name: 'WeightLogList',
  props: ['user', 'weightLogs'],
  data: () => {
    return {
      blockstack: window.blockstack,
      weightLog: {},
      weightLogInLbs: '',
      weightLogDate: '',
      uidCount: 0
    }
  },
  mounted () {
    this.fetchData()
  },
  methods: {
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
          this.weightLogs = weightLogs
        })
    }
  }
}
</script>
