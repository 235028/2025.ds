<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>今日の運勢をチェック！</title>
  <style>
    body {
      font-family: 'Hiragino Maru Gothic ProN', 'Arial', sans-serif;
      text-align: center;
      background-color: #fff7f9;
      color: #333;
      margin: 0;
      padding: 0;
    }

    h1 {
      background: linear-gradient(90deg, #ff9a9e, #fecfef);
      color: white;
      padding: 20px 0;
      font-size: 2em;
      margin-bottom: 30px;
      box-shadow: 0 3px 5px rgba(0,0,0,0.1);
    }

    button {
      background-color: #ffb6c1;
      border: none;
      color: white;
      padding: 12px 24px;
      font-size: 1.1em;
      border-radius: 25px;
      cursor: pointer;
      transition: 0.3s;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    }

    button:hover {
      background-color: #ff9aa2;
      transform: scale(1.05);
    }

    #result {
      margin-top: 30px;
      display: none;
      animation: fadeIn 1s ease-in;
    }

    .card {
      display: inline-block;
      padding: 20px 30px;
      border-radius: 20px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      color: white;
      font-size: 1.2em;
      margin-top: 10px;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>
  <h1>今日の運勢をチェック！</h1>
  <button id="uranaiBtn">占う</button>
  <div id="result">
    <div id="fortuneCard" class="card"></div>
    <p id="luckyItem"></p>
    <button id="retryBtn" style="display:none;">もう一度占う</button>
  </div>

  <script>
    const fortunes = [
      { text: "超ハッピー", color: "#ffcc70" },
      { text: "好調", color: "#fff176" },
      { text: "普通", color: "#b3e5fc" },
      { text: "少し注意", color: "#ffb74d" },
      { text: "要注意", color: "#e57373" }
    ];

    const items = [
      "ラッキーコーヒー", "青いペン", "お気に入りの香水",
      "白いスニーカー", "キラキラアクセサリー", "ヘアクリップ",
      "チョコレート", "ふわふわのタオル", "イヤホン", "メモ帳"
    ];

    const resultDiv = document.getElementById("result");
    const fortuneCard = document.getElementById("fortuneCard");
    const luckyItem = document.getElementById("luckyItem");
    const uranaiBtn = document.getElementById("uranaiBtn");
    const retryBtn = document.getElementById("retryBtn");

    function showFortune() {
      const fortune = fortunes[Math.floor(Math.random() * fortunes.length)];
      const item = items[Math.floor(Math.random() * items.length)];

      fortuneCard.textContent = `あなたの運勢：${fortune.text}`;
      fortuneCard.style.backgroundColor = fortune.color;

      luckyItem.textContent = `💫 ラッキーアイテム：${item}`;
      resultDiv.style.display = "block";
      retryBtn.style.display = "inline-block";
      resultDiv.style.animation = "fadeIn 1s ease-in";
    }

    uranaiBtn.addEventListener("click", showFortune);
    retryBtn.addEventListener("click", showFortune);
  </script>
</body>
</html>
