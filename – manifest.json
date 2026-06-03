// 発注データを {JAN:数} のオブジェクトで保持
let data = JSON.parse(localStorage.getItem('orderData')||'{}');
let buffer = '';

function showScreen(id){
document.querySelectorAll('.screen').forEach(s=>s.style.display='none');
document.getElementById(id).style.display='block';
if(id==='screen-scan') focusInput();
if(id==='screen-list') renderList();
if(id==='screen-export') renderExport();
}

function focusInput(){
const inp = document.getElementById('scanner-input');
buffer = ''; inp.value=''; inp.focus();
}

function renderStats(){
const skus = Object.keys(data).length;
const qty = Object.values(data).reduce((a,b)=>a+b,0);
document.getElementById('total-sku').textContent = skus;
document.getElementById('total-qty').textContent = qty;
}

document.getElementById('scanner-input')
.addEventListener('keydown', e=>{
if(e.key.match(/^[0-9]$/)) buffer+=e.key;
else if(e.key==='Backspace') buffer=buffer.slice(0,-1);
else if(e.key==='Enter'){
if(buffer.length>=8&&buffer.length<=13){
const jan=buffer;
data[jan]=(data[jan]||0)+1;
localStorage.setItem('orderData',JSON.stringify(data));
document.getElementById('last-jan').textContent=jan;
document.getElementById('last-count').textContent=data[jan];
renderStats();
}
buffer=''; e.target.value='';
}
});

function renderList(){
const tbody=document.querySelector('#table-list tbody');
tbody.innerHTML='';
Object.keys(data).sort().forEach(jan=>{
const tr=document.createElement('tr');
tr.innerHTML= <td>${jan}</td> <td>${data[jan]}</td> <td> <button class="plus">＋</button> <button class="minus">－</button> <button class="del">×</button> </td>;
tr.querySelector('.plus').onclick=()=>{data[jan]++;updateData()};
tr.querySelector('.minus').onclick=()=>{
if(--data[jan]<=0) delete data[jan]; updateData();
};
tr.querySelector('.del').onclick=()=>{delete data[jan];updateData()};
tbody.appendChild(tr);
});
renderStats();
}

function renderExport(){
const sku=Object.keys(data).length;
const qty=Object.values(data).reduce((a,b)=>a+b,0);
document.getElementById('exp-sku').textContent=sku;
document.getElementById('exp-qty').textContent=qty;
document.getElementById('csv-preview').textContent=generateCSV();
}

function updateData(){
localStorage.setItem('orderData',JSON.stringify(data));
renderList();
}

function generateCSV(){
const lines=['JAN,数量'];
for(let j in data) lines.push(${j},${data[j]});
return lines.join('\r\n');
}

document.getElementById('btn-export-csv')
.onclick=()=>{
const blob=new Blob([generateCSV()],{type:'text/csv'});
const url=URL.createObjectURL(blob);
const a=document.createElement('a');
a.href=url; a.download=order_${Date.now()}.csv; a.click();
URL.revokeObjectURL(url);
};

document.getElementById('btn-export-mail')
.onclick=()=>{
const body=encodeURIComponent(generateCSV());
location.href=mailto:?subject=発注データ&body=${body};
};

document.querySelectorAll('nav button')
.forEach(b=>b.onclick=()=>showScreen(b.getAttribute('data-show')));

// 初期表示
showScreen('screen-scan'); renderStats();
