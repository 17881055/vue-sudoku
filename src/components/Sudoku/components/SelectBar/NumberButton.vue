<template>
    <div class="botton"
         @click="handleClick()"
         :class="{able:disable}">
        <template v-if="title == 'clear'">
            <fa :icon="['fas', 'eraser']" />
        </template>
        <template v-else>{{title}}</template>
    </div>
</template>

<script>
import EVENT from "../../event";
export default {
  props: {
    title: {
      type: [String, Number],
      default: 0,
      required: true
    },
    disable: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    handleClick() {
      if (this.disable) return;
      this.$emit(EVENT.NUMBER_CLICK, this.title);
    }
  }
};
</script>

<style  lang="scss" scoped>
.botton {
  line-height: 50px;
  width: 50px;
  height: 50px;
  border: 1px solid #bbbbbb;
  background: #c4c4c4;
  font-size: 24px;
  font-weight: bold;
  text-align: center;
  cursor: pointer;
  color: rgb(238, 238, 238);
  -webkit-user-select: none; /* Chrome/Safari/Opera */
  &:hover {
    color: rgb(255, 255, 255);
    border: 1px solid #7e7e7e;
    background: #6e6e6e;
  }

  &.able {
    cursor: default;
    color: #b4b4b486;
    background: #d6d6d6b2;
    border: 1px solid #d6d6d6b2;
    &:hover {
      border: 1px solid #d6d6d6b2;
    }
  }
}
</style>