<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEXUS AI - 智慧進化平台</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&family=JetBrains+Mono&display=swap');

        :root {
            --bg-color: #050507;
            --accent-blue: #3b82f6;
        }

        body {
            background-color: var(--bg-color);
            color: #f3f4f6;
            font-family: 'Inter', sans-serif;
            overflow-x: hidden;
        }

        .glass {
            background: rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .gradient-blur {
            position: fixed;
            z-index: -1;
            filter: blur(150px);
            border-radius: 50%;
            pointer-events: none;
        }

        .nav-item.active {
            color: white;
            background: rgba(255, 255, 255, 0.05);
        }

        .nav-item.active i {
            color: var(--accent-blue);
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: fadeInUp 0.6s ease-out forwards;
        }

        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 自定義滾動條 */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
        }

        /* 實驗室高度修正 (避免 Liquid 異常的寫法) */
        .bar-container {
            height: 100%;
            width: 100%;
            background: rgba(255, 255, 255, 0.05);
            position: relative;
            border-radius: 2px;
            overflow: hidden;
        }

        .bar-fill {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(59, 130, 246, 0.4);
            transition: height 0.5s ease;
        }
    </style>
</head>
<body class="pb-24 md:pb-0 md:pt-16">

    <!-- 背景裝飾 -->
    <div class="gradient-blur top-[-10%] left-[-5%] w-[60vw] h-[60vw] bg-blue-600/10"></div>
    <div class="gradient-blur bottom-[-5%] right-[-5%] w-[40vw] h-[40vw] bg-purple-600/5"></div>

    <!-- 頂部導覽列 (桌面端) -->
    <nav class="fixed top-0 left-0 right-0 h-16 glass z-50 px-6 hidden md:flex items-center justify-between border-b border-white/5">
        <div class="flex items-center gap-3">
            <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center shadow-lg shadow-blue-600/20">
                <i class="fas fa-bolt text-white"></i>
            </div>
            <span class="text-xl font-black tracking-tighter">NEXUS</span>
        </div>
        <div class="flex gap-4">
            <button onclick="switchTab('roadmap')" class="nav-item active px-4 py-2 rounded-xl text-sm font-bold flex items-center gap-2 transition-all">
                <i class="fas fa-compass"></i> 學習路徑
            </button>
            <button onclick="switchTab('lab')" class="nav-item px-4 py-2 rounded-xl text-sm font-bold flex items-center gap-2 transition-all">
                <i class="fas fa-code"></i> 實驗室
            </button>
            <button onclick="switchTab('mentor')" class="nav-item px-4 py-2 rounded-xl text-sm font-bold flex items-center gap-2 transition-all">
                <i class="fas fa-microphone"></i> AI 私教
            </button>
        </div>
        <div class="flex items-center gap-3">
            <div class="text-right">
                <p class="text-[10px] text-gray-500 font-bold uppercase">Level 12</p>
                <p class="text-xs font-bold text-blue-400">系統架構師</p>
            </div>
            <div class="w-10 h-10 rounded-full bg-gradient-to-tr from-blue-500 to-purple-500 p-[2px]">
                <div class="w-full h-full rounded-full bg-black flex items-center justify-center overflow-hidden">
                    <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Felix" alt="User">
                </div>
            </div>
        </div>
    </nav>

    <!-- 主內容區 -->
    <main class="max-w-5xl mx-auto p-6">
        
        <!-- Tab 1: 學習路徑 -->
        <section id="roadmap" class="tab-content active space-y-12 py-8">
            <header class="space-y-4">
                <span class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-blue-500/10 border border-blue-500/20 text-blue-400 text-[10px] font-black uppercase tracking-widest">
                    <i class="fas fa-sparkles"></i> 目標：資深 AI 工程師
                </span>
                <h1 class="text-4xl md:text-5xl font-black leading-tight">
                    探索你的 <br><span class="text-blue-500">智慧進化路徑</span>
                </h1>
                <p class="text-gray-400 max-w-lg">根據你的實踐表現，AI 導師已動態調整了「進階期」的深度要求。</p>
            </header>

            <div class="relative space-y-8">
                <!-- 線條 -->
                <div class="absolute left-[23px] top-10 bottom-10 w-[2px] bg-white/5"></div>

                <!-- 步驟 1 (完成) -->
                <div class="flex gap-6 group relative">
                    <div class="w-12 h-12 shrink-0 rounded-2xl flex items-center justify-center border-2 border-blue-500 bg-blue-600 shadow-[0_0_20px_rgba(59,130,246,0.3)] z-10">
                        <i class="fas fa-check text-white"></i>
                    </div>
                    <div class="glass flex-1 p-6 rounded-3xl hover:bg-white/[0.06] transition-all">
                        <span class="text-[10px] font-black px-2 py-0.5 bg-purple-500/20 text-purple-400 rounded uppercase">Theory</span>
                        <h3 class="text-lg font-bold mt-2">AI 核心概念啟蒙</h3>
                    </div>
                </div>

                <!-- 步驟 2 (進行中) -->
                <div class="flex gap-6 group relative">
                    <div class="w-12 h-12 shrink-0 rounded-2xl flex items-center justify-center border-2 border-blue-500 bg-black animate-pulse z-10">
                        <i class="fas fa-play text-blue-500 text-sm"></i>
                    </div>
                    <div class="glass flex-1 p-6 rounded-3xl border-blue-500/30 ring-1 ring-blue-500/20 shadow-xl shadow-blue-500/5">
                        <div class="flex justify-between items-start">
                            <span class="text-[10px] font-black px-2 py-0.5 bg-blue-500/20 text-blue-400 rounded uppercase">Coding</span>
                            <span class="text-[10px] font-mono text-gray-500">1.5h</span>
                        </div>
                        <h3 class="text-lg font-bold mt-2">Python 強化訓練</h3>
                        <button class="mt-4 w-full py-3 bg-blue-600 hover:bg-blue-500 text-white rounded-xl font-bold transition-all transform active:scale-95 shadow-lg shadow-blue-600/20">
                            立即解鎖課程 <i class="fas fa-arrow-right ml-2"></i>
                        </button>
                    </div>
                </div>

                <!-- 步驟 3 (鎖定) -->
                <div class="flex gap-6 group relative opacity-50">
                    <div class="w-12 h-12 shrink-0 rounded-2xl flex items-center justify-center border-2 border-white/5 bg-[#0f0f13] z-10">
                        <i class="fas fa-lock text-gray-600"></i>
                    </div>
                    <div class="glass flex-1 p-6 rounded-3xl">
                        <h3 class="text-lg font-bold text-gray-500">神經網絡架構設計</h3>
                    </div>
                </div>
            </div>
        </section>

        <!-- Tab 2: 實驗室 -->
        <section id="lab" class="tab-content h-[calc(100vh-180px)] md:h-[700px] py-4">
            <div class="flex flex-col md:flex-row h-full rounded-3xl overflow-hidden border border-white/5">
                <!-- 編輯器 -->
                <div class="flex-1 flex flex-col bg-[#050507]">
                    <div class="flex justify-between items-center px-4 py-3 bg-[#0a0a0c] border-b border-white/5">
                        <div class="flex items-center gap-2">
                            <div class="flex gap-1">
                                <div class="w-2.5 h-2.5 rounded-full bg-red-500/50"></div>
                                <div class="w-2.5 h-2.5 rounded-full bg-yellow-500/50"></div>
                                <div class="w-2.5 h-2.5 rounded-full bg-green-500/50"></div>
                            </div>
                            <span class="text-[10px] font-mono text-gray-500 ml-2">nexus_brain.py</span>
                        </div>
                        <button onclick="simulateRun()" class="px-4 py-1.5 bg-white text-black rounded-full text-xs font-black hover:bg-gray-200 transition-all">
                            <i class="fas fa-play mr-1"></i> RUN
                        </button>
                    </div>
                    <textarea id="codeEditor" class="flex-1 p-6 bg-[#050507] text-blue-100 font-mono text-sm leading-relaxed outline-none resize-none" spellcheck="false">import torch
import torch.nn as nn

class NexusBrain(nn.Module):
    def __init__(self):
        super().__init__()
        self.layer = nn.Linear(512, 10)

    def forward(self, x):
        return self.layer(x)

print("引擎載入中...")
# 開始進行模型參數初始化</textarea>
                </div>

                <!-- 監控面板 -->
                <div class="w-full md:w-72 bg-[#0a0a0c] border-l border-white/5 flex flex-col">
                    <div class="p-6 space-y-8 flex-1">
                        <div class="space-y-4">
                            <h4 class="text-[10px] font-black text-gray-500 uppercase tracking-widest flex items-center gap-2">
                                <i class="fas fa-microchip"></i> 運算資源
                            </h4>
                            <div class="glass p-4 rounded-2xl space-y-4">
                                <div class="space-y-2">
                                    <div class="flex justify-between text-[10px] font-bold text-blue-400">
                                        <span>GPU 負載</span>
                                        <span id="gpuVal">0%</span>
                                    </div>
                                    <div class="h-1.5 bg-white/5 rounded-full overflow-hidden">
                                        <div id="gpuBar" class="h-full bg-blue-500 w-0 transition-all duration-1000"></div>
                                    </div>
                                </div>
                                <div class="grid grid-cols-6 gap-1 h-12" id="neuronGrid">
                                    <!-- Bars generated via JS -->
                                </div>
                            </div>
                        </div>

                        <div class="space-y-4">
                            <h4 class="text-[10px] font-black text-gray-500 uppercase tracking-widest flex items-center gap-2">
                                <i class="fas fa-terminal"></i> 輸出控制台
                            </h4>
                            <pre id="console" class="text-[11px] font-mono text-green-400/80 leading-relaxed whitespace-pre-wrap bg-black/50 p-4 rounded-xl h-40 overflow-y-auto">等待執行...</pre>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Tab 3: AI 私教 -->
        <section id="mentor" class="tab-content py-8 h-[calc(100vh-220px)] md:h-[700px] flex flex-col">
            <div id="chatWindow" class="flex-1 overflow-y-auto space-y-6 px-2 mb-6">
                <div class="flex justify-start">
                    <div class="glass p-5 rounded-3xl rounded-tl-none max-w-[80%] text-sm leading-relaxed">
                        準備好開始今天的深度學習探索了嗎？你可以問我「如何理解卷積網絡」，或者直接描述你的代碼問題。
                    </div>
                </div>
            </div>
            <div class="glass p-2 rounded-2xl flex items-center gap-3">
                <input type="text" id="chatInput" placeholder="在此輸入問題..." class="flex-1 bg-transparent border-none outline-none px-4 py-2 text-sm text-white">
                <button onclick="sendMessage()" class="w-10 h-10 bg-blue-600 rounded-xl flex items-center justify-center text-white hover:bg-blue-500 transition-all">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
        </section>

    </main>

    <!-- 底部導覽 (移動端) -->
    <nav class="fixed bottom-0 left-0 right-0 h-18 glass z-50 flex md:hidden justify-around items-center px-4 border-t border-white/5">
        <button onclick="switchTab('roadmap')" class="flex flex-col items-center gap-1 text-gray-500 active-tab">
            <i class="fas fa-compass text-lg"></i>
            <span class="text-[10px] font-bold">路徑</span>
        </button>
        <button onclick="switchTab('lab')" class="flex flex-col items-center gap-1 text-gray-500">
            <i class="fas fa-code text-lg"></i>
            <span class="text-[10px] font-bold">實驗室</span>
        </button>
        <button onclick="switchTab('mentor')" class="flex flex-col items-center gap-1 text-gray-500">
            <i class="fas fa-microphone text-lg"></i>
            <span class="text-[10px] font-bold">導師</span>
        </button>
    </nav>

    <script>
        // 切換頁籤邏輯
        function switchTab(tabId) {
            // 切換內容
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            document.getElementById(tabId).classList.add('active');

            // 更新導航樣式
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('active');
                if(item.innerText.includes(tabId === 'roadmap' ? '學習路徑' : (tabId === 'lab' ? '實驗室' : 'AI 私教'))) {
                    item.classList.add('active');
                }
            });

            // 針對實驗室初始化
            if(tabId === 'lab') {
                initLab();
            }
        }

        // 實驗室功能
        function initLab() {
            const grid = document.getElementById('neuronGrid');
            grid.innerHTML = '';
            for(let i=0; i<12; i++) {
                const container = document.createElement('div');
                container.className = 'bar-container';
                const fill = document.createElement('div');
                fill.className = 'bar-fill';
                fill.id = 'bar-' + i;
                container.appendChild(fill);
                grid.appendChild(container);
            }
        }

        function simulateRun() {
            const consoleEl = document.getElementById('console');
            const gpuBar = document.getElementById('gpuBar');
            const gpuVal = document.getElementById('gpuVal');

            consoleEl.innerText = ">>> 正在編譯模型...\n>>> 硬體加速已開啟: NVIDIA RTX 4090\n>>> 正在加載權重...";
            
            // 隨機 GPU 負載
            const targetGpu = Math.floor(Math.random() * 40) + 60;
            gpuBar.style.width = targetGpu + "%";
            gpuVal.innerText = targetGpu + "%";

            // 隨機神經元活動 (避免 `${}` 以免干擾某些模板引擎)
            for(let i=0; i<12; i++) {
                const bar = document.getElementById('bar-' + i);
                if(bar) bar.style.height = Math.floor(Math.random() * 100) + "%";
            }

            setTimeout(() => {
                consoleEl.innerText += "\n>>> Epoch 1/10 - Loss: 0.842\n>>> Epoch 2/10 - Loss: 0.521\n>>> 訓練成功！模型已準備好進行推理。";
            }, 1500);
        }

        // 導師聊天功能
        function sendMessage() {
            const input = document.getElementById('chatInput');
            const window = document.getElementById('chatWindow');
            if(!input.value.trim()) return;

            // 用戶消息
            const userMsg = document.createElement('div');
            userMsg.className = "flex justify-end";
            userMsg.innerHTML = `<div class="bg-blue-600 text-white p-5 rounded-3xl rounded-tr-none max-w-[80%] text-sm shadow-xl">${input.value}</div>`;
            window.appendChild(userMsg);

            const userText = input.value;
            input.value = '';
            window.scrollTop = window.scrollHeight;

            // AI 回應
            setTimeout(() => {
                const aiMsg = document.createElement('div');
                aiMsg.className = "flex justify-start";
                aiMsg.innerHTML = `<div class="glass p-5 rounded-3xl rounded-tl-none max-w-[80%] text-sm border border-white/10">聽起來這是一個關於 ${userText.slice(0,5)}... 的有趣挑戰。從神經網絡的角度來看，建議先檢查數據預處理的歸一化步驟。</div>`;
                window.appendChild(aiMsg);
                window.scrollTop = window.scrollHeight;
            }, 1000);
        }

        // 初始化
        window.onload = () => {
            initLab();
        };
    </script>
</body>
</html>
