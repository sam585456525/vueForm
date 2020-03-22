<template>
  <div id="app">
    <b-container>
      <!-- Content here -->
      <QuestionBase
        v-for="item in questionList"
        v-bind:title="item.title"
        v-bind:key="item.id"
        v-bind:index="item.id"
        v-on:removeQuestionSelf="removeQuestion"
      ></QuestionBase>
      <b-button @click="newQuestion">新增問題</b-button>
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
    }
  }
};
</script>

<style>
</style>
