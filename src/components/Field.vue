<template>
  <div class="emojiarea">
    <p class="emojiarea__field" ref="editable" contenteditable="true">{{ inputValue }}</p>
    <picker class="emojiarea__picker" />
  </div>
</template>

<script>
import Picker from "./Picker.vue";

export default {
  components: {
    Picker
  },
  data: function() {
    return {
      inputValue:
        "text text text text Lorem ipsum dolor sit amet consectetur adipisicing elit. Inventore nesciunt, nihil, impedit consequatur nam dolor sit, veritatis ullam voluptate placeat cum harum rerum. Delectus accusamus ea, obcaecati, eum cum, ex dolore deserunt architecto aliquid libero accusantium asperiores nulla vero? Quam, cum facere in quaerat dicta totam sapiente, adipisci earum quas consectetur tempore architecto, culpa blanditiis praesentium consequatur cupiditate vitae eius laborum dolorum. Laborum qui saepe laboriosam tempore earum quidem commodi, unde rerum vero dicta soluta accusantium nobis amet voluptatem. Aliquid enim nostrum officia? Odit placeat eos quas nostrum tempore, voluptatem voluptates ipsum earum similique molestiae et autem sequi soluta? Voluptas."
    };
  },
  methods: {
    onInput(e) {
      this.inputValue = e.target.innerHTML;
    }
  }
};
</script>
<style scope>
.emojiarea {
  display: block;
  position: relative;
  width: 100%;
  height: auto;
  padding: 0;
  font-size: 14px;
}
.emojiarea__field {
  overflow: auto;
  display: block;
  height: auto;
  min-height: 8em;
  max-height: 15em;
  padding: 6px 24px 6px 12px;
  border: 1px solid #ccc;
  border-radius: 3px;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  margin: 0;
  font-size: inherit;
  line-height: 1.42857143;
  color: #555;
  text-align: left;
  cursor: text;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}
.emojiarea__field:focus {
  outline: 0;
  border-color: #66afe9;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(102, 175, 233, 0.6);
}
.emojiarea__field span {
  top: 0;
}
.emojiarea__picker {
  z-index: 5;
  position: absolute;
  right: 3px;
  top: 3px;
}
</style>
