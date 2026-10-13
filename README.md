<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Shamilov Exchange</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
  :root{
    --bg:#0B1210;            /* фон приложения */
    --card:#12201B;          /* карточки */
    --brand:#183630;         /* фирменный зелёный */
    --accent:#35D99A;        /* яркий акцент (как зелёный Apple) */
    --accent-soft:rgba(53,217,154,.14);
    --text:#F2F7F5;
    --text2:rgba(242,247,245,.6);
    --text3:rgba(242,247,245,.38);
    --border:rgba(242,247,245,.09);
    --radius:18px;
  }
  *{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
  html,body{background:var(--bg);color:var(--text);
    font-family:-apple-system,BlinkMacSystemFont,"SF Pro Text","Segoe UI",Roboto,sans-serif}
  button{font:inherit;color:inherit;background:none;border:none;cursor:pointer}

  .wrap{max-width:430px;margin:0 auto;min-height:100vh;display:flex;flex-direction:column}

  /* ---------- Шапка ---------- */
  header{padding:20px 20px 8px;display:flex;align-items:center;gap:12px}
  .logo{width:44px;height:44px;border-radius:14px;background:var(--brand);
    display:flex;align-items:center;justify-content:center;font-size:20px;font-weight:600;color:var(--accent)}
  .h-title{font-size:17px;font-weight:600}
  .h-sub{font-size:13px;color:var(--text2);display:flex;align-items:center;gap:6px}
  .dot-live{width:7px;height:7px;border-radius:50%;background:var(--accent);box-shadow:0 0 8px var(--accent)}

  main{flex:1;padding:12px 20px 110px}
  .page{display:none;animation:fade .25s ease}
  .page.active{display:block}
  @keyframes fade{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}

  /* ---------- Карточки курсов ---------- */
  .rate-card{background:linear-gradient(160deg,var(--brand),#0F2922);
    border:1px solid rgba(53,217,154,.18);border-radius:22px;padding:22px;margin-bottom:14px}
  .rate-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:14px}
  .rate-pair{font-size:15px;font-weight:600;display:flex;align-items:center;gap:8px}
  .badge{font-size:12px;color:var(--accent);background:var(--accent-soft);
    padding:5px 10px;border-radius:99px;display:flex;align-items:center;gap:6px}
  .rate-value{display:flex;align-items:baseline;gap:8px}
  .rate-value b{font-size:46px;font-weight:600;letter-spacing:.5px;font-variant-numeric:tabular-nums}
  .rate-value span{font-size:16px;color:var(--text2)}
  .rate-note{font-size:13px;color:var(--text2);margin-top:8px}
  .usdt-card{background:var(--card);border:1px solid var(--border);border-radius:18px;
    padding:18px;margin-bottom:14px;display:flex;align-items:center;justify-content:space-between;gap:12px}
  .usdt-card .t{font-size:15px;font-weight:600}
  .usdt-card .s{font-size:13px;color:var(--text2);margin-top:3px}
  .btn-ghost{border:1px solid rgba(53,217,154,.4);color:var(--accent);
    padding:10px 14px;border-radius:12px;font-size:14px;font-weight:500;white-space:nowrap}

  /* ---------- Калькулятор ---------- */
  .calc{background:var(--card);border:1px solid var(--border);border-radius:18px;padding:18px;margin-bottom:14px}
  .calc h3{font-size:14px;color:var(--text2);font-weight:500;margin-bottom:12px}
  .calc-row{display:flex;align-items:center;gap:10px;background:rgba(255,255,255,.04);
    border:1px solid var(--border);border-radius:14px;padding:14px 16px}
  .calc-row input{flex:1;background:none;border:none;outline:none;color:var(--text);
    font-size:20px;font-weight:600;font-variant-numeric:tabular-nums;min-width:0}
  .calc-row .cur{font-size:16px;color:var(--text2)}
  .calc-arrow{text-align:center;color:var(--text3);margin:8px 0;font-size:18px}
  .calc-out{display:flex;align-items:baseline;gap:8px;padding:4px 4px 0}
  .calc-out b{font-size:30px;font-weight:600;color:var(--accent);font-variant-numeric:tabular-nums}
  .calc-out span{color:var(--text2);font-size:15px}

  /* ---------- Кнопки ---------- */
  .btn-primary{width:100%;background:var(--accent);color:#06231A;font-size:16px;font-weight:600;
    padding:17px;border-radius:16px;display:flex;align-items:center;justify-content:center;gap:8px;
    transition:transform .12s ease}
  .btn-primary:active{transform:scale(.97)}

  /* ---------- О нас ---------- */
  .about-card{background:var(--card);border:1px solid var(--border);border-radius:18px;padding:20px;margin-bottom:14px}
  .about-head{display:flex;gap:14px;align-items:center;margin-bottom:6px}
  .avatar{width:56px;height:56px;border-radius:18px;background:var(--brand);
    display:flex;align-items:center;justify-content:center;font-size:24px;font-weight:600;color:var(--accent)}
  .verified{color:var(--accent);display:flex;align-items:center;gap:5px;font-size:13px;font-weight:500}
  .insta{margin-top:14px;display:flex;align-items:center;gap:10px;background:rgba(255,255,255,.04);
    border:1px solid var(--border);border-radius:14px;padding:13px 15px;text-decoration:none;color:var(--text);font-size:14px;font-weight:500}
  .svc{display:flex;gap:14px;align-items:flex-start;padding:14px 0;border-bottom:1px solid var(--border)}
  .svc:last-child{border-bottom:none}
  .svc-ic{width:38px;height:38px;border-radius:12px;background:var(--accent-soft);color:var(--accent);
    display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:17px}
  .svc .t{font-size:15px;font-weight:500}
  .svc .s{font-size:13px;color:var(--text2);margin-top:2px}

  /* ---------- Заявки ---------- */
  .stat{background:linear-gradient(160deg,var(--brand),#0F2922);border:1px solid rgba(53,217,154,.18);
    border-radius:18px;padding:18px 20px;margin-bottom:14px}
  .stat .l{font-size:13px;color:var(--text2)}
  .stat .v{font-size:28px;font-weight:600;margin-top:4px;font-variant-numeric:tabular-nums}
  .order{background:var(--card);border:1px solid var(--border);border-radius:16px;
    padding:15px 16px;margin-bottom:10px;display:flex;justify-content:space-between;align-items:center;gap:10px}
  .order .pair{font-size:15px;font-weight:500}
  .order .meta{font-size:12px;color:var(--text3);margin-top:3px}
  .order .sum{font-size:15px;font-weight:600;text-align:right;font-variant-numeric:tabular-nums}
  .pill{font-size:11px;padding:4px 9px;border-radius:99px;display:inline-block;margin-top:4px}
  .pill.done{background:var(--accent-soft);color:var(--accent)}
  .pill.work{background:rgba(255,196,87,.14);color:#FFC457}
  .empty{text-align:center;color:var(--text3);font-size:14px;padding:40px 0}

  /* ---------- Таббар ---------- */
  nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:430px;
    background:rgba(11,18,16,.92);backdrop-filter:blur(20px);border-top:1px solid var(--border);
    display:flex;padding:10px 8px calc(10px + env(safe-area-inset-bottom))}
  .tab{flex:1;display:flex;flex-direction:column;align-items:center;gap:4px;
    color:var(--text3);font-size:11px;font-weight:500;padding:6px 0;border-radius:12px;transition:color .15s}
  .tab.active{color:var(--accent)}
  .tab svg{width:23px;height:23px}

  /* ---------- Онбординг ---------- */
  #onb{position:fixed;inset:0;z-index:50;background:var(--bg);max-width:430px;margin:0 auto;
    display:flex;flex-direction:column;padding:24px;transition:opacity .3s}
  #onb.hide{opacity:0;pointer-events:none}
  .skip{align-self:flex-end;color:var(--text3);font-size:14px;padding:8px}
  .onb-body{flex:1;display:flex;flex-direction:column;justify-content:center;align-items:center;text-align:center;gap:20px}
  .onb-ic{width:96px;height:96px;border-radius:28px;background:var(--brand);display:flex;align-items:center;justify-content:center}
  .onb-ic svg{width:44px;height:44px;color:var(--accent)}
  .onb-t{font-size:24px;font-weight:600}
  .onb-s{font-size:15px;color:var(--text2);line-height:1.5;max-width:280px}
  .dots{display:flex;gap:8px;justify-content:center;margin:20px 0}
  .dots i{width:8px;height:8px;border-radius:99px;background:var(--text3);transition:all .25s}
  .dots i.on{width:24px;background:var(--accent)}
  #onbNext{width:100%}
  .slide{display:none;flex-direction:column;align-items:center;gap:20px}
  .slide.on{display:flex}
</style>
<base target="_blank">
</head>
<body>

<!-- ======= ОНБОРДИНГ (показывается при первом запуске) ======= -->
<div id="onb">
  <button class="skip" onclick="finishOnb()">Пропустить</button>
  <div class="onb-body">
    <div class="slide on" id="s0">
      <div class="onb-ic">📈</div>
      <div class="onb-t">Актуальные курсы каждый день</div>
      <div class="onb-s">Курс ₽ → ¥ обновляется ежедневно. USDT — по договорённости с менеджером.</div>
    </div>
    <div class="slide" id="s1">
      <div class="onb-ic">🧾</div>
      <div class="onb-t">Заявка за минуту</div>
      <div class="onb-s">Введите сумму в калькуляторе и нажмите «Обменять» — заявка уйдёт менеджеру.</div>
    </div>
    <div class="slide" id="s2">
      <div class="onb-ic">🛡️</div>
      <div class="onb-t">Подтверждение менеджером</div>
      <div class="onb-s">Менеджер проверит детали и проведёт обмен. Все заявки сохраняются во вкладке «Заявки».</div>
    </div>
  </div>
  <div class="dots" id="dots"><i class="on"></i><i></i><i></i></div>
  <button class="btn-primary" id="onbNext" onclick="nextSlide()">Далее</button>
</div>

<div class="wrap">
  <header>
    <div class="logo">S</div>
    <div>
      <div class="h-title">Shamilov Exchange</div>
      <div class="h-sub"><span class="dot-live"></span><span id="updTxt">обновлено сегодня</span></div>
    </div>
  </header>

  <main>
    <!-- ======= КУРС ======= -->
    <section class="page active" id="p-rates">
      <div class="rate-card">
        <div class="rate-top">
          <div class="rate-pair">🇷🇺 ₽ → 🇨🇳 ¥ · Юань</div>
          <div class="badge">сегодня</div>
        </div>
        <div class="rate-value"><b id="rateVal">13.5</b><span>₽ за 1 ¥</span></div>
        <div class="rate-note" id="rateDate"></div>
      </div>

      <div class="usdt-card">
        <div>
          <div class="t">USDT · по договору</div>
          <div class="s">Курс фиксируется индивидуально с менеджером</div>
        </div>
        <button class="btn-ghost" onclick="toManager('USDT')">Уточнить курс</button>
      </div>

      <div class="calc">
        <h3>Калькулятор обмена</h3>
        <div class="calc-row"><input id="inRub" type="number" inputmode="decimal" placeholder="0" oninput="calc()"><span class="cur">₽</span></div>
        <div class="calc-arrow">↓</div>
        <div class="calc-out"><b id="outCny">0</b><span>¥ юаней</span></div>
      </div>

      <button class="btn-primary" onclick="createOrder()">Обменять ₽ → ¥</button>
    </section>

    <!-- ======= О НАС ======= -->
    <section class="page" id="p-about">
      <div class="about-card">
        <div class="about-head">
          <div class="avatar">S</div>
          <div>
            <div style="font-size:17px;font-weight:600">Shamilov Exchange</div>
            <div class="verified">✓ Доверенная компания · 4+ в команде</div>
          </div>
        </div>
        <a class="insta" href="https://instagram.com/shamilov_muhammadali" onclick="tgOpen(event,this.href)">📸 Instagram — @shamilov_muhammadali</a>
      </div>

      <div class="about-card">
        <div class="svc"><div class="svc-ic">🏢</div><div><div class="t">Переводы компания → компания</div><div class="s">Официальные платежи между юрлицами</div></div></div>
        <div class="svc"><div class="svc-ic">💵</div><div><div class="t">Наличные в Китае</div><div class="s">Выдача наличных юаней в городах Китая</div></div></div>
        <div class="svc"><div class="svc-ic">🏭</div><div><div class="t">Передача фабрике</div><div class="s">Оплата поставщику в любом городе Китая</div></div></div>
        <div class="svc"><div class="svc-ic">🏦</div><div><div class="t">Банковские переводы</div><div class="s">Переводы на банковские карты и счета</div></div></div>
      </div>
      <button class="btn-primary" onclick="toManager('консультация')">Связаться с менеджером</button>
    </section>

    <!-- ======= ЗАЯВКИ ======= -->
    <section class="page" id="p-orders">
      <div class="stat">
        <div class="l">Всего обменено</div>
        <div class="v" id="totalVal">0 ¥</div>
      </div>
      <div id="ordersList"></div>
    </section>
  </main>

  <nav>
    <button class="tab active" onclick="go('rates',this)">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M4 3.08a1 1 0 0 1 1 1V18.04h15l.1.005a1 1 0 0 1 0 2L5 20.04a2 2 0 0 1-2-2V4.08a1 1 0 0 1 1-1Zm14.26 3.44a1 1 0 0 1 1.49 1.34l-3.88 4.32a2.36 2.36 0 0 1-3.34.22l-2.2-1.8-3.46 4.06a1 1 0 1 1-1.52-1.3l3.88-4.53a2.36 2.36 0 0 1 3.35-.26l2.2 1.8 3.88-4.45Z"/></svg>
      Курс</button>
    <button class="tab" onclick="go('about',this)">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M19.2 9.38a2.64 2.64 0 0 1-1.95 3.18l-1.55 5.8a2.64 2.64 0 0 1-3.18 1.95l-4.91-1.32 3.1-.83a2.6 2.6 0 0 0 .34-.12l1.94.52a1 1 0 0 0 1.24-.7l1.55-5.79a1 1 0 0 0-.7-1.24l-3.68-.99-.54-2 4.68 1.25ZM10.18 6.6a2.64 2.64 0 0 1 3.18 1.95l1.55 5.8a2.64 2.64 0 0 1-1.95 3.18l-5.8 1.55a2.64 2.64 0 0 1-3.18-1.95l-1.55-5.8a2.64 2.64 0 0 1 1.95-3.18l5.8-1.55Zm1.8 2.52a1 1 0 0 0-1.24-.7L4.85 9.9a1 1 0 0 0-.7 1.23l1.55 5.8a1 1 0 0 0 1.24.7l5.8-1.55a1 1 0 0 0 .7-1.24l-1.55-5.8ZM15 3.53h-6a2.9 2.9 0 0 0-2.89 2.9v.33l-1.81.49v-.82a2.9 2.9 0 0 1 2.9-2.9h6a2.9 2.9 0 0 1 2.9 2.9v1.16l-1.8-.48V6.43A2.9 2.9 0 0 0 15 3.53Z"/></svg>
      О нас</button>
    <button class="tab" onclick="go('orders',this)">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M9.29 4.93a.9.9 0 1 0-1.27-1.27L5.9 5.73l-.75-.77a.9.9 0 1 0-1.29 1.25l1.38 1.43a.9.9 0 0 0 1.28.01l2.76-2.72ZM12 5.1a.9.9 0 1 0 0 1.8h8a.9.9 0 1 0 0-1.8h-8Zm0 6a.9.9 0 1 0 0 1.8h8a.9.9 0 1 0 0-1.8h-8Zm-.9 6.9a.9.9 0 1 1 0-1.8h8a.9.9 0 1 1 0 1.8h-8ZM9.3 9.77a.9.9 0 1 0-1.27 1.27l2.76 2.73a.9.9 0 0 0 1.28 0l1.38-1.42a.9.9 0 1 0-1.29-1.26l-.75.77-2.11-2.09Zm0 5.97a.9.9 0 1 0-1.27 1.27l2.76 2.73a.9.9 0 0 0 1.28 0l1.38-1.42a.9.9 0 1 0-1.29-1.26l-.75.77-2.11-2.09Z"/></svg>
      Заявки</button>
  </nav>
</div>

<script>
/* ================= НАСТРОЙКИ ================= */
const MANAGER = "shamllovv";                // <-- менеджер, которому уходят заявки
const RATE_KEY = "se_rate", ORD_KEY = "se_orders", ONB_KEY = "se_onb";
const ADMIN_IDS = [7822390789];             // <-- твой Telegram ID: менять курс прямо из приложения

const tg = window.Telegram?.WebApp;
tg?.ready(); tg?.expand();
tg?.setHeaderColor("#0B1210"); tg?.setBackgroundColor("#0B1210");

/* ================= КУРС ================= */
let rate = parseFloat(localStorage.getItem(RATE_KEY)) || 13.5;
function isAdmin(){ return tg && ADMIN_IDS.includes(tg.initDataUnsafe?.user?.id); }
function renderRate(){
  document.getElementById("rateVal").textContent = rate;
  document.getElementById("rateDate").textContent =
    "Обновлено: " + new Date().toLocaleDateString("ru-RU",{day:"numeric",month:"long"});
  if(isAdmin()){
    const c = document.querySelector(".rate-card");
    c.style.cursor = "pointer";
    c.title = "Нажмите, чтобы изменить курс";
    c.onclick = () => {
      const v = prompt("Новый курс (₽ за 1 ¥):", rate);
      if(v && !isNaN(parseFloat(v))){ rate = parseFloat(v);
        localStorage.setItem(RATE_KEY, rate); renderRate(); calc(); }
    };
  }
}

/* ================= КАЛЬКУЛЯТОР ================= */
function calc(){
  const rub = parseFloat(document.getElementById("inRub").value) || 0;
  document.getElementById("outCny").textContent = (rub / rate).toLocaleString("ru-RU",{maximumFractionDigits:2});
}

/* ================= ЗАЯВКИ ================= */
function getOrders(){ return JSON.parse(localStorage.getItem(ORD_KEY) || "[]"); }
function renderOrders(){
  const list = getOrders(), box = document.getElementById("ordersList");
  const total = list.reduce((s,o)=>s+o.cny,0);
  document.getElementById("totalVal").textContent = total.toLocaleString("ru-RU",{maximumFractionDigits:0}) + " ¥";
  if(!list.length){ box.innerHTML = '<div class="empty">Пока нет заявок.<br>Создайте первую на вкладке «Курс».</div>'; return; }
  box.innerHTML = list.map(o => `
    <div class="order">
      <div><div class="pair">₽ → ¥</div><div class="meta">${o.date}</div></div>
      <div><div class="sum">${o.cny.toLocaleString("ru-RU")} ¥</div>
      <span class="pill ${o.status==="done"?"done":"work"}">${o.status==="done"?"Выполнено":"В работе"}</span></div>
    </div>`).reverse().join("");
}
function createOrder(){
  const rub = parseFloat(document.getElementById("inRub").value) || 0;
  if(rub <= 0){ tg?.HapticFeedback?.notificationOccurred("error"); return; }
  const cny = +(rub / rate).toFixed(2);
  const orders = getOrders();
  orders.push({ rub, cny, rate, date: new Date().toLocaleString("ru-RU",{day:"numeric",month:"short",hour:"2-digit",minute:"2-digit"}), status: "work" });
  localStorage.setItem(ORD_KEY, JSON.stringify(orders));
  tg?.HapticFeedback?.notificationOccurred("success");
  renderOrders(); go("orders", document.querySelectorAll(".tab")[2]);
  toManager("заявка", `Хочу обменять ${rub.toLocaleString("ru-RU")} ₽ → ${cny.toLocaleString("ru-RU")} ¥ по курсу ${rate}`);
}

/* ================= ПЕРЕХОД К МЕНЕДЖЕРУ ================= */
function toManager(topic, text){
  const msg = encodeURIComponent(text || `Здравствуйте! Интересует обмен: ${topic}. Курс ₽→¥ сегодня ${rate}.`);
  const url = `https://t.me/${MANAGER}?start=${encodeURIComponent(topic)}`;
  tg?.openTelegramLink(url) || window.open(url, "_blank");
  console.log("Заявка менеджеру:", msg);
}
function tgOpen(e, url){ e.preventDefault(); tg?.openLink(url) || window.open(url,"_blank"); }

/* ================= НАВИГАЦИЯ / ОНБОРДИНГ ================= */
function go(p, btn){
  document.querySelectorAll(".page").forEach(x=>x.classList.remove("active"));
  document.getElementById("p-"+p).classList.add("active");
  document.querySelectorAll(".tab").forEach(t=>t.classList.remove("active"));
  btn.classList.add("active");
  tg?.HapticFeedback?.selectionChanged();
}
let slide = 0;
function nextSlide(){
  if(slide < 2){
    slide++;
    document.querySelectorAll(".slide").forEach((s,i)=>s.classList.toggle("on", i===slide));
    document.querySelectorAll("#dots i").forEach((d,i)=>d.classList.toggle("on", i===slide));
    document.getElementById("onbNext").textContent = slide===2 ? "Начать" : "Далее";
  } else finishOnb();
}
function finishOnb(){
  localStorage.setItem(ONB_KEY,"1");
  document.getElementById("onb").classList.add("hide");
}

renderRate(); calc(); renderOrders();
if(localStorage.getItem(ONB_KEY)) document.getElementById("onb").classList.add("hide");
</script>
</body>
</html>
