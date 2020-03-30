<template>
  <div
    class="botton"
    @click="handleClick()"
    @mouseover="handleMouseOver()"
    @mouseout="handleMouseOut()"
    :class="[{able:control.disable,hover:control.hover && !control.selected,selected:control.selected},border]"
  >
    <template v-if="control.number">{{control.number}}</template>
    <!-- <template v-else>
      <p class="font-12">{{control&&control.x}}{{control&&control.y}},{{control&&control.box}}</p>
    </template>-->
  </div>
</template>

<script>
import EVENT from "../../event";
export default {
  props: {
    control: {
      type: Object,
      default: function() {
        return {
          number: null,
          hover: false,
          selected: false,
          disable: false,
          box: null,
          x: "-",
          y: "-"
        };
      },
      required: true
    }
  },
  computed: {
    border: function() {
      const { x, y } = this.control;
      return {
        "l-b": y === 0,
        "t-b": x === 0,
        "r-b": y === 8 || y === 2 || y === 5,
        "b-b": x === 8 || x === 2 || x === 5
      };
    }
  },

  data() {
    return {
      empty: true
    };
  },
  methods: {
    handleClick() {
      if (this.control.disable) return;
      this.$emit(EVENT.CELL_CLICK, this.control);
      // console.log(EVENT.CELL_CLICK, this.title, this.position);
    },
    handleMouseOver() {
      if (this.control.disable) return;
      this.$emit(EVENT.CELL_MOUSE_OVER, this.control);
      //console.log(EVENT.CELL_MOUSE_OVER, this.title, this.position);
    },
    handleMouseOut() {
      if (this.control.disable) return;
      this.$emit(EVENT.CELL_MOUSE_OUT);
      // console.log(EVENT.CELL_MOUSE_OUT);
    }
  }
};
</script>

<style  lang="scss" scoped>
$size: 50px;
$background: #dadada;
$borderColor: #6d6d6d;
$borderlightColor: #bebebe;
$backgroundHover: #a5a5a552;
//Selected
$borderselectedcolor: #5ac4d6b2;
$backgroundselected: #52e2fcb2;
.botton {
  // pointer-events: auto;
  line-height: $size;
  width: $size;
  height: $size;
  border: 1px solid $borderlightColor;
  background: $background;
  font-size: 24px;
  font-weight: bold;
  text-align: center;

  color: rgb(26, 26, 26);
  -webkit-user-select: none; /* Chrome/Safari/Opera */

  &.selected {
    border: 1px solid $borderselectedcolor;
    background: $backgroundselected;
  }
  //
  &.hover {
    cursor: pointer;
    // color: rgb(255, 255, 255);
    border: 1px solid $borderlightColor;
    background: $backgroundHover;
  }

  &.able {
    cursor: default;
    color: rgb(128, 128, 128);
    background: #d6d6d6b2;
    border: 1px solid $borderlightColor;
    &.hover {
      // color: rgb(255, 255, 255);
      // border: 1px solid $borderlightColor;
      background: $backgroundHover;
    }
    // &:hover {
    //   border: 1px solid $borderlightColor;
    // }
  }
  &.l-b {
    border-left: 2px solid $borderColor;
  }

  &.t-b {
    border-top: 2px solid $borderColor;
  }

  &.r-b {
    border-right: 2px solid $borderColor;
  }

  &.b-b {
    border-bottom: 2px solid $borderColor;
  }
}
</style>