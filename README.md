<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>歌詞情緒分析專題</title>
  <style>
    :root{
      --bg:#0b1220;
      --card:#111a2e;
      --text:#eaf0ff;
      --muted:#a9b4d0;
      --accent:#7aa2ff;
      --good:#3ddc97;
      --bad:#ff6b6b;
      --mid:#ffd166;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family: system-ui, -apple-system, "PingFang TC","Microsoft JhengHei", sans-serif;
      background: radial-gradient(1200px 600px at 20% 10%, #17244a 0%, transparent 60%),
                  radial-gradient(900px 500px at 80% 20%, #2a1b4a 0%, transparent 55%),
                  var(--bg);
      color:var(--text);
      line-height:1.7;
    }
    a{color:var(--accent); text-decoration:none}
    header{
      position:sticky; top:0; z-index:10;
      backdrop-filter: blur(10px);
      background: rgba(11,18,32,.65);
      border-bottom:1px solid rgba(255,255,255,.08);
    }
    .nav{
      max-width:1100px; margin:0 auto; padding:14px 18px;
      display:flex; align-items:center; justify-content:space-between;
      gap:12px;
    }
    .brand{
      display:flex; align-items:center; gap:10px; font-weight:700;
      letter-spacing:.5px;
    }
    .dot{
      width:10px; height:10px; border-radius:50%;
      background: var(--accent);
      box-shadow: 0 0 20px rgba(122,162,255,.8);
    }
    .navlinks{display:flex; gap:16px; flex-wrap:wrap}
    .navlinks a{color:var(--muted); font-weight:600}
    .navlinks a:hover{color:var(--text)}
    .wrap{max-width:1100px; margin:0 auto; padding:28px 18px 60px}
    .hero{
      display:grid; grid-template-columns: 1.3fr .9fr; gap:18px;
      align-items:stretch;
    }
    @media (max-width: 900px){ .hero{grid-template-columns:1fr} }
    .card{
      background: rgba(17,26,46,.86);
      border:1px solid rgba(255,255,255,.08);
      border-radius:18px;
      box-shadow: 0 18px 40px rgba(0,0,0,.25);
      padding:18px;
    }
    .title{font-size:28px; margin:0 0 6px}
    .subtitle{color:var(--muted); margin:0 0 14px}
    .pillrow{display:flex; gap:10px; flex-wrap:wrap; margin-top:10px}
    .pill{
      padding:6px 10px; border-radius:999px;
      background: rgba(122,162,255,.12);
      border:1px solid rgba(122,162,255,.25);
      color: var(--text);
      font-weight:600;
      font-size:13px;
    }
    .grid{
      margin-top:18px;
      display:grid; grid-template-columns: repeat(3, 1fr);
      gap:14px;
    }
    @media (max-width: 900px){ .grid{grid-template-columns:1fr} }
    .stat{
      padding:14px; border-radius:16px;
      border:1px solid rgba(255,255,255,.08);
      background: rgba(255,255,255,.03);
    }
    .stat .k{color:var(--muted); font-size:13px}
    .stat .v{font-size:22px; font-weight:800; margin-top:4px}
    .section{margin-top:22px}
    h2{margin:0 0 10px; font-size:20px}
    .row{display:grid; grid-template-columns:1fr 1fr; gap:14px}
    @media (max-width: 900px){ .row{grid-template-columns:1fr} }

    textarea{
      width:100%; min-height:180px; resize:vertical;
      border-radius:14px;
      padding:12px 12px;
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.25);
      color: var(--text);
      outline:none;
    }
    textarea::placeholder{color:#8693b6}
    .btns{display:flex; gap:10px; flex-wrap:wrap; margin-top:10px}
    button{
      cursor:pointer;
      border:none;
      padding:10px 12px;
      border-radius:12px;
      font-weight:800;
      color: #081022;
      background: var(--accent);
    }
    button.secondary{
      background: rgba(255,255,255,.10);
      color: var(--text);
      border:1px solid rgba(255,255,255,.10);
    }
    .result{
      padding:14px;
      border-radius:16px;
      border:1px solid rgba(255,255,255,.08);
      background: rgba(255,255,255,.03);
    }
    .meter{
      height:12px; border-radius:999px;
      background: rgba(255,255,255,.08);
      overflow:hidden;
      border:1px solid rgba(255,255,255,.10);
      margin-top:10px;
    }
    .bar{
      height:100%;
      width: 50%;
      background: linear-gradient(90deg, var(--bad), var(--mid), var(--good));
      border-radius:999px;
      transition: width .3s ease;
    }
    .tagrow{display:flex; gap:10px; flex-wrap:wrap; margin-top:10px}
    .tag{
      font-size:13px; font-weight:700;
      padding:6px 10px; border-radius:999px;
      border:1px solid rgba(255,255,255,.10);
      background: rgba(0,0,0,.18);
      color: var(--muted);
    }
    footer{
      margin-top:24px; color:var(--muted); font-size:13px;
      text-align:center; opacity:.9;
    }
    .small{font-size:13px; color:var(--muted)}
    .mono{font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace}
  </style>
</head>
<body>
  <header>
    <div class="nav">
      <div class="brand">
        <div class="dot"></div>
        <div>歌詞情緒分析專題</div>
      </div>
      <nav class="navlinks">
        <a href="#intro">簡介</a>
        <a href="#demo">分析展示</a>
        <a href="#method">方法</a>
        <a href="#team">分工</a>
      </nav>
    </div>
  </header>

  <div class="wrap">
    <section class="hero" id="intro">
      <div class="card">
        <h1 class="title">用文字看見情緒：歌詞情緒分析</h1>
        <p class="subtitle">
          這個網站示範一個「可展示的畢專頁面」：輸入歌詞 → 計算情緒分數 → 顯示結果與關鍵詞。
          你之後也可以把後端 Python/模型接上來（API）讓結果更準。
        </p>
        <div class="pillrow">
          <span class="pill">HTML / CSS / JS</span>
          <span class="pill">情緒分數視覺化</span>
          <span class="pill">關鍵詞標記</span>
          <span class="pill">可接 Python API</span>
        </div>

        <div class="grid">
          <div class="stat">
            <div class="k">分析模式</div>
            <div class="v" id="modeText">字典規則</div>
          </div>
          <div class="stat">
            <div class="k">目前情緒</div>
            <div class="v" id="moodText">—</div>
          </div>
          <div class="stat">
            <div class="k">情緒分數</div>
            <div class="v"><span id="scoreText">—</span><span class="small"> / 100</span></div>
          </div>
        </div>
      </div>

      <div class="card">
        <h2>快速導覽</h2>
        <p class="small">
          你可以先用右邊的範例歌詞試試看。這裡用「情緒字典」做基本判斷：
          正向詞加分、負向詞扣分，中性詞不影響。
        </p>
        <div class="result">
          <div class="small">小提醒</div>
          <div style="font-weight:800;margin-top:6px;">
            想變成「真正畢專」：把分析改成 Python（如 TextBlob / transformers / 自訓模型），
            然後用 API 回傳分數給前端。
          </div>
          <div class="tagrow">
            <span class="tag">可直接展示</span>
            <span class="tag">可擴充後端</span>
            <span class="tag">排版乾淨</span>
          </div>
        </div>
      </div>
    </section>

    <section class="section" id="demo">
      <h2>分析展示</h2>
      <div class="row">
        <div class="card">
          <div class="small">貼上歌詞（可多行）</div>
          <textarea id="lyrics" placeholder="把歌詞貼在這裡，例如：&#10;我好想你 但又覺得好難過&#10;天空很藍 可是我心很亂"></textarea>
          <div class="btns">
            <button id="analyzeBtn">開始分析</button>
            <button class="secondary" id="sampleBtn">載入範例</button>
            <button class="secondary" id="clearBtn">清空</button>
          </div>
          <div class="small" style="margin-top:10px;">
            技巧：要更像研究可以加上「資料來源 / 資料量 / 評估指標（accuracy、F1）」等內容。
          </div>
        </div>

        <div class="card">
          <div class="small">分析結果</div>
          <div class="result" style="margin-top:8px;">
            <div style="display:flex; align-items:baseline; justify-content:space-between; gap:10px; flex-wrap:wrap;">
              <div style="font-size:18px; font-weight:900;">
                情緒判定：<span id="labelText">—</span>
              </div>
              <div class="small mono">pos: <span id="posCount">0</span> / neg: <span id="negCount">0</span></div>
            </div>

            <div class="meter">
              <div class="bar" id="bar"></div>
            </div>
            <div class="small" style="margin-top:8px;">
              分數：<span class="mono" id="scoreLine">—</span>（越高越偏正向）
            </div>

            <div class="tagrow" id="hitTags" style="margin-top:12px;"></div>

            <div class="small" style="margin-top:12px;">
              <b>說明：</b>目前是示範版（規則字典）。之後你把「分析」改成呼叫後端 API，
              這塊 UI 不用改就能直接顯示模型結果。
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="section" id="method">
      <h2>方法與流程（可放在報告/口試）</h2>
      <div class="card">
        <ol>
          <li><b>資料蒐集：</b>從歌詞資料來源整理文本（去除標點、統一大小寫等）。</li>
          <li><b>前處理：</b>斷詞（中文可用 Jieba）、移除停用詞、正規化。</li>
          <li><b>情緒分析：</b>（示範）情緒字典；（進階）機器學習/深度學習模型。</li>
          <li><b>視覺化：</b>分數條、關鍵詞標籤、統計資訊。</li>
          <li><b>評估：</b>使用標註資料計算 Accuracy / Precision / Recall / F1。</li>
        </ol>
        <div class="small">
          你要我幫你把這段改成更像你們「靜宜資科畢專」口吻，我也可以直接幫你潤稿。
        </div>
      </div>
    </section>

    <section class="section" id="team">
      <h2>組員分工（可自己改）</h2>
      <div class="card">
        <ul>
          <li><b>前端：</b>頁面設計、結果視覺化、互動流程</li>
          <li><b>後端：</b>API、模型推論、資料庫（可選）</li>
          <li><b>資料：</b>歌詞整理、標註、評估指標與報告</li>
        </ul>
        <div class="small">提示：口試最加分的是「你們怎麼做評估」和「資料怎麼來」。</div>
      </div>
    </section>

    <footer>
      © 2026 歌詞情緒分析專題｜前端示範模板（可擴充接 Python/模型 API）
    </footer>
  </div>

  <script>
    // --- 示範用情緒字典（你可以自己擴充） ---
    const positiveWords = [
      "開心","幸福","喜歡","愛","溫柔","美好","希望","陽光","快樂","微笑","自由","擁抱","感謝","期待","勇敢"
    ];
    const negativeWords = [
      "難過","傷心","痛","孤單","失望","討厭","哭","崩潰","疲倦","黑暗","分開","後悔","害怕","絕望","心碎"
    ];

    const el = (id) => document.getElementById(id);

    function tokenize(text){
      // 超簡易切詞：以非中文字/英數當分隔（示範用）
      // 真的畢專建議改用 jieba / CKIP / 你們的斷詞流程
      const cleaned = text.replace(/[，。！？、,.!?;:()\[\]{}"“”'’]/g, " ");
      // 先把連續空白變成一格
      const spaced = cleaned.replace(/\s+/g, " ").trim();
      // 再把每個字也拆出來，避免詞典抓不到（很粗略）
      const words = spaced ? spaced.split(" ") : [];
      const chars = text.replace(/\s+/g, "").split("");
      return [...words, ...chars].filter(Boolean);
    }

    function analyze(text){
      const tokens = tokenize(text);
      let posHits = [];
      let negHits = [];

      for (const t of tokens){
        if (positiveWords.includes(t)) posHits.push(t);
        if (negativeWords.includes(t)) negHits.push(t);
      }

      const pos = posHits.length;
      const neg = negHits.length;

      // 基礎分數：50 為中性，正向加分、負向扣分
      let score = 50 + pos * 6 - neg * 7;
      score = Math.max(0, Math.min(100, score));

      let label = "中性";
      if (score >= 65) label = "偏正向";
      else if (score <= 35) label = "偏負向";

      return {score, label, pos, neg, posHits, negHits};
    }

    function render(result){
      el("labelText").textContent = result.label;
      el("scoreText").textContent = result.score.toFixed(0);
      el("scoreLine").textContent = result.score.toFixed(0) + " / 100";
      el("posCount").textContent = result.pos;
      el("negCount").textContent = result.neg;

      el("moodText").textContent = result.label;

      // meter bar
      el("bar").style.width = `${result.score}%`;

      // tags
      const container = el("hitTags");
      container.innerHTML = "";

      const mkTag = (text, type) => {
        const span = document.createElement("span");
        span.className = "tag";
        span.textContent = text;
        // 用 border 色暗示正負（不指定顏色太多，簡單做）
        if (type === "pos") span.style.borderColor = "rgba(61,220,151,.35)";
        if (type === "neg") span.style.borderColor = "rgba(255,107,107,.35)";
        return span;
      };

      const uniq = (arr) => [...new Set(arr)];
      const posU = uniq(result.posHits);
      const negU = uniq(result.negHits);

      if (posU.length === 0 && negU.length === 0){
        container.appendChild(mkTag("未命中情緒詞（可擴充詞典）", "mid"));
      } else {
        posU.slice(0,10).forEach(w => container.appendChild(mkTag("+"+w, "pos")));
        negU.slice(0,10).forEach(w => container.appendChild(mkTag("-"+w, "neg")));
      }
    }

    // Buttons
    el("analyzeBtn").addEventListener("click", () => {
      const text = el("lyrics").value.trim();
      if (!text){
        alert("先貼上歌詞再分析～");
        return;
      }
      render(analyze(text));
    });

    el("sampleBtn").addEventListener("click", () => {
      el("lyrics").value =
`我以為我會很快樂
卻在深夜想起你的溫柔
說再見的那一刻我好難過
但我仍期待明天有陽光`;
      render(analyze(el("lyrics").value));
    });

    el("clearBtn").addEventListener("click", () => {
      el("lyrics").value = "";
      el("labelText").textContent = "—";
      el("scoreText").textContent = "—";
      el("scoreLine").textContent = "—";
      el("posCount").textContent = "0";
      el("negCount").textContent = "0";
      el("bar").style.width = "50%";
      el("hitTags").innerHTML = "";
      el("moodText").textContent = "—";
    });
  </script>
</body>
</html>
