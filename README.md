<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>あなたの性格タイプ診断</title>
<style>
  /* フォント指定（ユーザー環境に筑紫A丸ゴシックがあれば反映されます） */
  :root{
    --pink:#ff7fb5;
    --bg:#fff6fb;
    --card:#fff;
    --accent:#ff4d8b;
  }

  html,body{
    height:100%;
    margin:0;
    font-family: "筑紫A丸ゴシック","Tsukushi A Maru Gothic", "Hiragino Kaku Gothic ProN", "Noto Sans JP", "メイリオ", sans-serif;
    background: linear-gradient(180deg, #fff 0%, #fff6fb 100%);
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
    color:#333;
  }

  /* 全体中央レイアウト */
  .wrap{
    min-height:100%;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:24px;
    box-sizing:border-box;
  }

  .card{
    width:100%;
    max-width:520px;
    background:var(--card);
    border-radius:16px;
    box-shadow: 0 10px 30px rgba(255, 120, 170, 0.12);
    padding:28px;
    box-sizing:border-box;
    text-align:center;
  }

  /* タイトル */
  h1{
    margin:0 0 18px 0;
    font-size:24px;
    color:var(--accent);
    letter-spacing:0.02em;
    font-weight:700;
  }

  /* ピンク強調の装飾ライン */
  .title-deco{
    width:64px;
    height:6px;
    background:linear-gradient(90deg,var(--pink),#ff9ad0);
    margin:10px auto 18px auto;
    border-radius:20px;
    box-shadow: 0 6px 18px rgba(255,120,170,0.12);
  }

  /* 質問エリア */
  .question{
    font-size:18px;
    margin:14px 0 18px 0;
    min-height:56px;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:0 12px;
  }

  /* 選択肢ボタン */
  .choices{
    display:flex;
    gap:12px;
    justify-content:center;
    margin-top:8px;
    flex-wrap:wrap;
  }
  button.choice{
    min-width:76px;
    padding:12px 18px;
    border-radius:12px;
    border:none;
    font-size:16px;
    cursor:pointer;
    background:linear-gradient(180deg,#fff,#ffeef8);
    box-shadow: 0 6px 14px rgba(255,120,170,0.12);
    transition:transform .12s ease, box-shadow .12s;
  }
  button.choice:active{ transform:translateY(2px) scale(.99); }
  button.choice:focus{ outline:3px solid rgba(255,127,181,0.18); }

  /* 進捗表示 */
  .progress{
    margin-top:18px;
    font-size:13px;
    color:#666;
  }

  /* 結果カード */
  .result-wrap{
    margin-top:18px;
    display:none;
    justify-content:center;
    align-items:center;
  }
  .result{
    width:100%;
    background: linear-gradient(180deg,#fff,#fff6fb);
    border-radius:14px;
    padding:18px;
    box-sizing:border-box;
    border: 1px solid rgba(255,120,170,0.14);
    box-shadow: 0 12px 30px rgba(255,120,170,0.08);
    transform-origin:center;
    opacity:0;
    transform: translateY(8px) scale(.995);
  }

  .result.show{
    display:block;
    animation:fadeInUp .5s ease forwards;
  }

  @keyframes fadeInUp{
    to{ opacity:1; transform:none; }
  }

  .result-title{
    font-size:18px;
    margin:0 0 8px 0;
    color:var(--accent);
    font-weight:700;
  }
  .result-desc{
    font-size:15px;
    margin:6px 0 12px 0;
    color:#444;
    line-height:1.6;
  }

  /* イラスト風丸 */
  .result-badge{
    display:inline-block;
    width:68px;
    height:68px;
    border-radius:50%;
    background:linear-gradient(135deg,#ffd9ec,#fff);
    border:2px solid rgba(255,120,170,0.12);
    box-shadow: 0 8px 18px rgba(255,120,170,0.09);
    margin-bottom:8px;
    line-height:68px;
    font-weight:700;
    color:var(--accent);
  }

  /* リセットボタン */
  .reset{
    margin-top:14px;
    padding:10px 14px;
    border-radius:12px;
    border:none;
    cursor:pointer;
    font-weight:700;
    background: linear-gradient(90deg,#ff9ccf,#ff6ea8);
    color:#fff;
    box-shadow: 0 8px 20px rgba(255,110,150,0.18);
  }

  /* 小さい補助テキスト */
  .hint{ font-size:13px; color:#888; margin-top:8px; }

  /* スマホ調整 */
  @media (max-width:420px){
    .card{ padding:18px; border-radius:12px; }
    button.choice{ min-width:64px; padding:10px 14px; font-size:15px; }
  }
</style>
</head>
<body>
  <div class="wrap">
    <div class="card" role="main" aria-labelledby="mainTitle">
      <h1 id="mainTitle">あなたの性格タイプ診断</h1>
      <div class="title-deco" aria-hidden="true"></div>

      <!-- 質問エリア -->
      <div id="questionArea">
        <!-- 動的に差し替え -->
        <div class="question" id="questionText">準備中...</div>

        <div class="choices" id="choices">
          <!-- ボタンはJSで生成 -->
        </div>

        <div class="progress" id="progressText">0 / 3</div>
      </div>

      <!-- 結果エリア -->
      <div class="result-wrap" id="resultWrap" aria-live="polite">
        <div class="result" id="resultCard" role="status">
          <div class="result-badge" id="resultBadge">✦</div>
          <h2 class="result-title" id="resultTitle">結果タイプ</h2>
          <p class="result-desc" id="resultDesc">ここに診断説明が入ります。</p>
          <button class="reset" id="restartBtn">もう一度診断する</button>
          <div class="hint">（ボタンを押すと最初からやり直せます）</div>
        </div>
      </div>

    </div>
  </div>

<script>
  // --- 質問と選択肢 --- //
  const qa = [
    {
      q: "1. 初対面の人と会うとき、あなたの動きは？",
      choices: {
        A: "直感で話題を変えたり盛り上げる",
        B: "落ち着いて観察し、論理的に話す",
        C: "相手に合わせてバランスを取る"
      }
    },
    {
      q: "2. 仕事や学習で大事にするのは？",
      choices: {
        A: "ひらめきや創造性",
        B: "計画と効率的な方法",
        C: "人間関係と調和"
      }
    },
    {
      q: "3. 休みの日の過ごし方は？",
      choices: {
        A: "思いつきでお出かけや新しいことを試す",
        B: "学びや整理整頓をする",
        C: "友達や家族とゆったり過ごす"
      }
    }
  ];

  // カウント保持
  const counts = { A:0, B:0, C:0 };
  let index = 0;

  // DOM
  const qText = document.getElementById('questionText');
  const choicesWrap = document.getElementById('choices');
  const progressText = document.getElementById('progressText');
  const resultWrap = document.getElementById('resultWrap');
  const resultCard = document.getElementById('resultCard');
  const resultTitle = document.getElementById('resultTitle');
  const resultDesc = document.getElementById('resultDesc');
  const resultBadge = document.getElementById('resultBadge');
  const restartBtn = document.getElementById('restartBtn');

  // 初期表示
  function showQuestion(i){
    const item = qa[i];
    // fade out current then replace for nicer UX
    qText.style.opacity = 0;
    choicesWrap.style.opacity = 0;
    setTimeout(() => {
      qText.textContent = item.q;
      // clear choices
      choicesWrap.innerHTML = "";
      // create buttons
      Object.entries(item.choices).forEach(([key, text]) => {
        const btn = document.createElement('button');
        btn.className = 'choice';
        btn.type = 'button';
        btn.innerHTML = `<strong>${key}</strong><div style="font-size:13px;margin-top:6px">${text}</div>`;
        btn.dataset.code = key;
        btn.addEventListener('click', onChoose);
        choicesWrap.appendChild(btn);
      });
      // update progress
      progressText.textContent = `${i} / ${qa.length}`;
      // fade in
      setTimeout(() => {
        qText.style.transition = "opacity .18s ease";
        choicesWrap.style.transition = "opacity .18s ease";
        qText.style.opacity = 1;
        choicesWrap.style.opacity = 1;
      }, 30);
    }, 160);
  }

  // 選択時
  function onChoose(e){
    const key = e.currentTarget.dataset.code;
    if(!key) return;
    counts[key] = (counts[key]||0) + 1;

    // 次の問題へ or 結果表示
    index++;
    if(index < qa.length){
      showQuestion(index);
    } else {
      showResult();
    }
  }

  // 結果判定
  function showResult(){
    // determine majority
    const entries = Object.entries(counts); // [ ['A',n], ... ]
    // find max
    let max = -1;
    entries.forEach(([k,v]) => { if(v > max) max = v; });
    // check how many have max
    const topKeys = entries.filter(([k,v])=>v===max).map(e=>e[0]);

    let typeKey, title, desc, badge;
    if(max === 0 || topKeys.length !== 1){
      // no majority or tie
      typeKey = "mix";
      title = "自由タイプ";
      desc = "複数の特徴がバランスよく混ざっています。固定観念に囚われず、柔軟に行動するのが得意です。";
      badge = "♛";
    } else {
      const tk = topKeys[0];
      if(tk === "A"){
        typeKey = "A";
        title = "直感タイプ";
        desc = "ひらめきや直感で動くことが得意。新しいことに

