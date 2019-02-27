<template>
  <div>
    <form @submit.prevent="addWeightLog" :disabled="! weightLog">
      <div class="input-group">
        <div class="row">
          <input v-model="weightLog" type="text" class="form-control" placeholder="Enter your weight in lbs." autofocus>
          <input v-model="weightLogDate" type="date" class="form-control" placeholder="Weigh-in Date">
          <span class="input-group-btn">
            <button class="btn btn-default" type="submit" :disabled="! weightLog">Add</button>
          </span>
        </div>
      </div>
    </form>

    <ul class="list-group">
      <li v-for="weightLog in weightLogs" class="list-group-item" :key="weightLog.id">
        <label>
          {{ weightLog.text }}
        </label>
        <a @click.prevent="weightLogs.splice(weightLogs.indexOf(weightLog), 1)" class="delete pull-right" href="#">X</a>
      </li>
    </ul>
  </div>
</template>

<script>

var STORAGE_FILE = 'weightLogs.json'

export default {
  name: 'WeightLogForm',
  props: ['user'],
  data: () => {
    return {
      blockstack: window.blockstack,
      weightLogs: [],
      weightLog: '',
      weightLogDate: '',
      uidCount: 0
    }
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
  mounted () {
    this.fetchData()
  },
  methods: {
    addWeightLog () {
      if (!this.weightLog.trim()) {
        return
      }
      this.weightLogs.unshift({
        id: this.uidCount++,
        text: this.weightLog.trim(),
        completed: false
      })
      this.weightLog = ''
    },
    fetchData () {
      const blockstack = this.blockstack
      blockstack
        .getFile(STORAGE_FILE) // decryption is enabled by default
        .then(weightLogsText => {
          var weightLogs = JSON.parse(weightLogsText || '[]')
          weightLogs.forEach(function (weightLog, index) {
            weightLog.id = index
          })
          this.uidCount = weightLogs.length
          this.weightLogs = weightLogs
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
