<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>手搖飲 2026 年度戰報</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700;900&display=swap');
        body { font-family: 'Noto Sans TC', sans-serif; background-color: #000; color: #fff; -webkit-tap-highlight-color: transparent; }
        .animate-pulse-red { animation: pulse-red 2s infinite; }
        @keyframes pulse-red { 0%, 100% { filter: drop-shadow(0 0 2px rgba(220,38,38,0.4)); } 50% { filter: drop-shadow(0 0 10px rgba(220,38,38,0.8)); } }
    </style>
</head>
<body class="p-4 min-h-screen bg-black">
    <div class="max-w-md mx-auto">
        <div class="text-center mb-8 pt-6">
            <h1 class="text-4xl font-black text-red-600 tracking-tighter italic">DRINK REPORT</h1>
            <p class="text-[10px] text-zinc-500 font-bold tracking-[0.4em] uppercase mt-2">2026 個人手搖飲戰報</p>
        </div>
        <div class="grid grid-cols-2 gap-4 mb-6" id="stats-area"></div>
        <div class="bg-zinc-900/50 rounded-3xl p-4 border border-zinc-800">
            <div class="flex justify-between items-center mb-6">
                <button onclick="changeMonth(-1)" class="text-red-500 px-4 py-2"><i class="fas fa-chevron-left"></i></button>
                <span id="month-label" class="text-lg font-black tracking-widest uppercase text-white">2026 / 01</span>
                <button onclick="changeMonth(1)" class="text-red-500 px-4 py-2"><i class="fas fa-chevron-right"></i></button>
            </div>
            <div class="grid grid-cols-7 gap-1 text-center text-[10px] text-zinc-600 font-bold mb-2">
                <div>日</div><div>一</div><div>二</div><div>三</div><div>四</div><div>五</div><div>六</div>
            </div>
            <div id="calendar-grid" class="grid grid-cols-7 gap-1"></div>
        </div>
    </div>
    <div id="editor-modal" class="fixed inset-0 bg-black/95 hidden flex items-center justify-center p-4 z-50">
        <div class="bg-zinc-900 w-full max-w-sm rounded-[2rem] border border-red-900/30 overflow-hidden flex flex-col max-h-[85vh]">
            <div class="p-6 border-b border-zinc-800 flex justify-between items-center text-white">
                <h2 id="modal-date" class="text-red-600 font-black tracking-tight">日期</h2>
                <button onclick="closeModal()" class="text-zinc-500 text-2xl">&times;</button>
            </div>
            <div id="modal-body" class="p-6 overflow-y-auto flex-grow space-y-4"></div>
            <div class="p-6 bg-zinc-900">
                <button onclick="closeModal()" class="w-full py-4 bg-red-600 rounded-2xl font-black text-white shadow-lg active:scale-95 transition-all">儲存紀錄</button>
            </div>
        </div>
    </div>
    <script>
        let currentMonth = new Date(2026, 0, 1);
        let records = JSON.parse(localStorage.getItem('drinkData')) || {};
        let activeKey = '';
        function init() { renderStats(); renderCalendar(); }
        function save() { localStorage.setItem('drinkData', JSON.stringify(records)); renderStats(); renderCalendar(); }
        function changeMonth(d) { currentMonth.setMonth(currentMonth.getMonth() + d); renderCalendar(); }
        function renderStats() {
            const shops = {}, items = {}; let total = 0, spent = 0;
            Object.values(records).flat().forEach(r => { total++; spent += Number(r.price || 0); if (r.shop) shops[r.shop] = (shops[r.shop] || 0) + 1; if (r.item) items[r.item] = (items[r.item] || 0) + 1; });
            const getTop = (obj) => Object.entries(obj).sort((a,b)=>b[1]-a[1])[0] || ['-', 0];
            const topShop = getTop(shops); const topItem = getTop(items);
            document.getElementById('stats-area').innerHTML = `
                <div class="bg-zinc-900/40 p-5 rounded-3xl border border-zinc-800 animate-pulse-red text-center">
                    <div class="text-[10px] text-red-600 font-black uppercase mb-2">最強店家</div>
                    <div class="text-lg font-black truncate text-white">${topShop[0]}</div>
                    <div class="text-[10px] text-zinc-500 mt-1">${topShop[1]} 杯</div>
                </div>
                <div class="bg-zinc-900/40 p-5 rounded-3xl border border-zinc-800 animate-pulse-red text-center">
                    <div class="text-[10px] text-red-600 font-black uppercase mb-2">年度最愛</div>
                    <div class="text-lg font-black truncate text-white">${topItem[0]}</div>
                    <div class="text-[10px] text-zinc-500 mt-1">${topItem[1]} 次</div>
                </div>`;
        }
        function renderCalendar() {
            const y = currentMonth.getFullYear(), m = currentMonth.getMonth();
            document.getElementById('month-label').innerText = `${y} / ${(m+1).toString().padStart(2,'0')}`;
            const grid = document.getElementById('calendar-grid'); grid.innerHTML = '';
            const start = new Date(y, m, 1); start.setDate(start.getDate() - start.getDay());
            for(let i=0; i<35; i++){
                const d = new Date(start); const k = d.toISOString().split('T')[0];
                const isCurrent = d.getMonth() === m;
                const cell = document.createElement('div');
                cell.className = `h-16 flex flex-col items-center justify-center rounded-xl transition-all ${isCurrent?'bg-zinc-900/40':'opacity-0 pointer-events-none'}`;
                cell.onclick = () => isCurrent && openModal(k);
                const count = (records[k] || []).length;
                cell.innerHTML = `
                    <span class="text-[10px] font-bold ${count?'text-red-500':'text-zinc-700'}">${d.getDate()}</span>
                    ${count ? `<div class="mt-1 flex gap-0.5">${Array(count).fill('<div class="w-1 h-1 bg-red-600 rounded-full"></div>').join('')}</div>` : ''}`;
                grid.appendChild(cell); start.setDate(start.getDate()+1);
            }
        }
        function openModal(k) { activeKey = k; document.getElementById('editor-modal').classList.remove('hidden'); document.getElementById('modal-date').innerText = k; renderModalBody(); }
        function closeModal() { document.getElementById('editor-modal').classList.add('hidden'); save(); }
        function renderModalBody() {
            const rs = records[activeKey] || [];
            let html = rs.map((r, i) => `
                <div class="bg-black p-4 rounded-2xl border border-zinc-800 space-y-3 relative text-white">
                    <button onclick="delR(${i})" class="absolute right-4 top-4 text-zinc-700"><i class="fas fa-trash"></i></button>
                    <input type="text" placeholder="店家" value="${r.shop}" onchange="updR(${i},'shop',this.value)" class="w-full bg-transparent border-b border-zinc-800 py-2 outline-none focus:border-red-600 text-white">
                    <input type="text" placeholder="品項" value="${r.item}" onchange="updR(${i},'item',this.value)" class="w-full bg-transparent border-b border-zinc-800 py-2 outline-none focus:border-red-600 text-white">
                    <input type="number" placeholder="金額" value="${r.price}" onchange="updR(${i},'price',this.value)" class="w-full bg-transparent border-b border-zinc-800 py-2 outline-none focus:border-red-600 text-white">
                </div>`).join('');
            if(rs.length < 2) html += `<button onclick="addR()" class="w-full py-6 border-2 border-dashed border-zinc-800 rounded-2xl text-zinc-600 font-bold">+ 新增杯數</button>`;
            document.getElementById('modal-body').innerHTML = html;
        }
        function addR() { if(!records[activeKey]) records[activeKey]=[]; records[activeKey].push({shop:'',item:'',price:''}); renderModalBody(); }
        function updR(i,f,v) { records[activeKey][i][f]=v; }
        function delR(i) { records[activeKey].splice(i,1); if(!records[activeKey].length) delete records[activeKey]; renderModalBody(); }
        init();
    </script>
</body>
</html>

