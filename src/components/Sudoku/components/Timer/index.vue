 <template>
  <div class="timer">
    <div ref="startTimer"></div>
  </div>
</template>

<script>
export default {
  name: "Timer",
  data() {
    return {
      timer: "",
      content: "",
      hour: 0,
      minutes: 0,
      seconds: 0,
      millisecond: 0
    };
  },
  props: {
    result: {
      type: Object,
      default: function() {
        return { total: 0, empty: 0, error: null, success: null };
      },
      required: false
    },
    start: {
      type: Boolean,
      default: false,
      required: true
    }
  },
  watch: {
    result: {
      handler: function(newValue) {
        if (newValue.success) {
          //暂停
          this.pause();
        } else {
          //开始
          this.action();
        }
      },
      deep: true
    },
    start(val) {
      if (val == true) {
        this.action();
      } else {
        this.reset();
      }
    }
  },
  created() {},
  destroyed() {
    this.reset();
  },

  methods: {
    action() {
      if (this.timer) {
        return;
      }
      this.timer = setInterval(this.startTimer, 10);
    },
    pause() {
      clearInterval(this.timer);
      this.timer = null;
    },
    reset() {
      clearInterval(this.timer);
      this.timer = null;
      this.$refs.startTimer.innerHTML = null;
      this.hour = 0;
      this.minutes = 0;
      this.seconds = 0;
      this.millisecond = 0;
    },
    startTimer() {
      this.millisecond += 1;
      if (this.millisecond >= 100) {
        this.millisecond = 0;
        this.seconds = this.seconds + 1;
      }

      if (this.seconds >= 60) {
        this.seconds = 0;
        this.minutes = this.minutes + 1;
      }

      if (this.minute >= 60) {
        this.minutes = 0;
        this.hour = this.hour + 1;
      }
      this.$refs.startTimer.innerHTML =
        (this.hour < 10 ? "0" + this.hour : this.hour) +
        ":" +
        (this.minutes < 10 ? "0" + this.minutes : this.minutes) +
        ":" +
        (this.seconds < 10 ? "0" + this.seconds : this.seconds) +
        "." +
        (this.millisecond < 10 ? "0" + this.millisecond : this.millisecond) +
        parseInt(Math.random() * 10);
    }
  }
};
</script>
<style lang="scss" scoped>
</style>