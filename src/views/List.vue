<template>
  <div id="list">
    <div style="position: relative;">
      <div style="margin: 112px 650px;position: relative;width: 522px;height: 87px;">
        <input type="text" id="mission" name="mission" v-model="newinput" :state="newinputstate" @keydown.enter="additem">
        <div class="plus" @click="additem">
        <a href="#" @click="additem">
            <font-awesome-icon icon="plus" style="color: white;font-size: 82px;padding: 8px 0 0 12px;" />
          </a>
        </div>
      </div>
      <router-link to="/">
        <div class="close-btn">
          <a href="#">
            <font-awesome-icon icon="times" style="color: #F44438;font-size: 85px;padding: 8px 0 0 12px;" />
          </a>
        </div>
      </router-link>
      <div class="task-list">
        <div class="all-tasks">
          <h1>All Tasks</h1>
          <ul class="all">
            <li v-for="item in items" v-bind:key="item">
              <label id="radio">
                <input type="checkbox" value="radio1" name="grp1" />
                <span class="text">{{ item.name }}</span>
              </label><br>
            </li>
            <li v-for="item in finished" v-bind:key="item">
              <label id="radio">
                <input type="checkbox" value="radio1" name="grp1" />
                <span class="text">{{ item }}</span>
              </label>
            </li>
          </ul>
        </div>
        <div class="done">
          <h1>Had Done</h1>
          <ul class="had">
            <li class="had-list" v-for="(item, idx) in finished" v-bind:key="(item, idx)"><span class="text">{{ item }}</span></li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      newinput: ''
    }
  },
  computed: {
    newinputstate () {
      return this.newinput.length > 2 ? true : this.newinput.length === 0 ? null : false
    },
    items () {
      return this.$store.state.items
    },
    finished () {
      return this.$store.state.finished
    }
  },
  methods: {
    additem () {
      if (this.newinput.length > 2) {
        this.$store.commit('additem', this.newinput)
        this.newinput = ''
      }
    }
  }
}
</script>

<style>
    body {
      font-family: 'Arial Rounded MT';
      width: 1920px;
      height: 100vh;
      overflow: hidden;
      background-color: #444444;
    }

    input[type='checkbox'] {
      width: 32px;
      height: 32px;
      border-radius: 50%;
      /*取消預設外觀*/
      -webkit-appearance:none;
      border: 5px solid #FFFFFF;
      background-color: #444444;
      margin-right: 15px;
      position: relative;
    }

    input[type='checkbox']:checked {
      border: 5px solid #FFFFFF;
      background-color: #444444;
    }

    input[type='checkbox']:checked::after {
      content:"●";
      font-size: 35px;
      font-weight: 300;
      color: #fff;
      line-height: 16px;
      position: absolute;
    }

    input[type='checkbox']:checked + span{
      color: #fff;
      text-decoration: line-through 5px solid #fff;
    }

    #mission {
      width: 435px;
      height: 87px;
      font-size: 30px;
      border-radius: 5px 0 0 5px;
      border: 2px solid #F44438;
      color: #F44438;
    }

    .plus {
      height: 87px;
      width: 87px;
      background-color: #F44438;
      position: absolute;
      top: 0;
      right: 1px;
      border-radius: 0 5px 5px 0;
    }

    .close-btn {
      position: absolute;
      top: -15px;
      right: 85px;
      width: 71px;
      height: 71px;
    }

    .task-list {
      display: flex;
      width: 1154px;
      margin: 0 auto;
      color: #fff;
    }

    .all-tasks {
      width: 959px;
      height: 710px;
    }

    .all-tasks ul {
      list-style: none;
      margin-top: 30px;
      padding-left: 95px;
    }

    .all-tasks h1 {
      font-size: 50px;
      padding-left: 90px;
    }

    .done h1 {
      font-size: 50px;
      padding-left: 150px;
    }

    .text {
      font-size: 30px;
      font-weight: normal;
    }

    .done {
      width: 959px;
      height: 710px;
    }

    .had {
      padding-left: 175px;
    }

    .all-tasks::after {
      content: "";
      position: absolute;
      top: 485px;
      left: 700px;
      transform: rotate(90deg);
      border-bottom: 4px solid #fff;
      width: 493px;
    }
</style>
