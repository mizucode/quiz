  
<template>
  <div id="app">
    <v-app>
      <v-card width="600" class="mx-auto mt-9">
        <transition :duration="{ enter: 500, leave: 300 }" enter-active-class="animated zoomIn" leave-active-class="animated zoomOut" mode="out-in">
          <v-container v-if="questionIndex<quiz.questions.length" :key="questionIndex">
              <v-card-title>
              <h2 class="title mb-2"> {{ quiz.questions[questionIndex].text }} </h2>
              </v-card-title>

              <v-card-text>
                <v-chip-group
                    multiple
                    v-for="(response, index) in quiz.questions[questionIndex].responses" @select="selectOption(index)" :class="userResponses[questionIndex] == index" :key="index"
                    active-class="primary--text">
                      <v-chip>
                      {{ response.text }}
                      </v-chip>
                    </v-chip-group>
              </v-card-text>
              <v-divider></v-divider>
              <v-card-actions>
                <v-icon
                  large
                  @click="prev"
                  :disabled="questionIndex < 1"
                >
                  mdi-chevron-left
                </v-icon>
                <v-spacer></v-spacer>
                <v-icon
                  large
                  @click="next"
                >
                  mdi-chevron-right
                </v-icon>
              </v-card-actions>
          </v-container>
              <div v-if="questionIndex >= quiz.questions.length" v-bind:key="questionIndex" class="quizCompleted has-text-centered">
                   <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title>Results</v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                  <br>
                <v-card-actions>
                  <v-btn
                    class="ma-2"
                    color="primary"
                    @click="restart"
                  >
                    Restart
                    <v-icon
                    large
                  >
                    mdi-cached
                    </v-icon>
                  </v-btn>
            </v-card-actions>

            </div>
        </transition>
      </v-card>
    </v-app>
    </div>
</template>
  
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.js"></script> 
<script>

var quiz = {
  user: "Dave",
  questions: [
  {
    text: "Q1?",
    responses: [
    { text: "1A" },
    { text: "1B" },
    { text: "1C" },
    ] 
  },


  {
    text: "Q2?",
    responses: [
    { text: "2A" },
    { text: "2B" },
    { text: "2C" },
    { text: "2D" }
    ] 
  } ] },

userResponseSkelaton = Array(quiz.questions.length).fill;


export default {
  data: () => ({
    quiz: quiz,
    questionIndex: 0,
    userResponses: userResponseSkelaton,
    isActive: false,
     
  }),

  filters: {
    charIndex: function (i) {
      return String.fromCharCode(97 + i);
    } },

  methods: {
    restart: function(){
        this.questionIndex=0;
         this.userResponses=Array(this.quiz.questions.length).fill(null);
		},
    selectOption: function (index) {
      Vue.set(this.userResponses, this.questionIndex, index);
      console.log(this.userResponses);
    },
    next: function () {
      if (this.questionIndex < this.quiz.questions.length)
      this.questionIndex++;
    },
    prev: function () {
      if (this.quiz.questions.length > 0) this.questionIndex--;
    },
},
}
</script>

