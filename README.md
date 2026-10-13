<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#101827">
<title>My Finance AI — Starter</title>
<style>
:root{--bg:#0b1220;--panel:#111c2e;--panel2:#172338;--text:#edf3ff;--muted:#9aabc4;--line:#293750;--green:#35d39a;--red:#ff6b7a;--blue:#79aaff;--amber:#ffd166}
*{box-sizing:border-box}body{margin:0;background:linear-gradient(180deg,#0b1220,#0e1728);color:var(--text);font:15px system-ui,-apple-system,Segoe UI,Roboto,sans-serif}
.wrap{max-width:980px;margin:auto;padding:18px}.top{display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;margin-bottom:18px}.brand{font-size:23px;font-weight:800;letter-spacing:-.5px}.sub{color:var(--muted);font-size:12px;margin-top:3px}.tag{border:1px solid #37506d;border-radius:999px;padding:7px 11px;color:#b9d0ff;font-size:12px}
.notice{border:1px solid #6b5424;background:#2a2418;color:#ffe4a1;padding:12px 14px;border-radius:14px;margin-bottom:16px;font-size:13px;line-height:1.5}
.grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:12px}.card{background:rgba(17,28,46,.96);border:1px solid var(--line);border-radius:17px;padding:15px;min-width:0}.label{font-size:12px;color:var(--muted)}.big{font-size:25px;font-weight:800;margin-top:8px;overflow-wrap:anywhere}.small{font-size:12px;color:var(--muted);margin-top:5px}.section{margin-top:18px}.sectionhead{display:flex;justify-content:space-between;align-items:center;gap:10px;margin-bottom:10px}.section h2{font-size:17px;margin:0}.btn{border:0;border-radius:10px;background:#8bb4ff;color:#0a1424;font-weight:750;padding:10px 13px;cursor:pointer}.btn.secondary{background:#24344d;color:#eaf1ff;border:1px solid #3b4e6b}.btn.danger{background:#42212b;color:#ffd9df;border:1px solid #763544}.formgrid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:9px}.field{display:flex;flex-direction:column;gap:6px}.field label{font-size:12px;color:var(--muted)}input,select{width:100%;min-width:0;border-radius:10px;border:1px solid #344660;background:#0b1423;color:var(--text);padding:11px;font-size:14px}input::placeholder{color:#72839c}.formactions{display:flex;gap:8px;flex-wrap:wrap;margin-top:11px}.tablewrap{overflow-x:auto}table{border-collapse:collapse;width:100%;min-width:560px;font-size:13px}th,td{text-align:left;padding:11px 8px;border-bottom:1px solid var(--line);white-space:nowrap}th{color:var(--muted);font-weight:600}.pos{color:var(--green)}.neg{color:var(--red)}.empty{color:var(--muted);font-size:13px;padding:16px 0}.alertrow,.newsitem{padding:12px 0;border-bottom:1px solid var(--line);display:flex;justify-content:space-between;gap:12px;align-items:center}.alertrow:last-child,.newsitem:last-child{border-bottom:0}.newsitem a{color:#b8d1ff;text-decoration:none}.pill{font-size:11px;border-radius:7px;padding:4px 7px;background:#23324a;color:#bcd2f7}.two{display:grid;grid-template-columns:1fr 1fr;gap:12px}.foot{color:var(--muted);font-size:12px;line-height:1.6;margin:18px 2px}.chatline{display:flex;gap:8px}.chatline input{flex:1}.answer{background:#0b1423;border:1px solid var(--line);padding:12px;border-radius:12px;line-height:1.6;color:#d8e4f8;margin-top:10px;white-space:pre-wrap;display:none}
@media(max-width:650px){.wrap{padding:13px}.grid{grid-template-columns:1fr}.two{grid-template-columns:1fr}.formgrid{grid-template-columns:1fr 1fr}.big{font-size:23px}.brand{font-size:21px}}
</style>
</head>
<body>
<div class="wrap">
  <header class="top">
    <div><div class="brand">📈 My Finance AI</div><div class="sub">आपका निजी Stock Market & Portfolio Dashboard</div></div>
    <div class="tag">STARTER • ₹0 Prototype</div>
  </header>

  <div class="notice"><b>महत्वपूर्ण:</b> यह प्रोटोटाइप अभी लाइव NSE/BSE डेटा या Groww से जुड़ा नहीं है। शेयर और म्यूचुअल फंड की कीमतें आप खुद भरेंगे। API Key/Secret को इस सार्वजनिक वेबसाइट में न डालें।</div>

  <div class="grid">
    <div class="card"><div class="label">कुल निवेश</div><div class="big" id="invested">₹0.00</div><div class="small">खरीद मूल्य × मात्रा</div></div>
    <div class="card"><div class="label">वर्तमान वैल्यू</div><div class="big" id="value">₹0.00</div><div class="small">आपकी भरी हुई मौजूदा कीमतों से</div></div>
    <div class="card"><div class="label">कुल अनुमानित P&L</div><div class="big" id="pnl">₹0.00</div><div class="small" id="pnlpct">0.00% रिटर्न</div></div>
  </div>

  <section class="section card">
    <div class="sectionhead"><h2>➕ पोर्टफोलियो में शेयर जोड़ें</h2><span class="pill">मैन्युअल एंट्री</span></div>
    <form id="stockForm">
      <div class="formgrid">
        <div class="field"><label for="symbol">शेयर का नाम / Symbol</label><input id="symbol" placeholder="जैसे RELIANCE" required maxlength="30"></div>
        <div class="field"><label for="qty">मात्रा (Shares)</label><input id="qty" type="number" min="0.001" step="any" placeholder="10" required></div>
        <div class="field"><label for="buy">औसत खरीद मूल्य (₹)</label><input id="buy" type="number" min="0.01" step="any" placeholder="1250" required></div>
        <div class="field"><label for="current">मौजूदा कीमत (₹)</label><input id="current" type="number" min="0" step="any" placeholder="आज की कीमत" required></div>
        <div class="field"><label for="target">Target Alert (₹, वैकल्पिक)</label><input id="target" type="number" min="0" step="any" placeholder="जैसे 1400"></div>
        <div class="field"><label for="stop">Stop-Loss Alert (₹, वैकल्पिक)</label><input id="stop" type="number" min="0" step="any" placeholder="जैसे 1180"></div>
      </div>
      <div class="formactions"><button class="btn" type="submit">शेयर जोड़ें</button><button class="btn secondary" type="button" id="demo">Demo डेटा भरें</button></div>
    </form>
  </section>

  <section class="section card">
    <div class="sectionhead"><h2>💼 आपका पोर्टफोलियो</h2><button class="btn secondary" id="export">CSV Export</button></div>
    <div class="tablewrap"><table><thead><tr><th>शेयर</th><th>मात्रा</th><th>खरीद ₹</th><th>वर्तमान ₹</th><th>वैल्यू ₹</th><th>P&L ₹</th><th></th></tr></thead><tbody id="stocks"></tbody></table></div>
    <div id="empty" class="empty">अभी कोई शेयर नहीं है। ऊपर फॉर्म में शेयर जोड़ें या Demo डेटा भरें।</div>
  </section>

  <section class="section card">
    <div class="sectionhead"><h2>🪙 म्यूचुअल फंड पोर्टफोलियो</h2><span class="pill">मैन्युअल एंट्री • लाइव नहीं</span></div>
    <div class="grid" style="grid-template-columns:repeat(3,minmax(0,1fr));margin-bottom:14px">
      <div><div class="label">कुल फंड निवेश</div><div class="big" id="mfInvested">₹0.00</div></div>
      <div><div class="label">वर्तमान फंड मूल्य</div><div class="big" id="mfValue">₹0.00</div></div>
      <div><div class="label">फंड P&L</div><div class="big" id="mfPnl">₹0.00</div><div class="small" id="mfPct">0.00% रिटर्न</div></div>
    </div>
    <form id="mfForm">
      <div class="formgrid">
        <div class="field"><label for="mfName">म्यूचुअल फंड का नाम</label><input id="mfName" placeholder="जैसे SBI Contra Direct Plan Growth" required maxlength="100"></div>
        <div class="field"><label for="mfInvest">कुल निवेश (₹)</label><input id="mfInvest" type="number" min="0.01" step="any" placeholder="1000" required></div>
        <div class="field"><label for="mfCurrent">वर्तमान मूल्य (₹)</label><input id="mfCurrent" type="number" min="0" step="any" placeholder="965" required></div>
      </div>
      <div class="formactions"><button class="btn" type="submit">फंड जोड़ें</button><button class="btn secondary" type="button" id="mfDemo">मेरे स्क्रीनशॉट के Demo फंड भरें</button><button class="btn secondary" type="button" id="mfExport">फंड CSV Export</button></div>
    </form>
    <div class="tablewrap" style="margin-top:12px"><table style="min-width:480px"><thead><tr><th>फंड</th><th>निवेश ₹</th><th>वर्तमान मूल्य ₹</th><th>P&L ₹</th><th></th></tr></thead><tbody id="funds"></tbody></table></div>
    <div id="mfEmpty" class="empty">अभी कोई फंड नहीं जोड़ा गया है।</div>
    <div class="small">ये राशियाँ मैन्युअल/डेमो डेटा हैं। स्क्रीनशॉट में दिखे फंड के मूल्य पुरानी तारीख के हो सकते हैं; इन्हें आज का लाइव NAV न मानें।</div>
  </section>

  <div class="two section">
    <section class="card">
      <div class="sectionhead"><h2>🔔 Price Alerts</h2><span class="pill">कीमत अपडेट पर जाँच</span></div>
      <div class="small">अलर्ट तभी जाँचे जाएँगे जब आप मौजूदा कीमत अपडेट करेंगे। यह संस्करण बंद ब्राउज़र में लगातार लाइव निगरानी नहीं करता।</div>
      <div id="alerts"><div class="empty">Target या Stop-Loss सेट करने के बाद अलर्ट यहाँ दिखेंगे।</div></div>
    </section>
    <section class="card">
      <div class="sectionhead"><h2>📰 शेयर से जुड़ी खबरें</h2></div>
      <div class="field"><label for="newsSymbol">कंपनी / शेयर का नाम</label><input id="newsSymbol" placeholder="जैसे Reliance Industries"></div>
      <div class="formactions"><button class="btn" id="newsSearch" type="button">खबरें खोजें</button></div>
      <div class="small">यह Google News में खोज खोलेगा। समाचार लिंक/सारांश के लिए इंटरनेट चाहिए।</div>
    </section>
  </div>

  <section class="section card">
    <div class="sectionhead"><h2>🤖 Finance Helper (बेसिक)</h2><span class="pill">नियम-आधारित</span></div>
    <div class="small" style="margin-bottom:10px">यह अभी असली AI नहीं है। यह आपके पोर्टफोलियो के हिसाब से साधारण गणना और सारांश देता है।</div>
    <div class="chatline"><input id="question" placeholder="पूछें: मेरे पोर्टफोलियो का P&L क्या है?"><button class="btn" id="ask" type="button">पूछें</button></div>
    <div class="answer" id="answer"></div>
  </section>

  <div class="foot">सुरक्षा: यह प्रोटोटाइप डेटा इसी ब्राउज़र के Local Storage में रखता है। संवेदनशील जानकारी या Groww पासवर्ड/API टोकन यहाँ न डालें। यह निवेश सलाह नहीं है। वास्तविक Buy/Sell निर्णय से पहले स्वतंत्र जाँच करें। P&L में अभी ब्रोकरेज, टैक्स और अन्य शुल्क शामिल नहीं हैं।</div>
</div>
<script>
const $=id=>document.getElementById(id);
const key='myFinanceAIStarter_v1', mfKey='myFinanceAIFunds_v1';
let stocks=[], funds=[];
try{stocks=JSON.parse(localStorage.getItem(key)||'[]')}catch(e){stocks=[]}
try{funds=JSON.parse(localStorage.getItem(mfKey)||'[]')}catch(e){funds=[]}
function save(){localStorage.setItem(key,JSON.stringify(stocks));render()}
function saveFunds(){localStorage.setItem(mfKey,JSON.stringify(funds));renderFunds()}
function money(n){return Number(n||0).toLocaleString('en-IN',{style:'currency',currency:'INR',maximumFractionDigits:2})}
function fmt(n){return Number(n||0).toLocaleString('en-IN',{maximumFractionDigits:2})}
function esc(s){return String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
function render(){
 let inv=0,val=0;
 stocks.forEach(s=>{inv+=s.qty*s.buy;val+=s.qty*s.current});
 let pnl=val-inv,pct=inv?pnl/inv*100:0;
 $('invested').textContent=money(inv);$('value').textContent=money(val);$('pnl').textContent=money(pnl);
 $('pnl').className='big '+(pnl>0?'pos':pnl<0?'neg':'');$('pnlpct').textContent=fmt(pct)+'% रिटर्न';
 $('stocks').innerHTML=stocks.map((s,i)=>{let v=s.qty*s.current,p=v-s.qty*s.buy;return `<tr><td><b>${esc(s.symbol)}</b></td><td>${fmt(s.qty)}</td><td>${fmt(s.buy)}</td><td><input aria-label="Update current price for ${esc(s.symbol)}" data-price="${i}" type="number" min="0" step="any" value="${s.current}" style="width:105px;padding:7px"></td><td>${money(v)}</td><td class="${p>0?'pos':p<0?'neg':''}">${money(p)}</td><td><button class="btn danger" data-delete="${i}" style="padding:7px 9px">हटाएँ</button></td></tr>`}).join('');
 $('empty').style.display=stocks.length?'none':'block';
 $('alerts').innerHTML=stocks.filter(s=>s.target||s.stop).map(s=>{let state=[];if(s.target)state.push(`Target: ₹${fmt(s.target)} ${s.current>=s.target?'— लक्ष्य पार हुआ':''}`);if(s.stop)state.push(`Stop-Loss: ₹${fmt(s.stop)} ${s.current<=s.stop?'— स्तर छुआ/नीचे':''}`);return `<div class="alertrow"><div><b>${esc(s.symbol)}</b><div class="small">${state.join(' • ')}</div></div><span class="pill">${(s.target&&s.current>=s.target)||(s.stop&&s.current<=s.stop)?'जाँचें':'सेट'}</span></div>`}).join('')||'<div class="empty">Target या Stop-Loss सेट करने के बाद अलर्ट यहाँ दिखेंगे।</div>';
}
function renderFunds(){
 let inv=0,val=0;funds.forEach(f=>{inv+=f.invest;val+=f.current});let p=val-inv,pct=inv?p/inv*100:0;
 $('mfInvested').textContent=money(inv);$('mfValue').textContent=money(val);$('mfPnl').textContent=money(p);$('mfPnl').className='big '+(p>0?'pos':p<0?'neg':'');$('mfPct').textContent=fmt(pct)+'% रिटर्न';
 $('funds').innerHTML=funds.map((f,i)=>{let pl=f.current-f.invest;return `<tr><td><b>${esc(f.name)}</b></td><td>${money(f.invest)}</td><td><input aria-label="Update current value for ${esc(f.name)}" data-mf-price="${i}" type="number" min="0" step="any" value="${f.current}" style="width:115px;padding:7px"></td><td class="${pl>0?'pos':pl<0?'neg':''}">${money(pl)}</td><td><button class="btn danger" data-mf-delete="${i}" style="padding:7px 9px">हटाएँ</button></td></tr>`}).join('');$('mfEmpty').style.display=funds.length?'none':'block';
}
$('mfForm').addEventListener('submit',e=>{e.preventDefault();let f={name:$('mfName').value.trim(),invest:Number($('mfInvest').value),current:Number($('mfCurrent').value)};if(!f.name||f.invest<=0||f.current<0){alert('कृपया सही विवरण भरें।');return}funds.push(f);e.target.reset();saveFunds()});
$('funds').addEventListener('change',e=>{if(e.target.dataset.mfPrice!==undefined){let i=Number(e.target.dataset.mfPrice),v=Number(e.target.value);if(v>=0&&funds[i]){funds[i].current=v;saveFunds()}}});
$('funds').addEventListener('click',e=>{if(e.target.dataset.mfDelete!==undefined){funds.splice(Number(e.target.dataset.mfDelete),1);saveFunds()}});
$('mfDemo').addEventListener('click',()=>{funds=[{name:'HDFC Retirement Fund Equity Plan Direct Growth',invest:2005,current:1900},{name:'Nippon India Silver ETF FoF Direct Growth',invest:1500,current:1472},{name:'SBI Contra Direct Plan Growth',invest:1000,current:965}];saveFunds()});
$('mfExport').addEventListener('click',()=>{let rows=[['Fund','Invested','Current Value','P&L'],...funds.map(f=>[f.name,f.invest,f.current,f.current-f.invest])];let csv=rows.map(r=>r.map(x=>'"'+String(x??'').replace(/"/g,'""')+'"').join(',')).join('\r\n');let a=document.createElement('a');a.href=URL.createObjectURL(new Blob(['\ufeff'+csv],{type:'text/csv;charset=utf-8;'}));a.download='my-finance-mutual-funds.csv';a.click();URL.revokeObjectURL(a.href)});
$('stockForm').addEventListener('submit',e=>{e.preventDefault();let s={symbol:$('symbol').value.trim().toUpperCase(),qty:Number($('qty').value),buy:Number($('buy').value),current:Number($('current').value),target:Number($('target').value)||0,stop:Number($('stop').value)||0};if(!s.symbol||s.qty<=0||s.buy<=0||s.current<0){alert('कृपया सही विवरण भरें।');return}stocks.push(s);e.target.reset();save()});
$('stocks').addEventListener('change',e=>{if(e.target.dataset.price!==undefined){let i=Number(e.target.dataset.price),v=Number(e.target.value);if(v>=0&&stocks[i]){stocks[i].current=v;save();if((stocks[i].target&&v>=stocks[i].target)||(stocks[i].stop&&v<=stocks[i].stop)){alert('Price alert: '+stocks[i].symbol+' की दर्ज कीमत ₹'+fmt(v)+' आपके Target/Stop-Loss स्तर पर पहुँची है।')}}}});
$('stocks').addEventListener('click',e=>{if(e.target.dataset.delete!==undefined){stocks.splice(Number(e.target.dataset.delete),1);save()}});
$('demo').addEventListener('click',()=>{stocks=[{symbol:'RELIANCE',qty:2,buy:1200,current:1240,target:1300,stop:1150},{symbol:'TCS',qty:1,buy:3500,current:3420,target:3700,stop:3300}];save()});
$('newsSearch').addEventListener('click',()=>{let q=$('newsSymbol').value.trim();if(!q){alert('पहले कंपनी/शेयर का नाम लिखें।');return}window.open('https://news.google.com/search?q='+encodeURIComponent(q+' stock India'),'_blank','noopener')});
$('ask').addEventListener('click',()=>{let q=$('question').value.trim().toLowerCase(),inv=0,val=0;stocks.forEach(s=>{inv+=s.qty*s.buy;val+=s.qty*s.current});let p=val-inv;let msg;if(!stocks.length)msg='पहले पोर्टफोलियो में शेयर जोड़ें। फिर मैं कुल निवेश और अनुमानित P&L की गणना कर सकता हूँ।';else if(q.includes('नुकसान')||q.includes('loss')||q.includes('सबसे ज्यादा')){let ranked=stocks.map(s=>({s,p:(s.current-s.buy)*s.qty})).sort((a,b)=>a.p-b.p);msg='सबसे कमजोर P&L: '+ranked[0].s.symbol+' ('+money(ranked[0].p)+'). यह केवल दर्ज कीमतों पर आधारित गणना है।';}else if(q.includes('निवेश')||q.includes('invest'))msg='कुल दर्ज निवेश: '+money(inv)+'. वर्तमान वैल्यू: '+money(val)+'. अनुमानित P&L: '+money(p)+'.';else msg='आपके '+stocks.length+' शेयरों का कुल निवेश '+money(inv)+', वर्तमान वैल्यू '+money(val)+' और अनुमानित P&L '+money(p)+' ('+(inv?(p/inv*100).toFixed(2):'0.00')+'%) है। कीमतें मैन्युअल रूप से अपडेट की गई हैं; यह लाइव डेटा नहीं है।';$('answer').style.display='block';$('answer').textContent=msg});
$('export').addEventListener('click',()=>{let rows=[['Symbol','Quantity','Buy Price','Current Price','Value','P&L','Target','Stop Loss'],...stocks.map(s=>[s.symbol,s.qty,s.buy,s.current,s.qty*s.current,s.qty*(s.current-s.buy),s.target,s.stop])];let csv=rows.map(r=>r.map(x=>'"'+String(x??'').replace(/"/g,'""')+'"').join(',')).join('\r\n');let a=document.createElement('a');a.href=URL.createObjectURL(new Blob(['\ufeff'+csv],{type:'text/csv;charset=utf-8;'}));a.download='my-finance-portfolio.csv';a.click();URL.revokeObjectURL(a.href)});
render();renderFunds();
</script>
</body></html>