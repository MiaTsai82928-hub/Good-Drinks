<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>我超愛喝手搖 - 2026 年度戰報</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700;900&display=swap');
        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: #000;
            color: #fee2e2;
            -webkit-tap-highlight-color: transparent;
        }
        /* 呼吸閃爍動畫 */
        .animate-pulse-honor {
            animation: honor-pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
        @keyframes honor-pulse {
            0%, 100% { opacity: 1; transform: scale(1); filter: brightness(1.2); }
            50% { opacity: 0.7; transform: scale(0.97); filter: brightness(0.8); }
        }
        .custom-scrollbar::-webkit-scrollbar { width: 4px; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #7f1d1d; border-radius: 10px; }
    </style>
</head>
<body class="p-3 min-h-screen pb-10">

    <div class="max-w-4xl mx-auto">
        <!-- 標題 -->
        <div class="text-center mb-6 pt-4">
            <h1 class="text-3xl font-black text-red-600 tracking-tighter flex items-center justify-center gap-2 drop-shadow-[0_0_10px_rgba(220,38,38,0.5)] uppercase">
                <i class="fas fa-beer"></i> 我超愛喝手搖
            </h1>
            <p class="text-[9px] text-zinc-600 font-bold tracking-[0.3em] uppercase mt-1">2026 Beverage Tracker</p>
        </div>

        <!-- 榮譽榜 -->
        <div class="grid grid-cols-2 gap-3 mb-4" id="stats-container">
            <!-- 由 JavaScript 動態生成 -->
        </div>

        <!-- 數據概覽 -->
        <div class="grid grid-cols-3 gap-2 mb-6" id="overview-container">
            <!-- 由 JavaScript 動態生成 -->
        </div>

        <!-- 月份控制 -->
        <div class="flex justify-center mb-4">
            <div class="flex items-center gap-4 bg-zinc-900 px-5 py-2 rounded-full border border-red-900/40">
                <button onclick="changeMonth(-1)" class="text-red-500 p-2"><i class="fas fa-chevron-left"></i></button>
                <span id="current-month-display" class="text-sm font-bold text-red-100 min-w-[100px] text-center">2026年1月</span>
                <button onclick="changeMonth(1)" class="text-red-500 p-2"><i class="fas fa-chevron-right"></i></button>
            </div>
        </div>

        <!-- 日曆標題 -->
        <div class="grid grid-cols-7 mb-1 text-center text-red-900/50 text-[10px] font-bold">
            <div>日</div><div>一</div><div>二</div><div>三</div><div>四</div><div>五</div><div>六</div>
        </div>

        <!-- 日曆格 -->
        <div id="calendar-grid" class="rounded-xl overflow-hidden border border-zinc-900 shadow-xl grid grid-cols-7 bg-zinc-900/10">
            <!-- 由 JavaScript 動態生成 -->
        </div>
    </div>

    <!-- 編輯彈窗 -->
    <div id="modal" class="fixed inset-0 bg-black/95 backdrop-blur-md hidden flex items-center justify-center p-4 z-50">
        <div class="bg-zinc-950 rounded-2xl w-full max-w-sm border border-red-900/40 overflow-hidden flex flex-col max-h-[85vh]">
            <div class="bg-red-800 p-4 text-white flex justify-between items-center">
                <h2 id="modal-date-title" class="font-bold text-sm tracking-widest">日期</h2>
                <button onclick="closeModal()"><i class="fas fa-times text-xl"></i></button>
            </div>
            <div id="modal-content" class="p-4 overflow-y-auto custom-scrollbar flex-grow">
                <!-- 內容 -->
            </div>
            <div class="p-4 bg-zinc-950 border-t border-red-900/20">
                <button onclick="closeModal()" class="w-full py-3 bg-red-600 text-white rounded-xl font-bold active:scale-95 transition-all">儲存並返回</button>
            </div>
        </div>
    </div>

    <script>
        let currentMonth = new Date(2026, 0, 1);
        let records = JSON.parse(localStorage.getItem('drinkRecords')) || {};
        let selectedDateKey = '';

        const iceOpts = ['正常', '少冰', '微冰', '去冰', '常溫', '溫'];
        const sugarOpts = ['全糖', '少糖', '半糖', '微糖', '一分糖', '無糖'];

        function init() { renderStats(); renderCalendar(); }
        function save() { localStorage.setItem('drinkRecords', JSON.stringify(records)); renderStats(); renderCalendar(); }
        function changeMonth(d) { currentMonth.setMonth(currentMonth.getMonth() + d); renderCalendar(); }

        function renderStats() {
            const shops = {}, items = {};
            let total = 0, spent = 0;
            Object.values(records).flat().forEach(r => {
                total++; spent += Number(r.price || 0);
                if (r.shop) shops[r.shop] = (shops[r.shop] || 0) + 1;
                if (r.item) items[r.item] = (items[r.item] || 0) + 1;
            });
            const top = (obj) => Object.entries(obj).sort((a,b)=>b[1]-a[1]).slice(0,3);
            const ts = top(shops), ti = top(items);

            document.getElementById('stats-container').innerHTML = `
                <div class="bg-zinc-900/40 border border-red-900/20 rounded-xl p-3 flex flex-col min-h-[160px]">
                    <span class="text-[10px] font-bold text-red-500 mb-2 uppercase tracking-tight"><i class="fas fa-trophy mr-1"></i>店家排行</span>
                    <div class="flex-grow space-y-2">
                        ${ts.map(([n,c],i)=>`<div class="text-[10px] flex justify-between"><span class="truncate pr-1 ${i===0?'text-red-400 font-bold':''}">${i+1}.${n}</span><span class="text-red-500 font-bold">${c}杯</span></div>`).join('') || '<div class="text-[10px] text-zinc-800">無數據</div>'}
                    </div>
                    ${ts[0] ? `<div class="mt-2 pt-2 border-t border-red-900/10 text-center animate-pulse-honor"><span class="text-xs font-bold text-red-400">🥇 冠軍店家：${ts[0][0]}</span></div>` : ''}
                </div>
                <div class="bg-zinc-900/40 border border-red-900/20 rounded-xl p-3 flex flex-col min-h-[160px]">
                    <span class="text-[10px] font-bold text-red-500 mb-2 uppercase tracking-tight"><i class="fas fa-star mr-1"></i>品項排行</span>
                    <div class="flex-grow space-y-2">
                        ${ti.map(([n,c],i)=>`<div class="text-[10px] flex justify-between"><span class="truncate pr-1 ${i===0?'text-red-300 font-bold':''}">${i+1}.${n}</span><span class="text-red-400 font-bold">${c}次</span></div>`).join('') || '<div class="text-[10px] text-zinc-800">無數據</div>'}
                    </div>
                    ${ti[0] ? `<div class="mt-2 pt-2 border-t border-red-900/10 text-center animate-pulse-honor"><span class="text-xs font-bold text-red-300">💎 年度真愛：${ti[0][0]}</span></div>` : ''}
                </div>
            `;
            document.getElementById('overview-container').innerHTML = `
                <div class="bg-zinc-900/40 p-2 rounded-xl text-center border border-red-900/10">
                    <div class="text-[8px] text-zinc-500 font-bold mb-1 uppercase">年度杯數</div><div class="text-sm font-black text-red-100">${total}</div>
                </div>
                <div class="bg-zinc-900/40 p-2 rounded-xl text-center border border-red-900/10">
                    <div class="text-[8px] text-zinc-500 font-bold mb-1 uppercase">總支出</div><div class="text-sm font-black text-red-100">$${spent}</div>
                </div>
                <div class="bg-zinc-900/40 p-2 rounded-xl text-center border border-red-900/10">
                    <div class="text-[8px] text-zinc-500 font-bold mb-1 uppercase">平均單價</div><div class="text-sm font-black text-red-100">$${total?Math.round(spent/total):0}</div>
                </div>
            `;
        }

        function renderCalendar() {
            const year = currentMonth.getFullYear(), month = currentMonth.getMonth();
            document.getElementById('current-month-display').innerText = `${year}年${month+1}月`;
            const grid = document.getElementById('calendar-grid'); grid.innerHTML = '';
            const start = new Date(year, month, 1); start.setDate(start.getDate() - start.getDay());
            for(let i=0; i<35; i++){
                const d = new Date(start); const k = d.toISOString().split('T')[0];
                const active = d.getMonth() === month;
                const cell = document.createElement('div');
                cell.className = `min-h-[75px] border border-zinc-900 p-1 flex flex-col ${active?'bg-zinc-950':'opacity-10 pointer-events-none'}`;
                cell.onclick = () => openModal(k);
                cell.innerHTML = `
                    <span class="text-[9px] font-bold text-zinc-600">${d.getDate()}</span>
                    <div class="flex flex-wrap gap-0.5 mt-1">
                        ${(records[k]||[]).map(()=>`<div class="w-1.5 h-1.5 bg-red-600 rounded-full shadow-[0_0_5px_rgba(220,38,38,0.5)]"></div>`).join('')}
                    </div>
                `;
                grid.appendChild(cell); start.setDate(start.getDate()+1);
            }
        }

        function openModal(k) {
            selectedDateKey = k;
            document.getElementById('modal').classList.remove('hidden');
            document.getElementById('modal-date-title').innerText = k;
            renderModalContent();
        }

        function closeModal() { document.getElementById('modal').classList.add('hidden'); save(); }

        function renderModalContent() {
            const rs = records[selectedDateKey] || [];
            let h = rs.map((r, i) => `
                <div class="bg-zinc-900/60 p-3 rounded-xl border border-red-900/10 mb-3 text-[12px]">
                    <div class="flex justify-between items-center mb-3">
                        <span class="font-black text-red-500 italic uppercase">Drink No.${i+1}</span>
                        <button onclick="deleteR(${i})" class="text-zinc-600 p-1"><i class="fas fa-trash-alt"></i></button>
                    </div>
                    <div class="space-y-2">
                        <input type="text" placeholder="店家" value="${r.shop}" onchange="updR(${i},'shop',this.value)" class="w-full bg-black border border-zinc-800 rounded-lg p-3 outline-none focus:border-red-900 text-white">
                        <input type="text" placeholder="飲品" value="${r.item}" onchange="updR(${i},'item',this.value)" class="w-full bg-black border border-zinc-800 rounded-lg p-3 outline-none focus:border-red-900 text-white">
                        <input type="number" placeholder="金額" value="${r.price}" onchange="updR(${i},'price',this.value)" class="w-full bg-black border border-zinc-800 rounded-lg p-3 outline-none focus:border-red-900 text-white">
                    </div>
                    <div class="grid grid-cols-2 gap-2 mt-3">
                        <select onchange="updR(${i},'ice',this.value)" class="bg-black border border-zinc-800 rounded-lg p-3 text-zinc-400">
                            ${iceOpts.map(o=>`<option ${r.ice===o?'selected':''}>${o}</option>`).join('')}
                        </select>
                        <select onchange="updR(${i},'sugar',this.value)" class="bg-black border border-zinc-800 rounded-lg p-3 text-zinc-400">
                            ${sugarOpts.map(o=>`<option ${r.sugar===o?'selected':''}>${o}</option>`).join('')}
                        </select>
                    </div>
                </div>
            `).join('');
            if(rs.length < 2) h += `
                <button onclick="addR()" class="w-full py-6 border-2 border-dashed border-red-900/20 rounded-xl text-red-900 flex flex-col items-center gap-1 active:bg-red-900/10">
                    <i class="fas fa-plus"></i>
                    <span class="text-[10px] font-bold uppercase">新增紀錄</span>
                </button>`;
            document.getElementById('modal-content').innerHTML = h;
        }

        function addR() { if(!records[selectedDateKey]) records[selectedDateKey]=[]; records[selectedDateKey].push({shop:'',item:'',price:'',ice:'微冰',sugar:'微糖'}); renderModalContent(); }
        function updR(i,f,v) { records[selectedDateKey][i][f]=v; }
        function deleteR(i) { records[selectedDateKey].splice(i,1); if(!records[selectedDateKey].length) delete records[selectedDateKey]; renderModalContent(); }

        init();
    </script>
</body>
</html>

