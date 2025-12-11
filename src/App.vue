<template>



  <div class="container ">
    <div class="row  justify-content-center">
      <img class="col-4 col-sm-12" src="/spims.jpg" style="width: 200px;"  />
      <div class="col-8 col-sm-12 align-self-center  ">


        <div v-if="language==='en'"> 
                  <h5 class="fw-bold" >What Kind of Servant Are You?</h5>


        <br/>        <div>Answer these questions to discover your perfect SPIMS match!</div>

          
        </div>
        
        <div v-if="language==='ar'" >
                            <h5 class="fw-bold" >ما نوع الخادم الذي أنت ؟</h5>


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

      <h2>Your Result<span v-if="results.length > 1">s</span>:</h2>


      </div>

      <div v-else>
        <h2>النتائج <span v-if="results.length > 1">s</span>:</h2>
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
        <hr v-if="index !== results.length - 1" />
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
      language_select: true,
      currentQuestionIndex: 0,
      showResults: false,
      answers: Array(8).fill(null),
      categoryCount: { A: 0, B: 0, C: 0, D: 0 , E: 0 },
      results: [],
      questions: [],
      questions_en: [
    {
        "text": "What motivates you to study theology or Christian education?",
        "options": [
            { "text": "A) A desire to know Christ deeply and live Him, not just learn about Him academically.", "category": "A" },
            { "text": "B) I want depth and structure, but I need something concise and realistically paced.", "category": "B" },
            { "text": "C) I want the Bible to be the lens through which I understand my relationship with God.", "category": "C" },
            { "text": "D) I want a broad foundation across theology, apologetics, patristics, history, and worship.", "category": "D" },
            { "text": "E) I want to think deeply, reflect, and gain answers to the big questions that shape faith and life.", "category": "E" }
        ]
    },
    {
        "text": "Which statement best describes the way you like to teach or serve?",
        "options": [
            { "text": "A) I love to teach in a way that is simple yet profound — making Christ alive in every lesson.", "category": "A" },
            { "text": "B) I prefer structured content that I can manage with a busy schedule while still teaching meaningfully.", "category": "B" },
            { "text": "C) I focus on explaining the Word of God holistically, making Scripture relevant to modern life.", "category": "C" },
            { "text": "D) I want to communicate Christian education clearly — especially to people who need English.", "category": "D" },
            { "text": "E) I love guiding others through tough questions, helping them understand faith.", "category": "E" }
        ]
    },
    {
        "text": "In what language do you prefer to learn?",
        "options": [
            { "text": "A) I want an in-depth learning experience either in English or in Arabic or in both languages.", "category": "A" },
            { "text": "B) Arabic only is perfect for me.", "category": "B" },
            { "text": "C) Arabic-focused scriptural studies suit me best.", "category": "C" },
            { "text": "D) I prefer English that does not feature complex Christian terminology.", "category": "D" },
            { "text": "E) I want English content that builds depth in understanding.", "category": "E" }
        ]
    },
    {
        "text": "Which describes your stage in ministry right now?",
        "options": [
            { "text": "A) I’ve been serving for a while, and I want to deepen my knowledge of varying topics such as theology, worship, and church life.", "category": "A" },
            { "text": "B) I serve actively but my life is busy; I need a reasonable academic load.", "category": "B" },
            { "text": "C) I teach the Bible or want to teach it with more depth, relevance, and spirituality.", "category": "C" },
            { "text": "D) I’m new to teaching, serving those new to Christianity, or involved in English-based ministry.", "category": "D" },
            { "text": "E) I am someone who people come to with questions — I want deeper answers and tools for people who are struggling with difficult intellectual questions.", "category": "E" }
        ]
    },
    {
        "text": "When it comes to serving, what kind of impact do you want to have on others?",
        "options": [
            { "text": "A) I want people to encounter Christ through deep, simple, meaningful teaching.", "category": "A" },
            { "text": "B) I want to help others spiritually but with steady, balanced study that fits real life.", "category": "B" },
            { "text": "C) I want to illuminate Scripture so people understand God’s Word holistically.", "category": "C" },
            { "text": "D) I want to reach diverse audiences — especially those new to Christianity.", "category": "D" },
            { "text": "E) I want to help others navigate doubts, questions, and life challenges through wisdom and deep understanding.", "category": "E" }
        ]
    },
    {
        "text": "What spiritual challenge are you trying to overcome?",
        "options": [
            { "text": "A) Moving from intellectual knowledge to living theology — I want transformation, not information.", "category": "A" },
            { "text": "B) I want to study Christian subjects in-depth but I am afraid of long commitments or heavy workloads.", "category": "B" },
            { "text": "C) I want to understand the Bible beyond fragmented verses — fully, spiritually, historically.", "category": "C" },
            { "text": "D) I am seeking Christian education that is accessible and fast-paced in order to serve better.", "category": "D" },
            { "text": "E) I want to strengthen my ability to comprehend, explain, defend, and reflect on faith in a world full of competing ideas.", "category": "E" }
        ]
    },
    {
        "text": "What do you hope to gain from your next step in Christian education?",
        "options": [
            { "text": "A) A deep, holistic spiritual transformation that shapes my identity as a servant.", "category": "A" },
            { "text": "B) Academic depth that is efficient, accessible, and manageable.", "category": "B" },
            { "text": "C) A solid, spiritual, contextual understanding of Scripture from Genesis to Revelation.", "category": "C" },
            { "text": "D) A broad and simple foundation across all Christian subjects in clear English.", "category": "D" },
            { "text": "E) A deeper intellectual and reflective foundation — answers, wisdom, and clarity to guide myself and others.", "category": "E" }
        ]
    }
],
questions_ar: [
    {
        "text": "ما الذي يدفعك لدراسة اللاهوت أو التربية المسيحية؟",
        "options": [
            { "text": "أ) الرغبة في معرفة المسيح بعمق والاتحاد به، ليس مجرد التعرّف عليه معرفة أكاديمية.", "category": "A" },
            { "text": "ب) أبحث عن العمق والتنظيم، لكنني أحتاج إلى برنامج مختصر ومتوازن يتناسب مع وقتي.", "category": "B" },
            { "text": "ج) أريد أن يكون الكتاب المقدّس هو العدسة التي أفهم من خلالها علاقتي بالله.", "category": "C" },
            { "text": "د) أطمح إلى تكوين شامل في اللاهوت، والدفاعيات، والآباء، والتاريخ الكنسي، والعبادة.", "category": "D" },
            { "text": "هـ) أحب التفكير العميق والتأمل، وأسعى لإجابات عن الأسئلة الكبرى التي تشكّل الإيمان والحياة.", "category": "E" }
        ]
    },
    {
        "text": "أي من هذه العبارات تصف أسلوبك في التعليم أو الخدمة بشكل أدق؟",
        "options": [
            { "text": "أ) أحب أن أعلّم بطريقة بسيطة لكن عميقة، تجعل حضور المسيح حيًّا في كل درس.", "category": "A" },
            { "text": "ب) أفضل المحتوى المنظّم الذي يمكنني الالتزام به رغم انشغالاتي، مع الحفاظ على عمق المعنى.", "category": "B" },
            { "text": "ج) أركّز على شرح كلمة الله بصورة كلية، وربط الكتاب المقدّس بحياة الإنسان المعاصرة.", "category": "C" },
            { "text": "د) أسعى إلى نقل التعليم المسيحي بوضوح، خاصة لمن يحتاجون إلى الدراسة باللغة الإنجليزية.", "category": "D" },
            { "text": "هـ) أستمتع بارشاد الآخرين في أسئلتهم الصعبة ومساعدتهم على فهم الإيمان بعمق ووعي.", "category": "E" }
        ]
    },
    {
        "text": "ما اللغة التي تفضّل أن تتعلّم بها؟",
        "options": [
            { "text": "أ) أبحث عن تجربة تعليمية عميقة باللغـة العربية أو الإنجليزية أو كلتيهما.", "category": "A" },
            { "text": "ب) اللغة العربية وحدها مناسبة تمامًا لي.", "category": "B" },
            { "text": "ج) أفضل دراسة كتابية متخصصة باللغة العربية.", "category": "C" },
            { "text": "د) أفضل اللغة الإنجليزية بشرط أن تكون المصطلحات اللاهوتية مبسطة وغير معقّدة.", "category": "D" },
            { "text": "هـ) أفضّل محتوى باللغة الإنجليزية يساعدني على التفكير العميق وبناء فهم متقدّم.", "category": "E" }
        ]
    },
    {
        "text": "أي وصف يعبّر عن مرحلتك الحالية في الخدمة؟",
        "options": [
            { "text": "أ) أخدم منذ فترة، وأرغب في تعميق معرفتي في مجالات متعددة مثل اللاهوت، والعبادة، والحياة الكنسية.", "category": "A" },
            { "text": "ب) أخدم بانتظام، لكنني مشغول وأحتاج إلى دراسة معقولة ومتوازنة.", "category": "B" },
            { "text": "ج) أعلّم الكتاب المقدّس أو أرغب في تعليمه بعمق وروحانية وارتباط بالحياة.", "category": "C" },
            { "text": "د) أنا جديد في التعليم، أخدم حديثي الإيمان، أو أشارك في خدمة تعتمد على اللغة الإنجليزية.", "category": "D" },
            { "text": "هـ) يلجأ إليَّ الناس بأسئلتهم، وأحتاج إلى أدوات وإجابات أعمق للأسئلة الفكرية المعقّدة.", "category": "E" }
        ]
    },
    {
        "text": "ما نوع التأثير الذي تتمنى أن تتركه خدمتك في الآخرين؟",
        "options": [
            { "text": "أ) أن يلتقي الناس بالمسيح من خلال تعليم عميق، بسيط، وذو معنى.", "category": "A" },
            { "text": "ب) أن أساند الآخرين روحيًا بدراسة متدرّجة ومتوازنة بشكل ينسجم مع واقع الحياة.", "category": "B" },
            { "text": "ج) أن أساعد الناس على فهم كلمة الله بصورة شاملة ومتكاملة.", "category": "C" },
            { "text": "د) أن أصل إلى فئات متنوعة، خاصة الجدد في الإيمان المسيحي.", "category": "D" },
            { "text": "هـ) أن أرشد الآخرين في شكوكهم وتساؤلاتهم وتحدياتهم الحياتية بحكمة وفهم عميق.", "category": "E" }
        ]
    },
    {
        "text": "ما التحدي الروحي الذي تسعى لتجاوزه حاليًا؟",
        "options": [
            { "text": "أ) الانتقال من المعرفة الذهنية إلى اللاهوت المعاش؛ أبحث عن التحوّل ليس مجرد المعلومات.", "category": "A" },
            { "text": "ب) أريد دراسة معمّقة لكنني متخوّف من الالتزامات الطويلة أو الأعباء الثقيلة.", "category": "B" },
            { "text": "ج) أطمح لفهم الكتاب المقدّس بشكل متكامل، روحي وتاريخي، ليس مجرّد دراسة لآيات متفرقة.", "category": "C" },
            { "text": "د) أبحث عن تعليم مسيحي سهل الوصول إليه وسريع الإيقاع ليساعدني على الخدمة بفاعلية.", "category": "D" },
            { "text": "هـ) أسعى إلى تعزيز قدرتي على فهم الإيمان وشرحه والدفاع عنه والتأمل فيه وسط تعدّد الأفكار والرؤى.", "category": "E" }
        ]
    },
    {
        "text": "ماذا تأمل أن تكسب من خطوتك القادمة في التعليم المسيحي؟",
        "options": [
            { "text": "أ) تحوّلًا روحيًا عميقًا وشاملًا يشكّل هويتي كخادم.", "category": "A" },
            { "text": "ب) عمقًا أكاديميًا فعّالًا، مرنًا، وقابلًا للتطبيق.", "category": "B" },
            { "text": "ج) فهمًا راسخًا، روحيًا وسياقيًا، للكتاب المقدّس من سفرالتكوين إلى سفر الرؤيا.", "category": "C" },
            { "text": "د) أساسًا واسعًا وبسيطًا في مختلف مجالات التعليم المسيحي بلغة إنجليزية واضحة.", "category": "D" },
            { "text": "هـ) أساسًا فكريًا وتأمليًا أعمق يزوّدني بالحكمة والوضوح لإرشاد نفسي والآخرين.", "category": "E" }
        ]
    }
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
      this.categoryCount = { A: 0, B: 0, C: 0, D: 0, E: 0 };
      this.answers.forEach(cat => {
        if (cat) this.categoryCount[cat]++;
      });

      const max = Math.max(...Object.values(this.categoryCount));
      const topCategories = Object.keys(this.categoryCount).filter(
          key => this.categoryCount[key] === max
      );

      this.setResults(topCategories);
      this.showResults = true;
    },
    setResults(categories) {
      const profiles = {
        en: {
    "A": {
        "title": "The Seeker of Transformation",
        "description": "You are drawn to depth, transformation, and spiritual maturity. You don’t want dry knowledge — you want a living Christ at the center of theology, history, worship, and ministry. You teach with simplicity and meaning, and people naturally feel Christ in your words. You attract believers and non-believers alike because your depth is spiritual, not academic.",
        "match": "Coptic DOME Diploma — a 2-year bilingual formation for mature, deep, transformational servants."
    },
    "B": {
        "title": "The Steady Servant",
        "description": "You want real theological and spiritual depth, but in a format that respects your time and responsibilities. You love academic structure, but you need a shorter track that remains rich without being overwhelming. You are committed, wise, and realistic — seeking growth that fits your rhythm of life.",
        "match": "Make Him Known Certificate — academic, deep, manageable, and perfectly paced for busy servants."
    },
    "C": {
        "title": "The Disciple of Scripture",
        "description": "Your heart beats for the Bible. You want to understand God through His Word — not verse by verse, but as one living story. You teach Scripture in a way that is holistic, spiritual, and relevant — exactly what today’s generation needs. Whether you are a Sunday School teacher or a seeker of Biblical depth, this path forms your relationship with God through Scripture itself.",
        "match": "Orthodox Scriptures Diploma — a 2-year Arabic program dedicated entirely to Scripture."
    },
    "D": {
        "title": "The Practical Servant",
        "description": "You want depth that is fast, clear, and accessible. You are either busy, serving those who are new to Christianity, involved in English-based ministry, or seeking learning in simple English Christian terms. You prefer a lighter workload but meaningful content. You value practicality over heavy academia.",
        "match": "Catechesis Certificate — the simplest, most inclusive English program touching all main Christian education topics."
    },
    "E": {
        "title": "The Reflective Servant",
        "description": "You are someone who loves to reflect, question, analyze, and understand. You think deeply about God, truth, purpose, and the world around you. People come to you with their hardest questions because they feel you listen, process, and explain with clarity. You’re not afraid of complexity — you move toward it. Your heart is pastoral, intellectual, and thoughtful. You may feel called to defend your faith, counsel others, explore worldview topics, learn apologetics and understand culture and meaning to strengthen your Christian faith.",
        "match": "Walk in Wisdom Certificate — a 1-year, English, customizable program in apologetics, philosophy, worldviews, counseling, and mission theology — designed for thinkers, reflectors, and question-askers."
    }
},
ar: {
    "A": {
        "title": "الساعي للتحوّل الحقيقي",
        "description": "أنت تنجذب إلى العمق، والتحوّل، والنضج الروحي. لا تسعى إلى معرفة جافة، بل إلى المسيح الحي في قلب اللاهوت، والتاريخ، والعبادة، والخدمة. تعليمك بسيط لكنه ممتلئ بالمعنى، ويشعر الناس بحضور المسيح في كلماتك. عمقك روحي بالأساس، ولذلك يلمس كل من حولك.",
        "match": "دبلومة Coptic DOME — برنامج تكويني متاح باللغة الانجليزية و العربية لمدة عامين، موجّه لخدّام يسعون إلى عمق روحي وتحول حقيقي."
    },
    "B": {
        "title": "الخادم الثابت",
        "description": "تبحث عن عمق لاهوتي وروحي حقيقي، لكن ضمن إطار يحترم وقتك ومسؤولياتك. تحب الدراسة الأكاديمية، لكنك تحتاج إلى مسار أقصر غني بالمضمون بدون إثقال. أنت ملتزم، حكيم، وواقعي، وتبحث عن نمو يتماشى مع إيقاع حياتك.",
        "match": "شهادة Make Him Known — برنامج أكاديمي عميق ومتوازن، مصمَّم خصيصًا للخدّام المشغولين."
    },
    "C": {
        "title": "تلميذ الكلمة",
        "description": "قلبك نابض بمحبة الكتاب المقدّس. تريد أن تفهم الله من خلال كلمته كقصة حيّة واحدة، لا كآيات منفصلة. أسلوبك في تعليم الكتاب المقدّس كُلّي، روحي، ومرتبط بواقع الإنسان المعاصر.",
        "match": "دبلومة Orthodox Scriptures — برنامج عربي لمدة عامين، مكرّس بالكامل لدراسة العهدين القديم والجديد."
    },
    "D": {
        "title": "الخادم العملي",
        "description": "تبحث عن دراسة عميقة لكن سريعة وواضحة، وسهل الوصول لها. قد تكون مشغولًا، أو تخدم الجدد في الإيمان، أو تخدم في خدمة تعتمد على اللغة الإنجليزية. تفضّل دراسة خفيفة وبسيطة لكن بمحتوى ذي معنى.",
        "match": "شهادة Catechesis — أبسط وأكثر البرامج شمولًا باللغة الإنجليزية، تغطي مجالات التعليم المسيحي الأساسية."
    },
    "E": {
        "title": "الخادم المتأمل",
        "description": "أنت محب للتأمل، والتساؤل، والتحليل، والفهم العميق. تفكّر بعمق في الله، والحق، والمعنى، والعالم من حولك. يلجأ إليك الآخرون بأسئلتهم الصعبة لأنك تُحسن الإصغاء، والتحليل، والتوضيح. تشعر بدعوة إلى الدفاع عن الإيمان، أو الإرشاد، أو دراسة الرؤى الفكرية، أو الدفاعيات، أو فهم الثقافة والمعنى.",
        "match": "شهادة Walk in Wisdom — برنامج إنجليزي لمدة عام، مرن وقابل للتخصيص، في الدفاعيات، والفلسفة، والرؤى الفكرية، والإرشاد، ولاهوت الرسالة — مخصّص للمفكّرين والمتأمّلين."
    }
}
      };

      this.results = categories.map(cat => profiles[this.language][cat]);
    },
    restartQuiz() {
      this.currentQuestionIndex = 0;
      this.answers = Array(this.questions.length).fill(null);
      this.showResults = false;
      this.results = [];
      this.categoryCount = { A: 0, B: 0, C: 0, D: 0 ,E: 0};
      this.language_select = true;
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
