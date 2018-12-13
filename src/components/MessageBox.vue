<template>
  <el-row type="flex">
    <transition name="el-fade-in-linear">
      <el-card
        shadow="never"
        class="message-box"
        v-if="show"
      >
        <el-row
          type="flex"
          class="row-bg"
          justify="space-between">
          <div
            class="message">
            {{ message }}
          </div>
          <div>
            <el-button
              class="closeBtn"
              @click="toggleShow">X</el-button>
          </div>
        </el-row>
      </el-card>
    </transition>
  </el-row>
</template>

<script>

export default {
  name: 'MessageBox',
  props: {
    message: {
      type: String,
      required: true
    },
    show: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    toggleShow () {
      this.$emit('toggle-show')
    }
  }
}
</script>

<style scoped>
.el-card__body {
  padding: 10px;
}
.message-box {
  border: 1px solid #269ff6;
  margin: 10px;
}
.closeBtn {
  border: none;
  font-size: 20px;
  font-weight: bolder;
  padding: 2px;
}
.message {
  margin: 25px;
  color: #269ff6;
  font-size: 18px;
}
</style>
