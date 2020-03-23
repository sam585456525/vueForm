<template>
  <div id="app">
    <b-container class="mt-4">
      <!-- Content here -->
      <QuestionBase
        v-for="item in questionList"
        v-bind:title="item.title"
        v-bind:key="item.id"
        v-bind:index="item.id"
        v-on:removeQuestionSelf="removeQuestion"
      ></QuestionBase>
      <b-button @click="newQuestion">新增問題</b-button>
      <p></p>
      <b-button @click="outputAllResult">outputAllResult</b-button>
    </b-container>
  </div>
</template>

<script>
import QuestionBase from "./components/QuestionBase.vue";

export default {
  name: "App",
  components: {
    QuestionBase
  },
  data() {
    return {
      questionNum: 0,
      questionList: []
    };
  },
  methods: {
    newQuestion: function() {
      this.questionNum++;
      this.questionList.push({
        id: this.questionNum,
        title: `問題${this.questionNum}`
        // title:123,
      });
    },
    removeQuestion: function(index) {
      console.log(12);
      var newIndex = "";
      var list = this.questionList;
      list.forEach(function(item, key) {
        if (item.id == index) {
          newIndex = key;
          list.splice(newIndex, 1);
        }
      });
    },
    outputAllResult: function() {
      console.log("out put all result");
      let outputResultArr = [];
      for (var i = 0; i < this.$children.length; i++) {
        outputResultArr.push(this.$children[i].outputAnswer());
      }
      console.log(outputResultArr);
    }
  }
};
</script>

<style>
</style>
