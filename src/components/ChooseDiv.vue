<template>
  <li class="list-group-item option-item mb-1">
    <div class="container">
      <div class="row pl-4">
        <input
          type="text"
          class="form-control col-11 option_item"
          :placeholder="option.title"
          v-model="optionAnswer"
        />
        <button type="button" class="btn btn-outline-primary col-1" v-on:click="removeOption()">X</button>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  props: ["option", "index"],
  name: "choose-div",
  data() {
    return {
      optionAnswer: "",
      placeholderName: "選項" + this.index
    };
  },
  methods: {
    removeOption: function() {
      this.$emit("removeOptionSelf", this.index);
    },
    outputOptionAnswer: function() {
      this.$emit("pushOptionAnswerSelf",this.optionAnswer);
    }
  }
};
</script>

<style>
</style>