<template>
  <li id="vueTestId0" class="list-group-item pt-0 mb-4 px-0">
    <div class="col-12 text-center pb-3 mb-3 questionHandler" style="background:rgba(0,0,0,.125)"></div>
    <div class="form-group question_content col-11 mb-1 mx-auto">
      <div class="container">
        <div class="row">
          <input
            v-model="questionTittle"
            type="text"
            class="form-control col-8 question_tittle"
            :placeholder="title"
          />
          <select v-model="questionType" class="form-control col-2 question_type">
            <option>{{questionTypeText.ans}}</option>
            <option>{{questionTypeText.ltext}}</option>
            <option selected>{{questionTypeText.choose}}</option>
            <option>{{questionTypeText.check}}</option>
            <option>{{questionTypeText.list}}</option>
          </select>
          <div class="col-1">
            <!--空格用-->
          </div>
          <b-button type="button" class="btn btn-bee-one col-1" v-on:click="removeQuestion()">X</b-button>
          <div class="row col-12">
            <input v-model="questionMust" class="form-check-input must ml-5 mt-1" type="checkbox" value />
            <p>必填</p>
          </div>
        </div>
      </div>
      <div class="question_div container-fluid">
        <!--簡答和詳答類v-if-->
        <div v-if="questionTypeDecide_ansDiv">
          <answer-div @setAnsDivTypeself="setAnsDivType"></answer-div>
        </div>

        <!--選項類v-if-->
        <div v-if="questionTypeDecide_chooseDiv">
          <ul class="list-group option_list">
            <!-- <choose-div></choose-div> -->
            <choose-div
              v-for="option in optionList"
              v-bind:option="option"
              v-bind:key="option.id"
              v-bind:index="option.id"
              @removeOptionSelf="removeOption"
              @pushOptionAnswerSelf="pushOptionAnswer"
            ></choose-div>
          </ul>
          <b-button type="button" class="btn btn-bee-two new_option mt-3" @click="newOption">新增選項</b-button>
        </div>
      </div>
    </div>
  </li>
</template>

<script>
const questionTypeText = {
  ans: "單行回答",
  ltext: "多行回答",
  choose: "單選",
  check: "多選",
  list: "下拉式選單"
};
import ChooseDiv from "./ChooseDiv.vue";
import AnswerDiv from "./AnswerDiv.vue";

export default {
  props: ["title", "index"],
  components: {
    ChooseDiv,
    AnswerDiv
  },
  name: "QuestionBase",
  data() {
    return {
      questionTittle: "",
      questionType: questionTypeText.choose,
      questionTypeText: questionTypeText, //讀取全域資料
      chooseOptionNum: 1,
      optionItemNum: 0,
      optionList: [],
      optionAnswerList: [],
      ansDivType: "",
      questionMust:false,
    };
  },
  computed: {
    //決定ansDiv是否顯示
    questionTypeDecide_ansDiv: function() {
      let optionQuestion = [
        this.questionTypeText.ans,
        this.questionTypeText.ltext
      ];
      return optionQuestion.indexOf(this.questionType) != -1;
    },
    //決定chooseDiv是否顯示
    questionTypeDecide_chooseDiv: function() {
      let optionQuestion = [
        this.questionTypeText.choose,
        this.questionTypeText.check,
        this.questionTypeText.list
      ];
      return optionQuestion.indexOf(this.questionType) != -1;
    }
  },
  methods: {
    //移除此問題
    removeQuestion: function() {
      console.log(this.key);
      this.$emit("removeQuestionSelf", this.index);
    },
    //移除指定選項
    removeOption: function(index) {
      var newIndex = "";
      var list = this.optionList;
      list.forEach(function(item, key) {
        if (item.id == index) {
          newIndex = key;
          list.splice(newIndex, 1);
        }
      });
    },
    //新增選項
    newOption: function() {
      this.optionItemNum++;
      this.optionList.push({
        id: this.optionItemNum,
        content: ``,
        title: `選項內容${this.optionItemNum}`
      });
    },
    //輸出此問題的設定
    outputAnswer: function() {
      let outputAnswerObj = {};
      outputAnswerObj.title = this.questionTittle;
      outputAnswerObj.type = this.questionType;
      outputAnswerObj.must = this.questionMust;
      //判斷問題類型
      if (this.questionTypeDecide_chooseDiv == true) {
        this.optionAnswerList = [];
        for (var i = 0; i < this.$children.length; i++) {
          this.$children[i].outputOptionAnswer();
        }
        outputAnswerObj.answer = this.optionAnswerList;
      } else if (this.questionTypeDecide_ansDiv == true) {
        this.$children[0].outputAnswerType();
        outputAnswerObj.answer = this.ansDivType;
      }

      return outputAnswerObj;
    },
    /*********
     * 接受子組件傳輸答案呼叫
     * **********/
    pushOptionAnswer: function(optionAnswer) {
      this.optionAnswerList.push(optionAnswer);
    },
    setAnsDivType: function(answerDivType) {
      this.ansDivType = answerDivType;
    }
  }
};
</script>

<style>
</style>