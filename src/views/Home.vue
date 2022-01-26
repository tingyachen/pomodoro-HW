<template>
  <div id="home">
    <div label-for="newinput" style="margin: 112px 650px;position: relative;width: 522px;height: 87px;">
      <input type="text" id="mission" name="mission" v-model="newinput" @keydown.enter="additem" >
      <div class="plus" @click="additem">
        <a href="#" @click="additem">
        <font-awesome-icon icon="plus" style="color: white;font-size: 82px;padding: 8px 0 0 12px;" />
      </a>
      </div>
    </div>
    <router-link to="/list">
    <div class="menu">
      <a href="#">
        <font-awesome-icon icon="bars" style="color: #F44438;font-size: 85px;padding: 8px 0 0 12px;" />
      </a>
    </div>
    </router-link>
    <div class="timer" :class="{ breakColor: this.current == 'rest' }">
      <h1 class="time">{{ timeMinute }}
        <span style="font-size: 35px;">{{ timeSecond }}</span>
      </h1>
      <div class="start-btn" style="max-width: 187px;">
        <a href="#" :class="{ breakTime: this.current == 'rest' }" value="Start" v-if="status !== 1" @click="start">
          <span>Start</span>
        </a>
        <a href="#" :class="{ breakTime: this.current == 'rest' }" value="Reset" v-else @click="reset">
          <span style="letter-spacing: -3px;">Pause</span>
        </a>
      </div>
    </div>
    <ul class="work">
        <li v-for="item in items" v-bind:key="item.id" class="work-list">{{ item.name }}
        </li>
    </ul>
    <span style="text-decoration: underline;position: absolute;font-size: 45px;bottom: 110px;left: 730px;color: #595959;" :class="{ breakColor: this.current == 'rest' }">Work</span>
    <span style="position: absolute;font-size: 45px;bottom: 110px;left: 950px;color: #ADADAD;" :class="{ breakColor: this.current != 'rest' }">Break</span>
    <p style="color: #7E7E7E;position: absolute;bottom: 95px;left: 106px;font-size: 20px;">......Only show the first four tasks ......</p>
    <b-progress :value="value" height="43px" :style="progress" variant="info" max="1500"></b-progress>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 0 = 停止
      // 1 = 倒數中
      // 2 = 暫停
      newinput: '',
      status: 0,
      timer: 0,
      isBreak: true,
      value: 0,
      progress: {
        backgroundColor: '#f44438',
        position: 'absolute',
        bottom: '-8px',
        width: '100%'
      }
    }
  },
  methods: {
    additem () {
      if (this.newinput.length > 2) {
        this.$store.commit('additem', this.newinput)
        this.newinput = ''
      }
    },
    start () {
      if (this.status === 0 && this.items1.length > 0) {
        this.$store.commit('start')
      }
      if (this.current.length) {
        this.status = 1
        this.timer = setInterval(() => {
          this.$store.commit('countdown')
          this.value++
          if (this.timeleft <= -1) {
            this.finish(false)
          }
        }, 1000)
      }
    },
    finish () {
      clearInterval(this.timer)
      // 0 = 停止
      this.status = 0
      this.$store.commit('finish')
      this.value = 0
      if (this.items1.length > 0) {
        this.start()
      }
    },
    reset () {
      // 2 = 暫停
      this.status = 2
      clearInterval(this.timer)
    }
  },
  computed: {
    items () {
      return this.$store.state.items.map(item => {
        item.state = item.model.length > 2
        return item
      })
    },
    items1 () {
      return this.$store.state.items
    },
    current () {
      return this.$store.state.current
    },
    timeleft () {
      return this.$store.state.timeleft
    },
    timeMinute () {
      const m = Math.floor(this.timeleft / 60).toString().padStart(2, '0')
      // const s = Math.floor(this.timeleft % 60).toString().padStart(2, '0')
      return `${m}`
    },
    timeSecond () {
      const s = Math.floor(this.timeleft % 60).toString().padStart(2, '0')
      return `.${s}`
    }
  }
}
</script>
