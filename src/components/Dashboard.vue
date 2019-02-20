<template>
  <div class="hello">
    <div class="container">
      <div class="row">
        <div class="col-md-8 col-md-offset-2">
          <h1 class="page-header">Thunderstack
            <img :src="user.avatarUrl() ? user.avatarUrl() : '/avatar-placeholder.png'" class="avatar">
            <small><span class="sign-out">(<a href="#" @click.prevent="signOut">Sign Out</a>)</span></small>
          </h1>
          <h2 class="user-info">
            <small>
              {{ user.name() ? user.name() : 'Nameless Person'}}'s Weight Logs
            </small>
            <small class="pull-right">
            {{ user.username ? user.username : user.identityAddress }}
            </small>

          </h2>
          <form @submit.prevent="addWeightLog" :disabled="! weightLog">
            <div class="input-group">
              <input v-model="weightLog" type="text" class="form-control" placeholder="Log your weight" autofocus>
            <!-- <input v-model="weightLogDate" type="date" class="form-control" placeholder="Date"> -->
              <span class="input-group-btn">
                <button class="btn btn-default" type="submit" :disabled="! weightLog">Add</button>
              </span>
            </div>
          </form>

          <ul class="list-group">
            <li v-for="weightLog in weightLogs"
              class="list-group-item"
              :class="{completed: weightLog.completed}"
              :key="weightLog.id">
              <label>
                <input type="checkbox" v-model="weightLog.completed">{{ weightLog.text }}
              </label>
              <a @click.prevent="weightLogs.splice(weightLogs.indexOf(weightLog), 1)"
                class="delete pull-right"
                href="#">X</a>
            </li>
          </ul>

        </div>
      </div>
    </div>
  </div>
</template>

<script>

var STORAGE_FILE = 'weightLogs.json'

export default {
  name: 'dashboard',
  components: {},
  props: ['user'],
  data () {
    return {
      blockstack: window.blockstack,
      weightLogs: [],
      weightLog: '',
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
      blockstack.getFile(STORAGE_FILE) // decryption is enabled by default
      .then((weightLogsText) => {
        var weightLogs = JSON.parse(weightLogsText || '[]')
        weightLogs.forEach(function (weightLog, index) {
          weightLog.id = index
        })
        this.uidCount = weightLogs.length
        this.weightLogs = weightLogs
      })
    },

    signOut () {
      this.blockstack.signUserOut(window.location.href)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
.list-group-item {
  &.completed label {
    text-decoration: line-through;
  }

  .delete {
    display: none;
  }

  &:hover .delete {
    display: inline;
    color: grey;
    &:hover {
      text-decoration: none;
      color: red;
    }
  }
}
</style>
