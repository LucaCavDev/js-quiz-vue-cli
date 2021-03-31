<template>
  <div class="home container p-5">
    <img 
      alt="Js logo" 
      src="../assets/logo.png" 
      style="height: 250px;" 
    />
    <div class="container-quiz">

      <div class="quiz-header p-3">
        <h1>JS Quiz</h1>
      </div>

      <div 
        id="grad1"
        class="step-progress"
        :style="{'width': progress + '%'}"
      ></div>

      <div 
        class="quiz-main py-2" 
        v-for="(element,index) in questions.slice(a,b)" 
        :key="index"
      >

        <div class="box-question col-md-8 offset-md-2">
          <h2 class="p-2 ">Question {{b}}/{{ questions.length }}</h2>
          <!-- <p>{{ element.question }}</p> -->
          <p class=" p-2">
            <span v-html="element.question"></span>
          </p>
        </div>

        <div class="box-suggestions col-lg-6 offset-lg-3 ">

          <ul >
            <h4>Pick One!</h4>
            <li
              v-for="(item,index) in element.suggestions"
              :key="index"
              :class="select ? check(item) : ''"
              @click="selectResponse(item)"
              v-show="quiz"
              class=" p-2 my-2"
            >
              {{ item.suggestion }}

              <div 
                class="fa fa-check"
                v-if="select ? item.correct: ''"
              ></div>
              <div 
                class="fa fa-times"
                v-if="select ? !item.correct: ''"
              ></div>

            </li>
          </ul>

        </div>

      </div>

      <div class="box-score mt-5" v-if="score_show">
        <h2>Your score is:</h2>
        <h2>{{score}}/{{ questions.length }}</h2>
        <button 
          @click="restartQuiz"
          class="btn"

        >
          Restart
          <i class="fa fa-refresh"></i>
        </button>

      </div>
      <div class="quiz-footer pt-3" v-if="progress < 100">
        <button 
          @click="skipQuestion"
          :style="!next ? 'background-color:#f5de19' : ''"
          class="btn"
        >
        Skip
        </button>
        <button 
          @click="nextQuestion"
          :style="next ? 'background-color:#f5de19' : ''"
          class="btn"
        >
        Next
        </button>
      </div>

    </div>

  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  data(){
    return{
      questions:[
        {
          question:'JavaScript is a ___ -side programming language.',
          suggestions:[
            {suggestion:'Client'},
            {suggestion:'Server'},
            {suggestion:'Both',correct:true},
            {suggestion:'None'},
          ]
        },
        {
          question:'If you type the following code in the console window, what result will you get? <br> 3 > 2 > 1 === false;',
          suggestions:[
            {suggestion:'true',correct:true},
            {suggestion:'false'},
          ]
        },
        {
          question:'Which of the following will write the message “Hello DataFlair!” in an alert box?',
          suggestions:[
            {suggestion:'alertBox(“Hello DataFlair!”);'},
            {suggestion:'alert(Hello DataFlair!);'},
            {suggestion:'msgAlert(“Hello DataFlair!”);'},
            {suggestion:'alert(“Hello DataFlair!”);',correct:true},
          ]
        },
        {
          question:'How do you find the minimum of x and y using JavaScript?',
          suggestions:[
            {suggestion:'min(x,y);'},
            {suggestion:'Math.min(x,y)',correct:true},
            {suggestion:'Math.min(xy)'},
            {suggestion:'min(xy);'},
          ]
        },
        {
          question:'If the value of x is 40, then what is the output of the following program? <br> "(x % 10 == 0)? console.log(“Divisible by 10”) : console.log(“Not divisible by 10”);"',
          suggestions:[
            {suggestion:'ReferenceError'},
            {suggestion:'Divisible by 10',correct:true},
            {suggestion:'Not divisible by 10'},
            {suggestion:'None of the above'},
          ]
        },
        {
          question:'Which JavaScript label catches all the values, except for the ones specified?',
          suggestions:[
            {suggestion:'catch'},
            {suggestion:'default',correct:true},
            {suggestion:'label'},
            {suggestion:'try'},
          ]
        },
        {
          question:'Which are the correct “if” statements to execute certain code if “x” is equal to 2?',
          suggestions:[
            {suggestion:'if(x 2)'},
            {suggestion:'if(x == 2)',correct:true},
            {suggestion:'if(x = 2)'},
            {suggestion:'if(x != 2 )'},
          ]
        },
        {
          question:'What will the code return? <br> Boolean(3 < 7)',
          suggestions:[
            {suggestion:'false'},
            {suggestion:'true',correct:true},
            {suggestion:'NaN'},
            {suggestion:'SyntaxError'},
          ]
        },
        {
          question:'What is the output of the following code in the console? <br> var x = 0; <br> function fun(){ <br> ++x; <br> this.x = x; <br> return x; <br> } <br> var bar = new new fun; <br> console.log(bar.x);',
          suggestions:[
            {suggestion:'ReferenceError'},
            {suggestion:'TypeError',correct:true},
            {suggestion:'undefined'},
            {suggestion:'1'},
          ]
        },
        {
          question:'Which is the correct JavaScript syntax to change the HTML content given below? <br> Hello World!',
          suggestions:[
            {suggestion:'document.getElementById(“test”).innerHTML = “Hello DataFlair!”;',correct:true},
            {suggestion:'document.getElementsById(“test”).innerHTML = “Hello DataFlair!”;'},
            {suggestion:'document.getElementById(test).innerHTML = “Hello DataFlair!”;'},
            {suggestion:'document.getElementByTagName(“p”)[0].innerHTML = “Hello DataFlair!”;'},
          ]
        },
        {
          question:'Is there a difference between Java and Javascript?',
          suggestions:[
            {suggestion:'No'},
            {suggestion:'Yes',correct:true},
            {suggestion:'NaN'},
            {suggestion:'Undefined'},
          ]
        },
      ],

      a:0,
      b:1,
      select:false,
      score:0,
      quiz:true,
      score_show:false,
      next:false,
      progress:0,
    }
  },
  methods:{
    selectResponse(e){

      this.select = true;
      this.next = true;

      if (e.correct) {
        this.score++;
      }

    },

    check(status){
      if (status.correct){
        return 'correct'
      } else {
        return 'incorrect'
      }
    },

    nextQuestion(){

      if (!this.next) {
        return;
      }

      this.progress = this.progress + (100/this.questions.length);

      if (this.questions.length -1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
        this.select = false;
        this.next = false;
      }
    },

    skipQuestion(){

      if (this.next) {
        return;
      }

      this.progress = this.progress + (100/this.questions.length);



      if (this.questions.length -1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
      }
    },

    restartQuiz(){

      Object.assign(this.$data, this.$options.data()); 
    }
  }
};
</script>
