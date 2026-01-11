<template>



  <div class="container ">
    <div class="row  justify-content-center">
      <img class="col-4 col-sm-12" src="/spims.jpg" style="width: 200px;"  />
      <div class="col-8 col-sm-12 align-self-center  ">


        <div v-if="language==='en'"> 
                  <h5 class="fw-bold" >Examine Yourselves</h5>


        
        <div>Self assessment for week 1: Purpose & priorities</div>
                <br/>    


                <div>Rate 1 (Not true) → 5 (Consistently true)</div>

          
        </div>
        
        <div v-if="language==='ar'" >
                            <h5 class="fw-bold" >انت اي نوع من الخدام ؟</h5>


          <br/>        <div> أجب عن الأسئلة التالية لاكتشاف البرنامج الأنسب لك     </div>
        </div>


      </div>
    </div>


  </div>
  <hr/>



    <div v-if="language_select">
     <div class="language-selector ">
    
      <input class="radioo" type="radio" v-model="language" value="en" style="transform: scale(1.5); margin-right: 6px;"> English 
      <div style="padding: 10px;" ></div>
    
      <input class="radioo" type="radio" v-model="language" value="ar" style="transform: scale(1.5); margin-right: 6px; "> عربي
   


  </div>
             <div v-if="language==='en'"> 
                  <button @click="language_select = false">Next</button>
        </div>
        <div v-else>
                  <button @click="language_select = false">التالى</button>
        </div>




  </div>


  <div v-else>
    
  

  <div class="quiz" :dir="language === 'ar' ? 'rtl' : 'ltr'">
    <!-- Quiz Flow -->
    <div v-if="!showResults">
      <form @submit.prevent="handleNext">
        <div class="question-block">
          <p class="fw-semibold " style="text-align: center">{{ currentQuestionIndex + 1 }}. {{ currentQuestion.text }}</p>
          <ul class="answers">
            <li v-for="(opt, oIndex) in currentQuestion.options" :key="oIndex">
              <label class="container-radio">
                <!-- bind the checked state to answers[currentQuestionIndex] -->
                <input
                    type="radio"
                    :name="'question-' + currentQuestionIndex"
                    v-model="answers[currentQuestionIndex]"
                    :value="opt.category"
                />
                <span :style="language === 'ar' ? 'text-align: left;' : 'text-align: right;'" class="checkmark"></span>
                {{ opt.text }}
              </label>
            </li>
          </ul>
        </div>
        <div v-if="language==='en'"> 
                  <button type="submit">
          {{ currentQuestionIndex === questions.length - 1 ? 'Show Results' : 'Next' }}
        </button>
        </div>
        <div v-else>
                  <button type="submit">
          {{ currentQuestionIndex === questions.length - 1 ? 'أظهر النتائج' : 'التالى' }}
        </button>
        </div>

      </form>
    </div>

    <!-- Results -->
    <div v-else>
      <div v-if="language==='en'"> 

      <div class="score-card">
        <h6>Your Total Score: {{ totalScore }} </h6>
      </div>
      <h3 class="fw-bold">Your Result:</h3>


      </div>

      <div v-else>
        <div class="score-card">
          <h2>درجتك النهائية: {{ totalScore }}</h2>
        </div>
        <h3 class="fw-bold">النتيجة:</h3>
      </div>
      <div v-for="(res, index) in results" :key="index" class="result-block">
        <h3>{{ res.title }}</h3>
        <p>{{ res.description }}</p>
        <div v-if="language==='en'" > 

        <p><strong>Your match:</strong> {{ res.match }}</p>


        </div>

        <div v-if="language==='ar'" > 
          <p><strong>البرنامج الأنسب لك :</strong> {{ res.match }}</p>
        </div>
      </div>
      <hr />
      <div v-if="language==='en'" class="spiritual-caution">
        <h4 class="fw-bold">Spiritual Caution</h4>
        <p>Any score consistently below 3 is an alarm.</p>
        <p>It does not mean you failed.</p>
        <p>It means: Take care now, before it turns into a red flag later.</p>
        <p class="fst-italic">“Light is for revealing, not shaming. Healing begins where truth is spoken.”</p>
      </div>
      <div v-else class="spiritual-caution" dir="rtl">
        <h4 class="fw-bold">تنبيه روحي</h4>
        <p>أي درجة أقل من 3 باستمرار هي ناقوس خطر.</p>
        <p>هذا لا يعني أنك فشلت.</p>
        <p>معناه: انتبه الآن، قبل أن تتحول إلى علامة حمراء لاحقاً.</p>
        <p class="fst-italic">«النور للإعلان، وليس للخزي. يبدأ الشفاء حيث تنطق الحقيقة.»</p>
      </div>

      <div v-if="language==='en'"> 
              <button @click="restartQuiz">Restart</button>

      </div>
      <div v-else>
              <button @click="restartQuiz">أعادة</button>

      </div>
    </div>
  </div>

  </div>
</template>

<script>
export default {
  name: 'ServantQuiz',
  data() {
    return {
      language: 'en',
      language_select: false,
      currentQuestionIndex: 0,
      showResults: false,
      answers: Array(20).fill(null),
      results: [],
      totalScore: 0,
      questions: [],
      questions_en: [
        { text: "Christ is the reference point of my decisions.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "My schedule reflects my spiritual values.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I seek God’s will before major commitments.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I can say no to good things for better ones.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I don’t live in constant hurry or distraction.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "My purpose is clearer than my ambitions.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I measure success by faithfulness, not visibility.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I regularly re-align my priorities in prayer.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I choose eternal impact over instant gratification.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "My life direction brings me closer to Christ.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I care about the salvation and growth of others.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I try to obey God’s commandments even when difficult.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I share Christ through life, not just words.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I serve without seeking recognition.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "My life points others toward Christ.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I accept that God may not reveal my full mission at once.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I trust that my calling unfolds over seasons, not instantly.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I continue walking with God even when the direction feels unclear.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I choose faithfulness in small steps rather than waiting for perfect certainty.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I trust that God guides those who move in humility and prayer.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] }
      ],
      // Mirror English questions for Arabic for now as no translations provided
      questions_ar: [
        { text: "Christ is the reference point of my decisions.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "My schedule reflects my spiritual values.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I seek God’s will before major commitments.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I can say no to good things for better ones.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I don’t live in constant hurry or distraction.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "My purpose is clearer than my ambitions.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I measure success by faithfulness, not visibility.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I regularly re-align my priorities in prayer.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I choose eternal impact over instant gratification.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "My life direction brings me closer to Christ.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I care about the salvation and growth of others.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I try to obey God’s commandments even when difficult.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I share Christ through life, not just words.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I serve without seeking recognition.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "My life points others toward Christ.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I accept that God may not reveal my full mission at once.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I trust that my calling unfolds over seasons, not instantly.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I continue walking with God even when the direction feels unclear.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I choose faithfulness in small steps rather than waiting for perfect certainty.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] },
        { text: "I trust that God guides those who move in humility and prayer.", options: [{text: "1", category: 1}, {text: "2", category: 2}, {text: "3", category: 3}, {text: "4", category: 4}, {text: "5", category: 5}] }
      ]
    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex];
    },
  },
  methods: {
    handleNext() {
      if (this.currentQuestionIndex === this.questions.length - 1) {
        this.calculateResults();
      } else {
        this.currentQuestionIndex++;
      }
    },
    calculateResults() {
      const totalScore = this.answers.reduce((acc, curr) => acc + (parseInt(curr) || 0), 0);
      
      let resultKey = '';
      if (totalScore >= 80) resultKey = 'lifeResult';
      else if (totalScore >= 60) resultKey = 'sincereResult';
      else if (totalScore >= 40) resultKey = 'misalignedResult';
      else resultKey = 'notCentralResult';

      const resultsData = {
        'lifeResult': {
          title: "Life in Christ",
          description: "Your answers indicate that Christ is largely governing your decisions, time, and direction. Your life shows purpose, not just activity. Remain watchful, because spiritual order must be guarded through prayer and regular re-alignment.",
          match: "But seek first the kingdom of God and His righteousness, and all these things shall be added to you. (Matthew 6:33)"
        },
        'sincereResult': {
          title: "Sincere but Not Fully Ordered",
          description: "You desire to live for Christ, yet some areas of life are being shaped by distraction, pressure, or competing priorities. This is a vulnerable place spiritually: drift often begins here quietly. God is calling you to restore clear spiritual order before compromise becomes normal.",
          match: "Nevertheless I have this against you, that you have left your first love. Remember therefore from where you have fallen; repent and do the first works... (Revelation 2:4–5)"
        },
        'misalignedResult': {
          title: "Misaligned Direction",
          description: "Your life may be busy and outwardly committed, but it is not clearly centered on Christ’s will. There is likely fatigue, divided focus, or hidden resistance. This is a call to pause, repent where needed, and bring your priorities back under spiritual obedience.",
          match: "A double-minded man is unstable in all his ways. (James 1:8)"
        },
        'notCentralResult': {
          title: "Christ No Longer Central",
          description: "Something other than Christ is currently directing your life ; fear, comfort, people, or survival. This does not mean you have lost faith, but it does mean your life has lost spiritual order. This is a serious call to confession, repentance, and return to Christ as Lord.",
          match: "For My people have committed two evils: They have forsaken Me, the fountain of living waters, And hewn themselves cisterns... (Jeremiah 2:13)"
        }
      };

      this.totalScore = totalScore;
      this.results = [resultsData[resultKey]];
      this.showResults = true;
    },
    restartQuiz() {
      this.currentQuestionIndex = 0;
      this.answers = Array(this.questions.length).fill(null);
      this.showResults = false;
      this.results = [];
      this.totalScore = 0;
    },
  },
  watch: {
    language(newLang) {
      this.restartQuiz();
      this.questions = newLang === 'en' ? this.questions_en : this.questions_ar;
    }
  },
  created() {
    this.questions = this.questions_en;
  }
};
</script>

<style scoped>
.language-selector {
  text-align: center;
  margin-bottom: 1rem;
}
.quiz {
  max-width: 700px;
  margin: 2rem auto;
  font-family: sans-serif;
}
.question-block {
  margin-bottom: 1.5rem;
  text-align: v-bind("language === 'ar' ? 'right' : 'left'");
}
.answers {
  list-style: none;
  padding: 0;
}
.answers li {
  margin-bottom: 0.5rem;
}
button {
  margin-top: 1.5rem;
  padding: 0.5rem 1rem;
  background-color: #791a1a;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #571212;
}
.result-block {
  margin-bottom: 1.5rem;
}
.score-card {
  background-color: #f8f4f4;
  padding: 1rem;
  border-radius: 12px;
  margin-bottom: 2rem;
  text-align: center;
  border: 2px solid #791a1a;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}
.score-card h6 {
  margin: 0;
  color: #791a1a;
  font-weight: 800;
}

.spiritual-caution {
  background-color: #fdf2f2;
  border-left: 4px solid #791a1a;
  padding: 1.5rem;
  margin: 2rem 0;
  border-radius: 8px;
  text-align: left;
}

[dir="rtl"] .spiritual-caution {
  border-left: none;
  border-right: 4px solid #791a1a;
  text-align: right;
}

.spiritual-caution h4 {
  color: #791a1a;
  margin-top: 0;
  margin-bottom: 1rem;
}

.spiritual-caution p {
  margin-bottom: 0.5rem;
  line-height: 1.6;
}

/* ✅ Responsive Text */
@media (max-width: 768px) {
  .quiz {
    font-size: 16px;
    padding: 1rem;
  }
}
@media (max-width: 480px) {
  .quiz {
    font-size: 15px;
    padding: 0.5rem;
  }
}



.container-radio {
  display: block;
  position: relative;
  margin-bottom: 12px;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  /* Dynamic padding based on language */
  padding-left: v-bind("language === 'ar' ? '0' : '35px'");
  padding-right: v-bind("language === 'ar' ? '35px' : '0'");
}

/* Hide the browser's default radio button */
.container-radio input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

/* Create a custom radio button */
.checkmark {
  position: absolute;
  top: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
  border-radius: 50%;
  /* Dynamic positioning based on language */
  left: v-bind("language === 'ar' ? 'unset' : '0'");
  right: v-bind("language === 'ar' ? '0' : 'unset'");
}

/* On mouse-over, add a grey background color */
.container-radio:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the radio button is checked, add a blue background */
.container-radio input:checked ~ .checkmark {
  background-color: #791a1a;
}

/* Create the indicator (the dot/circle - hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the indicator (dot/circle) when checked */
.container-radio input:checked ~ .checkmark:after {
  display: block;
}

/* Style the indicator (dot/circle) */
.container-radio .checkmark:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}

.radioo{
      accent-color: #791a1a;

}
</style>
