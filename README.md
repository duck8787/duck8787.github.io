import React, { useState, useEffect, useRef, useMemo } from 'react';
import { 
  Compass, 
  Code, 
  Mic, 
  BookOpen, 
  Play, 
  CheckCircle, 
  MessageSquare, 
  Zap, 
  BarChart3, 
  Award,
  ChevronRight,
  BrainCircuit,
  Terminal,
  Search,
  Layout,
  Sparkles,
  Layers,
  Cpu,
  ArrowRight
} from 'lucide-react';

// --- 核心數據配置 ---
const ROADMAP_STEPS = [
  { id: 1, title: 'AI 核心概念啟蒙', status: 'completed', type: 'theory', duration: '30m', icon: <BookOpen className="w-5 h-5" /> },
  { id: 2, title: 'Python 強化訓練', status: 'current', type: 'coding', duration: '1.5h', icon: <Code className="w-5 h-5" /> },
  { id: 3, title: 'Pandas 數據洗煉', status: 'locked', type: 'coding', duration: '2h', icon: <BarChart3 className="w-5 h-5" /> },
  { id: 4, title: '神經網絡架構設計', status: 'locked', type: 'theory', duration: '3h', icon: <BrainCircuit className="w-5 h-5" /> },
  { id: 5, title: '生成式 AI 專案實戰', status: 'locked', type: 'project', duration: '5h', icon: <Award className="w-5 h-5" /> },
];

const INITIAL_PYTHON_CODE = `import torch
import torch.nn as nn

class NexusBrain(nn.Module):
    def __init__(self):
        super().__init__()
        self.layer1 = nn.Linear(512, 256)
        self.activation = nn.ReLU()
        self.output = nn.Linear(256, 1)

    def forward(self, x):
        return self.output(self.activation(self.layer1(x)))

print("Nexus 引擎載入中...")
model = NexusBrain()
print("模型參數初始化完成。")`;

// --- 通用 UI 組件 ---

const GlassCard = ({ children, className = "" }) => (
  <div className={`bg-white/5 border border-white/10 backdrop-blur-md rounded-2xl ${className}`}>
    {children}
  </div>
);

const Navbar = ({ activeTab, setActiveTab }) => {
  const tabs = [
    { id: 'roadmap', icon: Compass, label: '學習路徑' },
    { id: 'lab', icon: Code, label: '實驗室' },
    { id: 'mentor', icon: Mic, label: 'AI 私教' },
    { id: 'project', icon: Layout, label: '我的專案' },
  ];

  return (
    <nav className="fixed bottom-0 left-0 right-0 md:top-0 md:bottom-auto bg-[#050507]/80 backdrop-blur-xl border-t md:border-b border-white/5 px-4 py-2 z-50">
      <div className="max-w-5xl mx-auto flex justify-between items-center h-14">
        <div className="hidden md:flex items-center gap-3 font-black text-2xl tracking-tighter text-white">
          <div className="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center">
            <Zap size={20} className="fill-white text-white" />
          </div>
          <span>NEXUS</span>
        </div>
        
        <div className="flex w-full md:w-auto justify-around gap-2 md:gap-4">
          {tabs.map((tab) => {
            const isActive = activeTab === tab.id;
            return (
              <button
                key={tab.id}
                onClick={() => setActiveTab(tab.id)}
                className={`relative px-4 py-2 flex flex-col md:flex-row items-center gap-2 transition-all duration-300 rounded-xl ${
                  isActive ? 'text-white' : 'text-gray-500 hover:text-gray-300'
                }`}
              >
                {isActive && (
                  <div className="absolute inset-0 bg-white/5 rounded-xl animate-in fade-in zoom-in-95 duration-300" />
                )}
                <tab.icon size={20} className={isActive ? "text-blue-400" : ""} />
                <span className="text-[10px] md:text-sm font-bold tracking-wide">{tab.label}</span>
              </button>
            );
          })}
        </div>
      </div>
    </nav>
  );
};

// --- 分頁視圖組件 ---

const RoadmapView = () => (
  <div className="px-6 py-8 md:py-12 max-w-2xl mx-auto space-y-10 animate-in fade-in slide-in-from-bottom-8 duration-700">
    <header className="space-y-4">
      <div className="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-blue-500/10 border border-blue-500/20 text-blue-400 text-xs font-bold uppercase tracking-widest">
        <Sparkles size={12} /> 目標：資深 AI 工程師
      </div>
      <h1 className="text-4xl md:text-5xl font-black text-white leading-tight">
        探索你的 <br/><span className="text-blue-500">智慧進化路徑</span>
      </h1>
      <p className="text-gray-400 leading-relaxed">
        根據你的實踐表現，AI 導師已動態調整了「進階期」的深度要求。
      </p>
    </header>

    <div className="relative space-y-8">
      {ROADMAP_STEPS.map((step, index) => (
        <div key={step.id} className="group relative flex gap-6">
          {/* 連接線 */}
          {index !== ROADMAP_STEPS.length - 1 && (
            <div className="absolute left-[22px] top-12 bottom-0 w-[2px] bg-white/5 group-hover:bg-blue-500/20 transition-colors" />
          )}
          
          {/* 圖標節點 */}
          <div className={`shrink-0 w-12 h-12 rounded-2xl flex items-center justify-center border-2 z-10 transition-all duration-500 ${
            step.status === 'completed' ? 'bg-blue-600 border-blue-500 shadow-[0_0_20px_rgba(59,130,246,0.3)]' :
            step.status === 'current' ? 'bg-black border-blue-500 animate-pulse' : 'bg-[#0f0f13] border-white/5'
          }`}>
            {step.status === 'completed' ? <CheckCircle className="text-white" size={24} /> : 
             step.status === 'current' ? <Play className="text-blue-500 fill-current" size={20} /> : 
             <div className="text-gray-700">{step.icon}</div>}
          </div>

          {/* 內容卡片 */}
          <GlassCard className={`flex-1 p-5 transition-all duration-300 group-hover:bg-white/[0.08] ${
            step.status === 'current' ? 'border-blue-500/50 ring-1 ring-blue-500/20' : ''
          }`}>
            <div className="flex justify-between items-start mb-2">
              <div className="flex gap-2 items-center">
                <span className={`text-[10px] font-black px-2 py-0.5 rounded uppercase tracking-tighter ${
                  step.type === 'theory' ? 'bg-purple-500/20 text-purple-400' : 
                  step.type === 'coding' ? 'bg-blue-500/20 text-blue-400' : 'bg-emerald-500/20 text-emerald-400'
                }`}>
                  {step.type}
                </span>
                <span className="text-[10px] text-gray-500 font-mono tracking-tighter">{step.duration}</span>
              </div>
            </div>
            <h3 className={`text-lg font-bold ${step.status === 'locked' ? 'text-gray-600' : 'text-white'}`}>
              {step.title}
            </h3>
            {step.status === 'current' && (
              <button className="mt-4 w-full flex items-center justify-center gap-2 py-2.5 bg-blue-600 hover:bg-blue-500 text-white rounded-xl font-bold transition-all transform active:scale-95 shadow-lg shadow-blue-600/20">
                立即解鎖 <ArrowRight size={16} />
              </button>
            )}
          </GlassCard>
        </div>
      ))}
    </div>
  </div>
);

const LabView = () => {
  const [code, setCode] = useState(INITIAL_PYTHON_CODE);
  const [isRunning, setIsRunning] = useState(false);
  const [output, setOutput] = useState("");
  const [neuronActivity, setNeuronActivity] = useState([]);

  useEffect(() => {
    setNeuronActivity([...Array(12)].map(() => Math.random() * 100));
  }, []);

  const runCode = () => {
    setIsRunning(true);
    setOutput(">>> Initializing Tensors...\n>>> Device detected: CUDA (GeForce RTX 4090)\n>>> Running Layer 1 Forward...");
    
    setTimeout(() => {
      setOutput(prev => prev + "\n>>> Loss: 0.8421 -> 0.4312\n>>> Epoch 1: 100% Complete\n>>> 模型優化建議：考慮在 Layer 1 後加入 BatchNormalization 以穩定數值波動。");
      setIsRunning(false);
      setNeuronActivity([...Array(12)].map(() => Math.random() * 100));
    }, 2000);
  };

  return (
    <div className="h-full flex flex-col md:flex-row animate-in fade-in duration-500">
      {/* 編輯器區域 */}
      <div className="flex-1 flex flex-col border-r border-white/5">
        <div className="flex items-center justify-between px-4 py-3 bg-[#0a0a0c] border-b border-white/5">
          <div className="flex items-center gap-3">
            <div className="flex gap-1.5">
              <div className="w-3 h-3 rounded-full bg-red-500/50" />
              <div className="w-3 h-3 rounded-full bg-yellow-500/50" />
              <div className="w-3 h-3 rounded-full bg-green-500/50" />
            </div>
            <span className="text-xs font-mono text-gray-500 ml-2">model_trainer.py</span>
          </div>
          <button 
            onClick={runCode}
            disabled={isRunning}
            className="flex items-center gap-2 px-5 py-1.5 bg-white text-black hover:bg-gray-200 rounded-full text-sm font-black transition-all disabled:opacity-50"
          >
            {isRunning ? "運算中..." : <><Play size={14} fill="black" /> RUN</>}
          </button>
        </div>
        <textarea 
          value={code}
          onChange={(e) => setCode(e.target.value)}
          className="flex-1 w-full p-6 bg-[#050507] text-blue-100 font-mono text-sm leading-relaxed outline-none resize-none"
          spellCheck="false"
        />
      </div>

      {/* 監控區域 */}
      <div className="w-full md:w-80 bg-[#0a0a0c] p-6 space-y-6 overflow-y-auto">
        <div className="space-y-4">
          <h4 className="text-[10px] font-black text-gray-500 uppercase tracking-widest flex items-center gap-2">
            <Cpu size={14} /> 運算資源監控
          </h4>
          <GlassCard className="p-4 space-y-4">
            <div className="space-y-2">
              <div className="flex justify-between text-[10px] font-bold text-blue-400">
                <span>GPU UTILIZATION</span>
                <span>84%</span>
              </div>
              <div className="h-1.5 w-full bg-white/5 rounded-full overflow-hidden">
                <div className="h-full bg-blue-500 transition-all duration-1000" style={{ width: '84%' }} />
              </div>
            </div>
            <div className="grid grid-cols-6 gap-1 h-12">
              {neuronActivity.map((val, i) => (
                <div key={i} className="relative w-full h-full bg-white/5 rounded-sm overflow-hidden">
                  <div 
                    className="absolute bottom-0 left-0 right-0 bg-blue-400/40 transition-all duration-500" 
                    style={{ height: `${val}%` }} 
                  />
                </div>
              ))}
            </div>
          </GlassCard>
        </div>

        <div className="space-y-4">
          <h4 className="text-[10px] font-black text-gray-500 uppercase tracking-widest flex items-center gap-2">
            <Terminal size={14} /> 控制台輸出
          </h4>
          <pre className="text-[11px] font-mono text-green-400/80 leading-relaxed whitespace-pre-wrap bg-black/30 p-3 rounded-lg border border-white/5">
            {output || "等待代碼執行..."}
          </pre>
        </div>
      </div>
    </div>
  );
};

const MentorView = () => {
  const [messages, setMessages] = useState([
    { role: 'ai', content: '準備好開始今天的深度學習探索了嗎？你可以問我「如何理解反向傳播算法」，或者直接語音輸入你的技術卡點。' }
  ]);
  const [input, setInput] = useState("");
  const endRef = useRef(null);

  const handleSend = () => {
    if (!input.trim()) return;
    const userMsg = { role: 'user', content: input };
    setMessages(prev => [...prev, userMsg]);
    setInput("");
    
    setTimeout(() => {
      setMessages(prev => [...prev, { 
        role: 'ai', 
        content: '聽起來你正在處理權重衰減的問題。想像一下你的模型正在「減肥」：為了不讓它長得太胖（參數過大導致過擬合），我們給每一份多餘的脂肪（權重）增加一點點跑步壓力（懲罰項）。' 
      }]);
    }, 1000);
  };

  useEffect(() => endRef.current?.scrollIntoView({ behavior: 'smooth' }), [messages]);

  return (
    <div className="h-full flex flex-col bg-[#050507] animate-in fade-in duration-500">
      <div className="flex-1 overflow-y-auto p-6 space-y-6">
        {messages.map((msg, i) => (
          <div key={i} className={`flex ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
            <div className={`max-w-[85%] md:max-w-[70%] p-5 rounded-3xl ${
              msg.role === 'user' 
                ? 'bg-blue-600 text-white rounded-br-none' 
                : 'bg-white/5 border border-white/10 text-gray-200 rounded-tl-none'
            }`}>
              <p className="text-sm md:text-base leading-relaxed tracking-wide">{msg.content}</p>
            </div>
          </div>
        ))}
        <div ref={endRef} />
      </div>

      <div className="p-6 bg-gradient-to-t from-[#0a0a0c] to-transparent">
        <div className="max-w-3xl mx-auto flex items-center gap-3 bg-white/5 backdrop-blur-xl p-2 rounded-2xl border border-white/10 shadow-2xl">
          <button className="w-10 h-10 flex items-center justify-center text-gray-400 hover:text-blue-400 transition-colors">
            <Mic size={22} />
          </button>
          <input 
            value={input}
            onChange={(e) => setInput(e.target.value)}
            onKeyDown={(e) => e.key === 'Enter' && handleSend()}
            placeholder="輸入你的問題，AI 導師隨時在線..."
            className="flex-1 bg-transparent border-none outline-none text-white text-sm py-2"
          />
          <button 
            onClick={handleSend}
            className="w-10 h-10 bg-blue-600 hover:bg-blue-500 rounded-xl flex items-center justify-center transition-all transform active:scale-90"
          >
            <Zap size={20} className="fill-white text-white" />
          </button>
        </div>
      </div>
    </div>
  );
};

const ProjectsView = () => (
  <div className="p-6 md:p-10 max-w-4xl mx-auto space-y-8 animate-in fade-in duration-700">
    <div className="flex justify-between items-end">
      <div>
        <h1 className="text-3xl font-black text-white">實戰專案工坊</h1>
        <p className="text-gray-500 mt-1">你的代碼將在這裡轉化為真實世界的產品</p>
      </div>
      <div className="px-4 py-1.5 rounded-full bg-emerald-500/10 border border-emerald-500/20 text-emerald-400 text-xs font-black">
        COMPLETION: 42%
      </div>
    </div>

    <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
      <div className="group bg-gradient-to-br from-blue-600 to-indigo-800 p-8 rounded-[32px] shadow-2xl relative overflow-hidden transition-all duration-500 hover:scale-[1.02]">
        <div className="absolute -top-10 -right-10 w-40 h-40 bg-white/10 rounded-full blur-3xl group-hover:bg-white/20 transition-all" />
        <div className="relative z-10 space-y-6">
          <div className="flex justify-between items-start">
            <div className="p-3 bg-white/10 rounded-2xl backdrop-blur-md">
              <Cpu size={32} className="text-white" />
            </div>
            <Award className="text-white/40" />
          </div>
          <div>
            <h3 className="text-2xl font-black text-white">智慧視覺辨識系統</h3>
            <p className="text-blue-100/70 text-sm mt-2">使用 PyTorch 構建自定義 CNN 模型，並部署為移動端 API。</p>
          </div>
          <div className="space-y-3">
            <div className="h-2 w-full bg-black/20 rounded-full overflow-hidden">
              <div className="h-full bg-white rounded-full w-[65%]" />
            </div>
            <div className="flex justify-between text-[10px] font-black text-white uppercase tracking-tighter">
              <span>進度: 65%</span>
              <span>剩餘: 4 課時</span>
            </div>
          </div>
          <button className="w-full py-4 bg-white text-blue-700 rounded-2xl font-black text-sm transition-all hover:bg-gray-100 active:scale-95 shadow-xl">
            進入沙盒開發
          </button>
        </div>
      </div>

      <div className="space-y-4">
        <h4 className="text-[10px] font-black text-gray-500 uppercase tracking-widest ml-1">AI 代碼審查日誌</h4>
        <GlassCard className="p-5 space-y-4">
          <div className="flex gap-4">
            <div className="w-10 h-10 shrink-0 bg-amber-500/10 rounded-xl flex items-center justify-center">
              <Zap size={20} className="text-amber-500" />
            </div>
            <div>
              <p className="text-sm font-bold text-white">發現計算樽頸</p>
              <p className="text-xs text-gray-500 mt-1">在 `loss_func.py` 中，建議將矩陣乘法改為矢量化運算，預計可提升訓練速度 12x。</p>
            </div>
          </div>
          <div className="h-px bg-white/5" />
          <div className="flex gap-4">
            <div className="w-10 h-10 shrink-0 bg-blue-500/10 rounded-xl flex items-center justify-center">
              <Layers size={20} className="text-blue-400" />
            </div>
            <div>
              <p className="text-sm font-bold text-white">架構優化建議</p>
              <p className="text-xs text-gray-400 mt-1">偵測到模型在驗證集上出現過擬合，建議增加一層 Dropout 層或調整權重衰減。 </p>
            </div>
          </div>
        </GlassCard>
      </div>
    </div>
  </div>
);

// --- 根應用組件 ---

export default function App() {
  const [activeTab, setActiveTab] = useState('roadmap');
  const [showSplash, setShowSplash] = useState(true);

  useEffect(() => {
    const timer = setTimeout(() => setShowSplash(false), 800);
    return () => clearTimeout(timer);
  }, []);

  if (showSplash) {
    return (
      <div className="min-h-screen bg-[#050507] flex items-center justify-center">
        <div className="text-center space-y-4">
          <div className="w-16 h-16 bg-blue-600 rounded-2xl mx-auto flex items-center justify-center animate-bounce shadow-2xl shadow-blue-600/40">
            <Zap size={32} className="fill-white text-white" />
          </div>
          <h2 className="text-white font-black tracking-widest text-xl animate-pulse">NEXUS AI</h2>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-[#050507] text-gray-100 font-sans selection:bg-blue-500/30 overflow-x-hidden">
      {/* 奢華背景動效 */}
      <div className="fixed top-[-20%] left-[-10%] w-[60%] h-[60%] bg-blue-600/10 blur-[180px] rounded-full pointer-events-none animate-pulse" />
      <div className="fixed bottom-[-10%] right-[-10%] w-[50%] h-[50%] bg-purple-600/5 blur-[150px] rounded-full pointer-events-none" />

      {/* 導覽列 */}
      <Navbar activeTab={activeTab} setActiveTab={setActiveTab} />

      {/* 內容容器 */}
      <main className="min-h-screen pt-0 pb-20 md:pt-14 md:pb-0">
        <div className="h-full">
          {activeTab === 'roadmap' && <RoadmapView />}
          {activeTab === 'lab' && <LabView />}
          {activeTab === 'mentor' && <MentorView />}
          {activeTab === 'project' && <ProjectsView />}
        </div>
      </main>

      {/* 全域懸浮通知 */}
      {activeTab === 'roadmap' && (
        <div className="fixed top-20 left-1/2 -translate-x-1/2 z-[100] w-[90%] max-w-sm">
          <div className="bg-white/10 backdrop-blur-2xl border border-white/20 p-3 rounded-2xl shadow-2xl flex items-center gap-4 animate-in fade-in slide-in-from-top-10 duration-1000 delay-500">
            <div className="w-8 h-8 bg-blue-500 rounded-full flex items-center justify-center animate-pulse">
              <Sparkles size={16} className="text-white" />
            </div>
            <p className="text-xs font-bold text-white leading-snug">
              AI 解析中：你目前的學習速率超過 85% 的用戶，推薦直接跳轉至「神經網絡設計」。
            </p>
          </div>
        </div>
      )}
    </div>
  );
}
