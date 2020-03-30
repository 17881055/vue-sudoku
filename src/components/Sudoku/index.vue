<template>
  <div class="main">
    <Timer :start="start" :result="result" class="timer" />
    <Info :start="start" :result="result" class="info" />
    <Board
      class="board"
      :start="start"
      :showMouseHover="showMouseHover"
      :model="boardModel"
      @eventCellClick="handleCellClickEvent"
      @eventSudokuResult="handleResultEvent"
    />
    <SelectBar :start="start" @eventNumberClick="handleButtonClick" class="selectBar" />
  </div>
</template>

<script>
import Info from "./components/Info";
import Timer from "./components/Timer";
import Board from "./components/Board";
import SelectBar from "./components/SelectBar";

export default {
  components: {
    SelectBar,
    Board,
    Timer,
    Info
  },
  data() {
    return {
      result: null,
      boardModel: {
        selectCell: null,
        selectBarNumber: null
      },
      selectBarModel: {
        able: false
      }
    };
  },
  props: {
    config: {
      type: Object,
      default: function() {
        return {
          level: 1
        };
      },
      required: false
    },
    start: {
      type: Boolean,
      default: false,
      required: true
    },
    showMouseHover: {
      type: Boolean,
      default: true,
      required: false
    }
  },
  watch: {
    start(val) {
      if (val == true) {
        this.boardModel.level = this.config.level;
      }
    }
  },
  beforeCreate() {},
  mounted() {},
  methods: {
    handleResultEvent(result) {
      //{total: 15, empty: 14, error: 0, success: false}
      this.result = result;
      // console.log(result);
    },
    handleCellClickEvent(cell) {
      this.boardModel.selectCell = cell;
      this.boardModel.selectBarNumber = cell.number;
    },
    handleButtonClick(number) {
      this.boardModel.selectBarNumber = number;
    }
  }
};
</script>

<style lang="scss" scoped>
.main {
  position: relative;
  width: 600px;
  height: 480px;
  //   border: 1px solid rgb(209, 207, 207);
}
.info {
  position: absolute;
  left: 120px;
  top: 0px;
  font-size: 18px;
  font-weight: bold;
  color: #228cbd;
}

.timer {
  position: absolute;
  left: 0px;
  top: 0px;

  font-size: 18px;
  color: #a8a7a7;
}
.board {
  position: absolute;
  left: 0px;
  bottom: 0px;
  width: 450px;
  //   border: 1px solid #757575;
  background: #e6e6e6;
}
.selectBar {
  position: absolute;
  right: 30px;
  bottom: 200px;
  width: 100px;

  //  background: #e6e6e6;
}
</style>