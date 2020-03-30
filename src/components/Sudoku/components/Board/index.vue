<template>
  <div class="board">
    <template v-for="(item, i) in row">
      <div class="row" :key="i">
        <template v-for="(item, j) in col">
          <Cell
            :control="getCellControl(i,j)"
            @eventCellClick="handleCellClickEvent"
            @eventMouseOver="handlCellMouseEvent"
            @eventMouseOut="handlCellMouseEvent"
            :key="`${i}-${j}`"
          />
        </template>
      </div>
    </template>
  </div>
</template>

<script>
import Cell from "./Cell";
import EVENT from "../../event";
export default {
  components: {
    Cell
  },
  props: {
    start: {
      type: Boolean,
      default: false,
      required: true
    },
    model: {
      type: Object,
      default: function() {
        return {
          level: 1,
          selectCell: null,
          selectBarNumber: null
        };
      },
      required: true
    },
    showMouseHover: {
      type: Boolean,
      default: true,
      required: false
    }
  },

  data() {
    return {
      cells: [],
      row: 9,
      col: 9
    };
  },

  created() {},
  watch: {
    start(val) {
      if (val == true) {
        this.initGrid();
        this.sendResult();
      } else {
        this.gameOver();
      }
    },
    model: {
      handler: function() {
        this.cellwriteNumber();
        this.sendResult();
      },
      deep: true
    },
    cells: {
      handler: function() {
        // console.log(newValue);
      },
      deep: true
    }
  },
  methods: {
    //初始化Grid
    initGrid() {
      this.cells = [];
      for (let i = 0; i < this.row; i++) {
        for (let j = 0; j < this.col; j++) {
          let cell = {
            number: null,
            hover: false,
            selected: false,
            disable: false,
            box: this.whichBox(i, j),
            x: i,
            y: j
          };
          this.cells.push(cell);
        }
      }

      // let a = new Date();
      // console.log("start");
      this.generateGame(this.model.level);
      // console.log("end", new Date() - a);
    },
    sendResult() {
      let result = true;
      if (this.getDefectCell() === 0) {
        result = this.cells.some(cell => {
          return !this.check(cell, cell.number);
        });
      }
      // console.log(!result);
      this.$emit(EVENT.SUDOKU_RESULT, {
        total: this.model.level * 15,
        empty: this.getDefectCell(),
        error: result,
        success: !result
      });
    },
    gameOver() {
      this.cells = [];
      for (let i = 0; i < this.row * this.col; i++) {
        let cell = {
          number: null,
          hover: false,
          selected: false,
          disable: false,
          box: null,
          x: "-",
          y: "-"
        };
        this.cells.push(cell);
      }

      // console.log("gameOver", this.cells);
    },
    //判断属于哪个宫 B1,B2,B3...B9
    whichBox(x, y) {
      let r = parseInt(x / 3) + "" + parseInt(y / 3);
      const BoxMap = {
        "00": "B1",
        "01": "B2",
        "02": "B3",
        "10": "B4",
        "11": "B5",
        "12": "B6",
        "20": "B7",
        "21": "B8",
        "22": "B9"
      };
      return BoxMap[r];
    },
    //检查是否有重复数；
    check(targetCell, num) {
      let { cells } = this;

      for (let index = 0; index < cells.length; index++) {
        let cell = cells[index];

        if (
          targetCell == cell ||
          (targetCell.x === cell.x && targetCell.y === cell.y)
        )
          continue;
        // 判断行;
        // 判断列;
        // 判断宫;
        if (
          targetCell.x === cell.x ||
          targetCell.y === cell.y ||
          targetCell.box === cell.box
        ) {
          //   console.log(num, cell.number);
          if (num === cell.number) {
            return false;
          }
        }
      }
      return true;
    },

    //生成随机号码串
    randomNumbers(arr = [], length = 1) {
      var result = [];
      for (var i = 0; i < length; i++) {
        var ran = Math.floor(Math.random() * (arr.length - i));
        result.push(arr[ran]);
        arr[ran] = arr[arr.length - i - 1];
      }
      return result;
    },

    //按等级生成随机数字
    generateGame(level = 1) {
      let t = 0;
      // let numArr = [];
      let rn = level * 15; //Level 1 隐藏 15个 ；Level 2 隐藏 30个 ；Level 3 隐藏 45个
      //let rn = 3; //Level 1 隐藏 15个 ；Level 2 隐藏 30个 ；Level 3 隐藏 45个
      let rArr = this.randomNumbers(Array.from(new Array(81).keys()), rn);
      // console.log(level);

      // let upset = true;
      let { cells } = this;
      let oi = null;
      for (let i = 0; i < cells.length; i++) {
        //console.log(i);

        const cell = cells[i];
        let nums = this.randomNumbers([1, 2, 3, 4, 5, 6, 7, 8, 9], 9);
        //获取一个随机数并判断这个随机数是否符合要求
        for (let j = 0; j < nums.length; j++) {
          const num = nums[j];
          // 判断行，
          // 判断列，
          // 判断宫，
          if (this.check(cell, num)) {
            cell.number = num;
            cell.disable = true;
            break;
          } else if (j === nums.length - 1) {
            //判断格式尝试数量达到 9 次后重新洗牌再算；
            const RESET_TIME = 9;
            if (oi === i) {
              t++;
            } else {
              oi = i;
              t = 0;
            }
            i = 0;
            //重新洗牌
            if (t % RESET_TIME == 0 && t != 0) {
              i = -1;
              j = 0;
              for (let index = 0; index < cells.length; index++) {
                let cell = cells[index];
                cell.number = null;
              }
              //给一个固定值防止死循环
            } else if (t >= 550) {
              return;
            }
            // console.log(t);
            break;
          }
        }
      }

      for (let index = 0; index < rArr.length; index++) {
        let hideNum = rArr[index];
        const cell = cells[hideNum];
        cell.number = null;
        cell.disable = false;
      }
    },
    getCellControl(x, y) {
      return this.cells.find(item => {
        return item.x === x && item.y === y;
      });
    },
    //写入选中号码
    cellwriteNumber() {
      let { cells, model } = this;
      for (let index = 0; index < cells.length; index++) {
        let cell = cells[index];
        let number =
          model.selectBarNumber === "clear" ? null : model.selectBarNumber;
        cell.number = cell.selected ? number : cell.number;
      }
    },

    showCellSelect(x, y) {
      let { cells } = this;
      for (let index = 0; index < cells.length; index++) {
        let cell = cells[index];
        cell.selected = cell.x == x && cell.y == y ? true : false;
      }
    },
    //显示鼠标经过的方格的当前列和当前行
    showCellsMouseHover(x = null, y = null) {
      let { cells } = this;
      for (let index = 0; index < cells.length; index++) {
        let cell = cells[index];
        cell.hover = cell.x == x || cell.y == y ? true : false;
      }
    },
    getDefectCell() {
      let { cells } = this;
      return cells.reduce((prev, cell) => {
        if (!cell.number) {
          return ++prev;
        }
        return prev;
      }, 0);
    },
    handlCellMouseEvent(cell) {
      if (!this.start) return;
      if (cell) {
        if (this.showMouseHover) {
          let { x, y } = cell;
          this.showCellsMouseHover(x, y);
        }
      } else {
        this.showCellsMouseHover();
      }
    },
    handleCellClickEvent(cell) {
      if (!this.start) return;
      let { x, y } = cell;
      this.showCellSelect(x, y);
      this.$emit(EVENT.CELL_CLICK, cell);
    }
  }
};
</script>

<style lang="scss" scoped>
.board {
  position: absolute;
  left: 0px;
  bottom: 0px;
}
.row {
  display: flex;
}
</style>