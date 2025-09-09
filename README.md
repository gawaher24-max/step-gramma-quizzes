# step-gramma-quizzes
<!doctype html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>اختبارات STEP – القرامر | تبويبات النماذج</title>
<style>
  :root{
    --brand:#d78378; --brand2:#ec8b70; --bg:#fff7f5; --ink:#1f1f1f; --muted:#666;
    --card:#ffffff; --ok:#1e8e3e; --bad:#b00020; --shade:0 10px 30px rgba(0,0,0,.06);
  }
  *{box-sizing:border-box}
  body{margin:0;background:linear-gradient(135deg,var(--bg),#fff);color:var(--ink);font-family:system-ui, "Segoe UI", Tahoma, Arial}
  header{padding:18px 20px;background:linear-gradient(90deg,var(--brand),var(--brand2));color:#fff}
  header h1{margin:0;font-size:22px}
  header p{margin:6px 0 0;font-size:14px;opacity:.95}

  main{max-width:1100px;margin:20px auto;padding:0 16px 40px}

  /* تبويبات */
  .tabs{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:14px}
  .tab{
    border:1px solid #ffd4cc;background:#fff;border-radius:999px;padding:8px 14px;
    color:#a23a2b; cursor:pointer; user-select:none
  }
  .tab.active{background:linear-gradient(90deg,var(--brand),var(--brand2));border-color:transparent;color:#fff}
  .tab .count{opacity:.85;margin-inline-start:6px;font-size:12px}

  .toolbar{display:flex;gap:8px;align-items:center;margin:10px 0 16px}
  .toolbar .spacer{flex:1}
  button{border-radius:10px;border:1px solid #eee;padding:10px 14px;font-size:14px;cursor:pointer}
  .btn-primary{background:var(--brand);color:#fff;border-color:var(--brand)}
  .btn-ghost{background:#fff;color:var(--brand);border-color:var(--brand)}

  .card{background:var(--card);border-radius:14px;box-shadow:var(--shade);padding:18px;margin:12px 0}
  .q-text{font-weight:700;margin-bottom:10px;line-height:1.5}
  .choices{display:grid;gap:10px}
  .choice{border:1px solid #eee;border-radius:12px;padding:10px;display:flex;gap:10px;align-items:flex-start}
  .choice input{margin-top:4px;inline-size:18px;block-size:18px}
  .meta{font-size:12px;color:var(--muted)}

  .result{padding:14px;border-radius:12px;margin:12px 0}
  .ok{background:#e8f5ec;color:#0a6e2e;border:1px solid #cfe9d7}
  .bad{background:#fdecee;color:#7c1020;border:1px solid #f4c9cf}
  .explain{font-size:13px;color:#444;margin-top:6px}

  .footer-note{color:#777;font-size:12px;text-align:center;margin-top:18px}
</style>
</head>
<body>
<header>
  <h1>اختبارات STEP – قسم القرامر</h1>
  <p>كل تبويب = نموذج مستقل. اختر التبويب، أجب، ثم اضغط <b>إنهاء وتصحيح</b> لعرض الدرجة والتصحيح الفوري.</p>
</header>

<main>
  <!-- شريط التبويبات -->
  <div class="tabs" id="tabs"></div>

  <!-- شريط أدوات النموذج الحالي -->
  <div class="toolbar">
    <div id="modelTitle" class="meta">النموذج الحالي</div>
    <div class="spacer"></div>
    <button id="submitBtn" class="btn-primary">إنهاء وتصحيح</button>
    <button id="resetBtn" class="btn-ghost">إعادة الاختبار</button>
  </div>

  <!-- هنا تُرسم الأسئلة -->
  <div id="quiz"></div>

  <!-- هنا تُعرض الدرجة -->
  <div id="scoreBox"></div>

  <p class="footer-note">المصدر: تجميعات “Free STEP Academy – Grammar”. (الإجابات موضوعة وفق القاعدة والسياق)</p>
</main>

<script>
/* ============== (1) بنك الأسئلة لكل تبويب/نموذج ============== */
/* ملاحظة: املئي model2, model3 … ببقية النماذج. نفس البنية تمامًا. */
const BANK = {
  model1: [
    /* —— النموذج 1: نفس المجموعة التي أعددتُها لك سابقًا —— */
    { q:"My sister's name is Anna. ...... is eight years old.", a:["He","Him","She","Her"], correct:2, explain:"الضمير يعود على مؤنث مفرد: She." },
    { q:"Russia is ...... than Canada.", a:["bigger","the bigger","biggest","the biggest"], correct:0, explain:"مقارنة: bigger." },
    { q:"He went to university in the UK ...... a PhD in physics.", a:["get","getting","to get","to getting"], correct:2, explain:"غاية: to + base → to get." },
    { q:"In my opinion, it is not economical ........ your money on cheap, poor-quality products.", a:["spend","spent","to spend","be spending"], correct:2, explain:"It is + adj + to-inf → to spend." },
    { q:"The computer costs so ........ money that the family decided not to buy it.", a:["most","more","many","much"], correct:3, explain:"money غير معدود → much." },
    { q:"Go outside and ......... the gardener some water.", a:["bring","to bring","bringing","will bring"], correct:0, explain:"فعل أمر + فعل مجرد." },
    { q:'In which sentence is all CAPITALIZATION correct?', a:[
      'Running late, he yelled, "Stop!" as he chased after the kingdom university bus that took him to work every morning.',
      'Running late, he yelled, "Stop!" as he chased after the Kingdom university bus that took him to work every morning.',
      'Running late, he yelled, "Stop!" as he chased after the Kingdom University bus that took him to work every morning.',
      'Running late, He yelled, "Stop!" as he chased after the Kingdom University bus that took him to work every morning.'
    ], correct:2, explain:"الأسماء العلم بحرف كبير: Kingdom University." },
    { q:"In which sentence is all CAPITALIZATION correct?", a:[
      "Among John's favorite places to travel were malaysia, morocco, and any exotic location with a nice, clean beach.",
      "Among John's favorite places to travel were Malaysia, Morocco, and any Exotic Location with a nice, clean beach.",
      "Among John's favorite places to travel were Malaysia, Morocco, and any exotic location with a nice, clean beach.",
      "Among John's favorite Places to Travel were Malaysia, Morocco, and any exotic location with a nice, clean beach."
    ], correct:2, explain:"الدول بحرف كبير، لا نُكبّر generic nouns." },
    { q:"Which sentence has the CORRECT WORD ORDER?", a:[
      "And then they would start getting ready for bed, after eating dinner, the family used to read books while drinking tea.",
      "After eating dinner, the family used to read books while drinking tea, and then they would start getting ready for bed.",
      "After eating dinner, and then they would start getting ready for bed, the family used to read books while drinking tea.",
      "The family used to read books while drinking tea, and then they would start getting ready, after eating dinner, for bed."
    ], correct:1, explain:"تمهيد → جملة رئيسية → and then …" },
    { q:"The students had been studying lesson one and the teacher wanted ................ lesson two.", a:["begin","to begin","they begin","that they begin"], correct:1, explain:"want + to-inf." },
    { q:"The boys' mother told them to finish all of their breakfast, ...... they stuffed their mouths until their plates were empty.", a:["however","since","as","so"], correct:3, explain:"نتيجة → so." },
    { q:"At tomorrow's event, a large sum of money ...... by an anonymous supporter.", a:["have donated","will donate","will be donated","are being donated"], correct:2, explain:"مبني للمجهول مستقبل: will be donated." },
    { q:"A large box of dates ...... from my uncle.", a:["have just arrived","has just arrived","having just arrived","just have arrived"], correct:1, explain:"المسند الحقيقي مفرد box → has." },
    { q:"Bob ...... on holiday last year.", a:["didn't go","didn't went","doesn't go","not go"], correct:0, explain:"نفي الماضي البسيط: didn't + base." },
    { q:"Which sentence has the CORRECT WORD ORDER?", a:[
      "He claimed he was simply meditating, but when asked about it later, Adam had obviously fallen asleep while sitting on the couch.",
      "Adam had obviously fallen asleep while sitting on the couch, but when asked about it later, he claimed he was simply meditating.",
      "Adam had obviously fallen asleep while sitting on the couch, he claimed he was simply meditating, but when asked about it later.",
      "While sitting on the couch, but when asked about it later, he claimed he was simply meditating, Adam had obviously fallen asleep."
    ], correct:1, explain:"ترتيب منطقي: الحدث ثم التبرير." },
    { q:"Steve's watch was not working properly ...... he took it to a repair shop.", a:["but","so","because","while"], correct:1, explain:"سبب ← نتيجة: so." },
    { q:"To which sentence can we add the clause: “Since she was living in Saudi Arabia, ...” ?", a:["[1] Sarah wanted to learn Arabic.","[2] To start, she made a flash card for each letter.","[3] She practiced with the flash cards until she memorized the entire Arabic alphabet.","[4] Then she bought children's books to build vocabulary."], correct:0, explain:"تصلح تمهيدًا قبل [1]." },
    { q:"Which sentence has the CORRECT WORD ORDER?", a:[
      "Invented by Thomas Edison, was the electric light bulb, one of the many commercially successful products, an essential element of our modern lives.",
      "One of the many commercially successful products, an essential element of our modern lives was invented by Thomas Edison, the electric light bulb.",
      "The electric light bulb, an essential element of our modern lives, was one of the many commercially successful products invented by Thomas Edison.",
      "An essential element of our modern lives, one of the many commercially successful products, was invented the electric light bulb by Thomas Edison."
    ], correct:2, explain:"المبتدأ: The electric light bulb … was … invented." },
    { q:"Which one is INCORRECT in: “Did you know that the tree over there might actually been hundreds of years old?”", a:["Did","know","there","been"], correct:3, explain:"بعد might → be (وليس been)." },
    { q:"Which sentence has the CORRECT WORD ORDER?", a:[
      "Catching ants that were crawling around inside the house, the boys spent the whole afternoon and putting them outside in the garden.",
      "The boys spent the whole afternoon catching ants that were crawling around inside the house and putting them outside in the garden.",
      "Catching ants that were crawling around inside the house and putting them outside in the garden, the boys spent the whole afternoon.",
      "The boys spent the whole afternoon and putting them outside in the garden, catching ants that were crawling around inside the house."
    ], correct:1, explain:"spent … catching … and putting …" },
    { q:"INCORRECT in: “Tom and Adam went to play tennis … hours ago, and still has not returned home yet.”", a:["to play","ago","has","yet"], correct:2, explain:"فاعل جمع → have." },
    { q:"INCORRECT in: “To become a doctor and to helping sick children are the goals that Lucy hopes to achieve …”", a:["to helping","the goals","hopes","finishes"], correct:0, explain:"التوازن: to become … and to help." },
    { q:"Working in a university last year ...... very interesting.", a:["is","was","can be","could be"], correct:1, explain:"إشارة للماضي → was." },
    { q:"If I ...... my car back from the mechanic tomorrow, I will travel to Dubai.", a:["got","get","am getting","will get"], correct:1, explain:"شرط أول: If + present → will + base." },
    { q:"I do not understand this sentence. What ...........................?", a:["does mean this word.","does this word mean.","this word means.","means this word."], correct:1, explain:"What does this word mean?" },
    { q:"He worked ............. forty years before he decided to retire.", a:["for","from","since","during"], correct:0, explain:"مدة → for." },
    { q:"Good! .......... you remember to tell him to fix the stairs?", a:["Are","Were","Do","Did"], correct:3, explain:"سؤال ماضٍ محدد → Did … ?" },
    { q:"In England, many children ............. wear school uniforms; they can't come to school if they don't.", a:["have to","must to","need","can"], correct:0, explain:"have to (وليس must to)." },
    { q:"That's a beautiful new jacket! Where ......................... you buy it?", a:["are","were","have","did"], correct:3, explain:"Where did you buy it?" },
    { q:"When you arrive, someone ............. for you at the airport.", a:["waits","will wait","is waiting","will be waiting"], correct:3, explain:"will be waiting (ترتيب مستقبلي)." },
    { q:"The children could not ...... out to ride their bikes because it was raining.", a:["go","going","gone","to go"], correct:0, explain:"بعد could → base." },
    { q:"They went to bed as soon as their guests ........... left.", a:["are","were","had","have"], correct:2, explain:"had left قبل الماضي البسيط." },
    { q:"Does Chris have to go home now? Let him .......... a while longer.", a:["stay","stays","to stay","staying"], correct:0, explain:"let + مفعول + base." },
    { q:"Tom cannot remember ..................", a:["where are kept the files","where are the files kept","where the files kept are","where the files are kept"], correct:3, explain:"سؤال غير مباشر → ترتيب خبرية." },
    { q:"Dairy products should ......................... to keep them fresh.", a:["refrigerate","refrigerating","be refrigerate","be refrigerated"], correct:3, explain:"should be + p.p." },
    { q:"The box ............................. from recycled paper.", a:["was made","was making","made","make"], correct:0, explain:"مبني للمجهول (ماضٍ): was made." },
    { q:"If I ....................... there, I ....................... said something to the manager.", a:["was/ would had","have been/will have","had been / would have","were / would had"], correct:2, explain:"شرط ثالث: had + pp → would have + pp." },
    { q:"After he ................. the door, he discovered that his keys were not in his pocket.", a:["was closing","was closed","had closed","has closed"], correct:2, explain:"حدث أسبق: had closed." },
    { q:"The postman had already delivered the mail by the time I ........................... for work this morning.", a:["have left","had left","leave","left"], correct:3, explain:"by the time + past simple (left) مقابل had delivered." },
    { q:"— Are you going to the party tonight?  .........................", a:["Thank you","I was playing football","You bet",""], correct:2, explain:"رد تأكيدي مناسب: You bet!" }
  ],

  /* ——— أضيفي هنا بقية النماذج ——— */
  model2: [
    /* مثال مبدئي — استبدليه بأسئلة النموذج 2 الحقيقي: 
    { q:"...", a:["A","B","C","D"], correct:0, explain:"سبب التصحيح..." },
    */
  ],
  model3: [],
  model4: [],
  model5: [],
  model6: []
};

/* أسماء التبويبات بالترتيب */
const TAB_ORDER = [
  {key:'model1', title:'النموذج 1'},
  {key:'model2', title:'النموذج 2'},
  {key:'model3', title:'النموذج 3'},
  {key:'model4', title:'النموذج 4'},
  {key:'model5', title:'النموذج 5'},
  {key:'model6', title:'النموذج 6'}
];

/* ============== (2) عناصر DOM ============== */
const tabsEl = document.getElementById('tabs');
const quizEl = document.getElementById('quiz');
const scoreBox = document.getElementById('scoreBox');
const modelTitle = document.getElementById('modelTitle');
const submitBtn = document.getElementById('submitBtn');
const resetBtn = document.getElementById('resetBtn');

let currentKey = TAB_ORDER[0].key;

/* ============== (3) رسم التبويبات ============== */
function renderTabs(){
  tabsEl.innerHTML = '';
  TAB_ORDER.forEach(t=>{
    const btn = document.createElement('button');
    btn.className = 'tab'+(currentKey===t.key?' active':'');
    const count = BANK[t.key]?.length || 0;
    btn.innerHTML = `${t.title}<span class="count">(${count})</span>`;
    btn.addEventListener('click', ()=>{
      currentKey = t.key;
      document.querySelectorAll('.tab').forEach(x=>x.classList.remove('active'));
      btn.classList.add('active');
      renderQuiz(currentKey);
      window.scrollTo({top:0,behavior:'smooth'});
    });
    tabsEl.appendChild(btn);
  });
}

/* ============== (4) رسم الأسئلة للنموذج الحالي ============== */
function renderQuiz(setKey){
  quizEl.innerHTML = '';
  scoreBox.innerHTML = '';
  modelTitle.textContent = `النموذج الحالي: ${TAB_ORDER.find(t=>t.key===setKey).title}`;
  const qs = BANK[setKey] || [];
  if(!qs.length){
    quizEl.innerHTML = `<div class="card"><div class="q-text">لا توجد أسئلة بعد في هذا التبويب.</div>
      <div class="meta">أضيفي الأسئلة داخل BANK.${setKey} في الكود (انظري التعليقات).</div></div>`;
    return;
  }

  qs.forEach((item, idx)=>{
    const card = document.createElement('div');
    card.className = 'card';
    card.dataset.qindex = idx;

    const q = document.createElement('div');
    q.className = 'q-text';
    q.textContent = (idx+1)+". "+item.q;
    card.appendChild(q);

    const choices = document.createElement('div');
    choices.className = 'choices';
    (item.a||[]).forEach((text, i)=>{
      const row = document.createElement('label');
      row.className = 'choice';
      const radio = document.createElement('input');
      radio.type = 'radio';
      radio.name = setKey + '_q' + idx; // اسم فريد لكل تبويب
      radio.value = i;
      row.appendChild(radio);
      const span = document.createElement('span');
      span.textContent = text;
      row.appendChild(span);
      choices.appendChild(row);
    });

    const meta = document.createElement('div');
    meta.className = 'meta';
    meta.textContent = 'اختر إجابة واحدة';
    card.appendChild(choices);
    card.appendChild(meta);

    quizEl.appendChild(card);
  });
}

/* ============== (5) التصحيح للنموذج الحالي فقط ============== */
submitBtn.addEventListener('click', ()=>{
  const qs = BANK[currentKey] || [];
  if(!qs.length){ return; }

  // إزالة نتائج سابقة
  document.querySelectorAll('.result').forEach(x=>x.remove());

  let correctCount = 0;
  let wrongs = [];
  qs.forEach((item, idx)=>{
    const chosen = document.querySelector(`input[name="${currentKey}_q${idx}"]:checked`);
    const card = document.querySelector(`.card[data-qindex="${idx}"]`);
    const res = document.createElement('div');

    if (chosen && Number(chosen.value) === item.correct){
      correctCount++;
      res.className = 'result ok';
      res.innerHTML = '✔️ إجابة صحيحة';
    }else{
      res.className = 'result bad';
      const your = chosen ? item.a[Number(chosen.value)] : '— لم تُجِب —';
      const correctText = item.a[item.correct];
      res.innerHTML = `❌ إجابة غير صحيحة<br>
        <div class="explain"><b>الصحيح:</b> ${correctText}<br>${item.explain || ''}</div>`;
      wrongs.push({idx, your, correctText});
    }
    card.appendChild(res);
  });

  const score = Math.round(100 * correctCount / qs.length);
  scoreBox.innerHTML = `
    <div class="card">
      <div class="q-text">نتيجتك في ${TAB_ORDER.find(t=>t.key===currentKey).title}: ${correctCount} / ${qs.length} — ${score}%</div>
      ${wrongs.length ? `<div class="meta">عدد الأسئلة الخاطئة: ${wrongs.length}</div>` : `<div class="meta">ممتاز! جميع الإجابات صحيحة 🎉</div>`}
    </div>`;
  window.scrollTo({top:0,behavior:'smooth'});
});

/* ============== (6) إعادة تعيين للنموذج الحالي فقط ============== */
resetBtn.addEventListener('click', ()=>{
  // إلغاء التحديد والنتائج لهذا التبويب
  document.querySelectorAll(`input[name^="${currentKey}_q"]`).forEach(r=> r.checked = false);
  document.querySelectorAll('.result').forEach(x=>x.remove());
  scoreBox.innerHTML = '';
  window.scrollTo({top:0,behavior:'smooth'});
});

/* بدء التشغيل */
renderTabs();
renderQuiz(currentKey);

/* ============== (7) كيف تضيفين أسئلة نموذج جديد؟ ==============
1) اذهبي إلى الكائن BANK أعلاه، وافتحي القسم model2 (أو model3 …).
2) أضيفي الأسئلة بالمصفوفة بنفس البنية:
   {
     q: "نص السؤال هنا",
     a: ["الخيار A","الخيار B","الخيار C","الخيار D"],
     correct: 0, // رقم الفهرس الصحيح (0=A، 1=B، 2=C، 3=D)
     explain: "شرح مختصر لماذا هذه الإجابة صحيحة"
   }
3) لو احتجتِ تبويبًا إضافيًا (model7 مثلاً):
   - أضيفي BANK.model7 = [ ... ].
   - أضيفي سطرًا إلى TAB_ORDER: {key:'model7', title:'النموذج 7'}
   انتهى! سيظهر تبويب جديد تلقائيًا مع عدّاد الأسئلة.
=============================================================== */
</script>
</body>
</html>
