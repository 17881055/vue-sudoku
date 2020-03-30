<template>
  <div id="together" class="wrap">
    <h4 class="title">
      数独
      <span class="font-12"
        >满足每一行、每一列、每一个粗线宫（3*3）内的数字均含1-9，不重复。</span
      >
    </h4>
    <div class="mean">
      <a-row>
        <a-col :span="6">
          <a-button
            type="primary"
            :disabled="start"
            size="small"
            @click="handleStartClick"
            >开始</a-button
          >
          <a-button
            type="primary"
            :disabled="!start"
            size="small"
            @click="handleEndClick"
            >停止</a-button
          >
        </a-col>

        <a-col :span="6">
          <a-radio-group
            size="small"
            :disabled="start"
            buttonStyle="solid"
            @change="handleChange"
            v-model="config.level"
          >
            <a-radio-button value="1">简单</a-radio-button>
            <a-radio-button value="2">正常</a-radio-button>
            <a-radio-button value="3">困难</a-radio-button>
          </a-radio-group>
        </a-col>

        <a-col :span="4">
          <a-switch
            size="small"
            checkedChildren="鼠标效果开"
            unCheckedChildren="鼠标效果关"
            defaultChecked
            @change="handlSwitchChange"
          />
        </a-col>
      </a-row>
    </div>
    <Sudoku :config="config" :start="start" :showMouseHover="showMouseHover" />
  </div>
</template>

<script>
import Sudoku from "./components/Sudoku";

export default {
  name: "App",
  components: {
    Sudoku
  },
  data() {
    return {
      config: {
        level: "1"
      },
      start: false,
      value: null,
      showMouseHover: true
    };
  },
  mounted() {},
  methods: {
    // moment,
    handleChange(e) {
      console.log(`checked = ${e.target.value}`);
      this.config.level = e.target.value;
    },
    handlSwitchChange(checked) {
      this.showMouseHover = checked;
    },
    handleStartClick() {
      this.start = true;
    },
    handleEndClick() {
      this.start = false;
    }
  }
};
</script>

<style lang="scss" scoped>
#together {
}
.title {
  padding: 10px;
  font-size: 22px;
  width: 900px;
  border-bottom: 1px solid #f1f1f1;
  .font-12 {
    font-size: 12px;
  }
}
.mean {
  padding: 20px;
  width: 40%;
}
.wrap {
  padding: 20px;
  margin: 0 auto;
  min-height: calc(100vh - 270px);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
