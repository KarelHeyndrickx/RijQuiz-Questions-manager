<template>
  <div class="questionlist page">
    <div id="listQuestions">
      <h2 v-if="questions.length == 0">There are no questions.</h2>
      <h2 v-if="questions.length > 0">This is an overview of all the existing questions.</h2>
      <hr>
      <ul class="questions">
        <transition-group
          name="list"
          enter-active-class="animated bounceInUp"
          leave-active-class="animated bounceOutDown"
        >
          <li v-for="(data, index) in questions" :key="index+0">
            <div class="existingQuestionContainer">      
              <div class="existingQuestion">
                <div class="questionHeader">
                  <h4>{{data.text}}</h4>
                </div>
                <div class="questionBody">
                  <h5>Possible answers:</h5>
                  <p>
                    {{data.answers[0]}}
                    <br>
                    {{data.answers[1]}}
                    <br>
                    {{data.answers[2]}}
                  </p>
                  <h5>Correct answer:</h5>
                  <p>{{data.rightAnswer}}</p>
                  <font-awesome-icon
                    icon="trash-alt"
                    class="iconButton"
                    v-on:click="removeQuestion(data._id)"
                  />
                </div>
              </div>
            </div>
          </li>
        </transition-group>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Questions",
  data() {
    return {
      question: {
        _id: "",
        text: "",
        answers: ["", "", ""],
        rightAnswer: ""
      },
      questions: []
    };
  },
  methods: {
    loadQuestions() {},
    addQuestion() {
      this.$validator.validateAll().then(result => {
        //add the correct answer to the answers array as well
        this.question.answers[0] = this.question.rightAnswer;

        if (result) {
          this.questions.push({ question: this.question });
          this.question = {
            _id: "",
            text: "",
            answers: ["", "", ""],
            rightAnswer: ""
          };
        }
      });
    },
    removeQuestion(id) {
      this.questions.splice(id, 1);
      fetch("https://rijquiz-backend.herokuapp.com/api/question/" + id, {
        method: "DELETE"
      }).then(() => {
        alert("deleted");
      });
    }
  },
  created: function() {
    fetch("https://rijquiz-backend.herokuapp.com/api/questions")
      .then(res => res.json())
      .then(data => {
        this.questions = data;
      });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1";
@import "../assets/css/devices.min.css";

h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}
li {
  font-size: 1.3em;
  background-color: white;
  border-radius: 20px;
  margin-bottom: 10px;
  color: #3e5252;
  -webkit-box-shadow: 3px 4px 15px -6px rgba(42, 46, 43, 0.77);
  box-shadow: 3px 4px 15px -6px rgba(42, 46, 43, 0.77);
}
.container {
  box-shadow: 0px 0px 40px lightgray;
}
a {
  color: #42b983;
}
p {
  text-align: center;
  padding: 10px 0;
  color: gray;
}
.alert {
  display: inline-block;
  padding-left: 20px;
  padding-right: 20px;
  text-align: left;
  border-color: #42b983;
  border-left: 10px solid #42b983;
  margin: 10px;
}
.alert-in-enter-active {
  animation: slide-down 0.4s ease;
}
.alert-in-leave-active {
  animation: slide-down 0.4s reverse ease;
}
.questions {
  text-align: left;
}

div.submitbutton {
  text-align: right;
}
#listQuestions {
  width: 70%;
  margin: auto;
}

.existingQuestionContainer h5 {
  margin: 0;
}
.indexNr {
  position: absolute;
  background-color: #2a86bb;
  color: white;
  width: 60px;
  text-align: center;
  border-bottom-right-radius: 50px;
}
.questionHeader{
    background-color: #088755;
    color:white;
}
.questionHeader h4{
padding: 20px;
margin: 0px;
}
.questionBody{
    padding: 20px;
}
.existingQuestion p {
  text-align: left;
  font-size: 0.8em;
  padding: 0px;
}

.iconButton {
  float: right;
  cursor: pointer;
  position: relative;
  top: -10px;
}
</style>
