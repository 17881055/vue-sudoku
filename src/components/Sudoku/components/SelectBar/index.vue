<template>
  <div class="wrap">
    <template v-for="(item, index) in 9">
      <NumberButton
        @eventNumberClick="handleButtonClick"
        :title="index+1"
        :key="index"
        :disable="!start"
      />
    </template>
    <NumberButton @eventNumberClick="handleButtonClick" :title="'clear'" :disable="!start" />
  </div>
</template>

<script>
import NumberButton from "./NumberButton";
import EVENT from "../../event";
export default {
  components: {
    NumberButton
  },
  data() {
    return {};
  },
  props: {
    start: {
      type: Boolean,
      default: false,
      required: true
    }
  },
  methods: {
    handleButtonClick(number) {
      //   console.log(EVENT.NUMBER_CLICK, number);
      this.$emit(EVENT.NUMBER_CLICK, number);
    }
  }
};
</script>

<style  lang="scss" scoped>
.wrap {
  display: flex;
  flex-wrap: wrap;
}
</style>