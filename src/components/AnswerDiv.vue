<template>
  <div class="row">
    <h5 class="mr-1 mt-2">輸入類型</h5>
    <select v-model="answerDivType" class="form-control col-2 input_type">
      <option selected>文字</option>
      <option>時間</option>
      <option>email</option>
      <option>連結</option>
    </select>
    <input class="col-3 form-control" type="text" disabled="disabled" placeholder="回答" />
  </div>
</template>

<script>
export default {
  name: "answer-div",
  data() {
    return {
      answerDivType: "文字",
    };
  },
  methods:{
     outputAnswerType: function() {
      this.$emit("setAnsDivTypeself",this.answerDivType);
    }
  }
};
</script>

<style>
</style>