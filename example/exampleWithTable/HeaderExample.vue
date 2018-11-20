<template>
  <el-row
    type="flex">
    <el-select
      :clearable="true"
      value="allBranch"
      placeholder="Select">
      <el-option
        v-for="item in branchList"
        :key="item.value"
        :label="item.label"
        :value="item.value"/>
    </el-select>
    <el-select
      :clearable="true"
      value="allRegister"
      placeholder="Select Register"
      class="sm-lr-margin"
      :disabled="true">
      <el-option
        v-for="item in registersList"
        :key="item.value"
        :label="item.label"
        :value="item.value"/>
    </el-select>
    <el-select
      :value="staffValue"
      placeholder="Select Staff"
      class="sm-lr-margin"
      @change="changeStaff">
      <el-option
        v-for="item in staffList"
        :key="item.value"
        :label="item.label"
        :value="item.value"/>
    </el-select>
  </el-row>
</template>

<script>
export default {
  name: 'HeaderExample',
  data () {
    return {
      hideRegisters: true,
      branchList: [{ value: 'allBranch', label: 'All Branch' }],
      registersList: [{ value: 'allRegister', label: 'All Register' }],
      staffList: [{ value: 'allStaff', label: 'All Staff' }, { value: 'staffA', label: 'Staff A' }, { value: 'staffB', label: 'Staff B' }],
      staffValue: 'allStaff'
    }
  },
  methods: {
    changeStaff (value) {
      this.staffValue = value
      this.$emit('staff-changed', value)
    }
  }
}
</script>

<style scoped>
.width-full {
  width: 100%;
}

.sm-lr-margin {
  margin: 0px 10px;
}

.no-url {
  background-color: #525f69;
  height: 100%;
  display: flex;
  align-content: center;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: 500;
  font-size: 200%;
  user-select: none;
}

.no-url p {
  white-space: pre-line;
  text-align: center;
}
</style>
