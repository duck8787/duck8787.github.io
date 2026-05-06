import React, { useState, useEffect, useRef } from 'react';
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
  Layout
} from 'lucide-react';

// --- 模擬數據 ---
const ROADMAP_DATA = [
  { id: 1, title: '啟蒙期: AI 導論', status: 'completed', type: 'theory', icon: <BookOpen className="w-5 h-5" /> },
  { id: 2, title: '築基期: Python 基礎', status: 'current', type: 'coding', icon: <Code className="w-5 h-5" /> },
  { id: 3, title: '築基期: 數據處理 (Pandas)', status: 'locked', type: 'coding', icon: <BarChart3 className="w-5 h-5" /> },
  { id: 4, title: '進階期: 深度學習原理', status: 'locked', type: 'theory', icon: <BrainCircuit className="w-5 h-5" /> },
  { id: 5, title: '大師期: 實踐 NLP 專案', status: 'locked', type: 'project', icon: <Award className="w-5 h-5" /> },
];

const INITIAL_CODE = `import torch
import torch.nn as nn

# 定義一個簡單的神經網絡
class SimpleNet(nn.Module):
    def __init__(self):
        super(SimpleNet, self).__init__()
        self.fc1 = nn.Linear(784, 128)
        self.relu = nn.ReLU()
        self.fc2 = nn.Linear(128, 10)

    def forward(self, x):
        x = self.fc1(x)
        x = self.relu(x)
        x = self.fc2(x)
        return x

print("模型初始化成功！")`;

// --- 組件 ---

const Navbar = ({ activeTab, setActiveTab }) => (
  <nav className="fixed bottom-0 left-0 right-0 bg-[#0a0a0c]/90 backdrop-blur-md border-t border-white/10 px-6 py-3 z-50 md:top-0 md:bottom-auto md:border-b md:border-t-0">
    <div className="max-w-4xl mx-auto flex justify-between items-center">
      <div className="hidden md:flex items-center gap-2 font-bold text-xl text-blue-500">
        <Zap className="fill-current" />
        <span>NEXUS AI</span>
      </div>
      <div className="flex w-full md:w-auto justify-around md:justify-end gap-8">
        {[
          { id: 'roadmap', icon: Compass, label: '路徑' },
          { id: 'lab', icon: Code, label: '實驗室' },
          { id: 'mentor', icon: Mic, label: '私教' },
          { id: 'project', icon: Layout, label: '專案' },
        ].map((item) => (
          <button
            key={item.id}
            onClick={() => setActiveTab(item.id)}
            className={`flex flex-col md:flex-row items-center gap-1 md:gap-2 transition-colors ${
              activeTab === item.id ? 'text-blue-500' : 'text-gray-400 hover:text-gray-200'
            }`}
          >
            <item.icon size={20} />
            <span className="text-[10px] md:text-sm font-medium">{item.label}</span>
          </button>
        ))}
      </div>
    </div>
  </nav>
);

const RoadmapView = () => (
  <div className="p-6 space-y-8 animate-in fade-in slide-in-from-bottom-4 duration-500">
    <div className="space-y-2">
      <h1 className="text-3xl font-bold bg-gradient-to-r from-white to-gray-500 bg-clip-text text-transparent">
        你的學習星圖
      </h1>
      <p className="text-gray-400">AI 已根據你的目標「開發圖像辨識 App」優化了路徑</p>
    </div>

    <div className="relative space-y-6">
      <div className="absolute left-6 top-0 bottom-0 w-0.5 bg-gradient-to-b from-blue-500/50 to-transparent z-0" />
      
      {ROADMAP_DATA.map((node, idx) => (
        <div key={node.id} className="relative flex items-start gap-6 z-10">
          <div className={`mt-1 w-12 h-12 rounded-full flex items-center justify-center border-2 transition-all duration-300 ${
            node.status === 'completed' ? 'bg-blue-500 border-blue-400 shadow-[0_0_15px_rgba(59,130,246,0.5)]' :
            node.status === 'current' ? 'bg-gray-900 border-blue-500 animate-pulse' :
            'bg-gray-900 border-gray-700'
          }`}>
            {node.status === 'completed' ? <CheckCircle className="text-white w-6 h-6" /> : 
             node.status === 'current' ? <Play className="text-blue-500 w-5 h-5 fill-current" /> : 
             <div className="text-gray-600">{node.icon}</div>}
          </div>
          
          <div className={`flex-1 p-4 rounded-2xl border transition-all ${
            node.status === 'current' ? 'bg-white/5 border-blue-500/30' : 'bg-white/5 border-white/5'
          }`}>
            <div className="flex justify-between items-center mb-1">
              <span className={`text-xs font-bold px-2 py-0.5 rounded uppercase tracking-wider ${
                node.type === 'theory' ? 'bg-purple-500/20 text-purple-400' : 
                node.type === 'coding' ? 'bg-blue-500/20 text-blue-400' : 'bg-orange-500/20 text-orange-400'
              }`}>
                {node.type}
              </span>
              <span className="text-[10px] text-gray-500">預計 45 分鐘</span>
            </div>
            <h3 className={`font-bold ${node.status === 'locked' ? 'text-gray-500' : 'text-gray-100'}`}>
              {node.title}
            </h3>
            {node.status === 'current' && (
              <button className="mt-3 w-full py-2 bg-blue-600 hover:bg-blue-500 text-white text-sm font-bold rounded-lg transition-all">
                繼續學習
              </button>
            )}
          </div>
        </div>
      ))}
    </div>
  </div>
);

const LabView = () => {
  const [code, setCode] = useState(INITIAL_CODE);
  const [isExecuting, setIsExecuting] = useState(false);
  const [output, setOutput] = useState("");
  // 用於儲存隨機生成的高度，避免 Liquid 語法錯誤
  const [neuronHeights, setNeuronHeights] = useState([]);

  useEffect(() => {
    // 初始化隨機高度
    const heights = [...Array(8)].map(() => `${Math.floor(Math.random() * 80) + 20}%`);
    setNeuronHeights(heights);
  }, []);

  const handleRun = () => {
    setIsExecuting(true);
    setTimeout(() => {
      setOutput(">>> 模型初始化成功！\n>>> Input Shape: [batch, 784]\n>>> Output Shape: [batch, 10]\n>>> 建議：嘗試增加 Dropout 層以減少過擬合。");
      setIsExecuting(false);
      // 模擬運行後的高度變化
      setNeuronHeights([...Array(8)].map(() => `${Math.floor(Math.random() * 80) + 20}%`));
    }, 1500);
  };

  return (
    <div className="h-full flex flex-col animate-in fade-in duration-500">
      <div className="flex items-center justify-between p-4 border-b border-white/10 bg-[#0a0a0c]">
        <div className="flex items-center gap-2">
          <Terminal size={18} className="text-blue-500" />
          <span className="font-mono text-sm">neural_network.py</span>
        </div>
        <button 
          onClick={handleRun}
          disabled={isExecuting}
          className="flex items-center gap-2 px-4 py-1.5 bg-blue-600 hover:bg-blue-500 rounded-lg text-sm font-bold transition-all disabled:opacity-50"
        >
          {isExecuting ? <div className="w-4 h-4 border-2 border-white/30 border-t-white rounded-full animate-spin" /> : <Play size={16} fill="currentColor" />}
          執行
        </button>
      </div>

      <div className="flex-1 overflow-auto bg-[#050507] p-4 font-mono text-sm leading-relaxed">
        <textarea 
          value={code}
          onChange={(e) => setCode(e.target.value)}
          className="w-full h-1/2 bg-transparent text-blue-100 outline-none resize-none"
          spellCheck="false"
        />
        <div className="mt-4 border-t border-white/10 pt-4">
          <div className="flex items-center gap-2 text-gray-500 mb-2">
            <BarChart3 size={16} />
            <span className="text-xs font-bold uppercase tracking-widest">AI 啟發式建議 / 控制台輸出</span>
          </div>
          <pre className="text-green-400 whitespace-pre-wrap">{output || "等待運行代碼..."}</pre>
          
          <div className="mt-4 grid grid-cols-2 gap-4">
            <div className="p-4 rounded-xl bg-blue-500/5 border border-blue-500/20">
              <p className="text-[10px] text-blue-400 font-bold uppercase mb-1">神經元激活狀態</p>
              <div className="flex gap-1">
                {neuronHeights.map((h, i) => (
                  <div key={i} className="h-12 w-full bg-blue-500/20 rounded-sm relative overflow-hidden">
                    <div 
                      className="absolute bottom-0 left-0 right-0 bg-blue-400 animate-pulse transition-all duration-500" 
                      style={{ height: h }} 
                    />
                  </div>
                ))}
              </div>
            </div>
            <div className="p-4 rounded-xl bg-purple-500/5 border border-purple-500/20">
              <p className="text-[10px] text-purple-400 font-bold uppercase mb-1">梯度消失風險</p>
              <div className="h-12 flex items-center justify-center">
                <span className="text-2xl font-bold text-purple-400">極低</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

const MentorView = () => {
  const [messages, setMessages] = useState([
    { role: 'ai', text: '你好！我是你的 AI 私教 Nexus。今天想深入討論哪一部分？你可以用說的，或是直接問我關於 Loss Function 的比喻。' }
  ]);
  const [input, setInput] = useState("");
  const scrollRef = useRef(null);

  const sendMessage = () => {
    if (!input.trim()) return;
    const newMessages = [...messages, { role: 'user', text: input }];
    setMessages(newMessages);
    setInput("");
    
    setTimeout(() => {
      setMessages([...newMessages, { 
        role: 'ai', 
        text: '這是一個好問題！把「卷積層 (Convolution)」想像成你在「做菜」前洗菜的過程：你不是一次洗整籃菜，而是把水流（濾鏡）在每一片葉子上滑過，提取出乾淨的部分（特徵）。' 
      }]);
    }, 1000);
  };

  useEffect(() => {
    scrollRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);

  return (
    <div className="flex flex-col h-full bg-[#0a0a0c] animate-in fade-in duration-500">
      <div className="flex-1 overflow-y-auto p-4 space-y-4">
        {messages.map((msg, i) => (
          <div key={i} className={`flex ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
            <div className={`max-w-[80%] p-4 rounded-2xl ${
              msg.role === 'user' ? 'bg-blue-600 text-white rounded-tr-none' : 'bg-white/10 text-gray-200 rounded-tl-none border border-white/5 shadow-xl'
            }`}>
              <p className="text-sm leading-relaxed">{msg.text}</p>
            </div>
          </div>
        ))}
        <div ref={scrollRef} />
      </div>

      <div className="p-4 bg-white/5 border-t border-white/10">
        <div className="flex items-center gap-3 bg-black/40 p-2 rounded-2xl border border-white/10">
          <button className="p-2 text-gray-400 hover:text-blue-500 transition-colors">
            <Mic size={24} />
          </button>
          <input 
            value={input}
            onChange={(e) => setInput(e.target.value)}
            onKeyPress={(e) => e.key === 'Enter' && sendMessage()}
            placeholder="詢問任何概念或請求模擬面試..."
            className="flex-1 bg-transparent text-sm outline-none text-white"
          />
          <button 
            onClick={sendMessage}
            className="p-2 text-blue-500 hover:text-blue-400 transition-transform active:scale-95"
          >
            <Zap size={24} fill="currentColor" />
          </button>
        </div>
        <p className="text-[10px] text-center text-gray-600 mt-3 uppercase tracking-widest">
          Nexus AI 正在傾聽並學習你的學習節奏
        </p>
      </div>
    </div>
  );
};

const ProjectView = () => (
  <div className="p-6 space-y-6 animate-in fade-in duration-500">
    <div className="flex justify-between items-center">
      <h1 className="text-2xl font-bold">專案實戰引擎</h1>
      <span className="text-xs bg-green-500/20 text-green-400 px-2 py-1 rounded">2 個活躍專案</span>
    </div>

    <div className="bg-gradient-to-br from-blue-600 to-indigo-700 rounded-2xl p-6 shadow-2xl relative overflow-hidden">
      <div className="absolute top-0 right-0 p-4 opacity-10">
        <Award size={100} />
      </div>
      <div className="relative z-10">
        <h3 className="text-xl font-bold mb-2 text-white">貓狗辨識分類器</h3>
        <p className="text-blue-100 text-sm mb-4">使用 CNN 架構，準確率目標 95% 以上。</p>
        <div className="w-full bg-white/20 h-2 rounded-full mb-2">
          <div className="bg-white h-full rounded-full w-[65%]" />
        </div>
        <div className="flex justify-between text-[10px] text-blue-100 font-bold uppercase mb-4">
          <span>進度 65%</span>
          <span>剩餘 3 個單元</span>
        </div>
        <button className="w-full py-3 bg-white text-blue-600 font-bold rounded-xl hover:bg-blue-50 transition-colors">
          進入沙盒繼續開發
        </button>
      </div>
    </div>

    <div className="space-y-4">
      <h4 className="text-sm font-bold text-gray-400 uppercase tracking-widest">AI 代碼審查建議</h4>
      <div className="bg-white/5 border border-white/5 rounded-xl p-4 flex gap-4 items-start">
        <div className="p-2 bg-orange-500/20 rounded-lg">
          <Zap className="text-orange-500" size={20} />
        </div>
        <div>
          <h5 className="font-bold text-sm text-gray-200">效能優化建議</h5>
          <p className="text-xs text-gray-400 mt-1">偵測到數據載入器 (DataLoader) 未開啟多執行緒，建議設置 num_workers=4 以提升訓練速度。</p>
        </div>
      </div>
    </div>
  </div>
);

// --- 主應用 ---

export default function App() {
  const [activeTab, setActiveTab] = useState('roadmap');
  const [isLoaded, setIsLoaded] = useState(false);

  useEffect(() => {
    setIsLoaded(true);
  }, []);

  return (
    <div className="min-h-screen bg-[#050507] text-gray-100 font-sans selection:bg-blue-500/30">
      {/* 背景裝飾 */}
      <div className="fixed top-[-10%] left-[-10%] w-[40%] h-[40%] bg-blue-600/10 blur-[120px] rounded-full pointer-events-none" />
      <div className="fixed bottom-[-10%] right-[-10%] w-[40%] h-[40%] bg-purple-600/10 blur-[120px] rounded-full pointer-events-none" />

      {/* 主內容區 */}
      <main className="max-w-4xl mx-auto pb-24 md:pt-20 min-h-screen flex flex-col">
        {activeTab === 'roadmap' && <RoadmapView />}
        {activeTab === 'lab' && <LabView />}
        {activeTab === 'mentor' && <MentorView />}
        {activeTab === 'project' && <ProjectView />}
      </main>

      <Navbar activeTab={activeTab} setActiveTab={setActiveTab} />
      
      {/* 情緒激勵通知 (模擬彈出) */}
      {isLoaded && activeTab === 'roadmap' && (
        <div className="fixed top-8 left-1/2 -translate-x-1/2 bg-blue-900/40 border border-blue-500/30 backdrop-blur-md px-4 py-2 rounded-full flex items-center gap-3 animate-in fade-in slide-in-from-top-4 duration-1000 delay-1000 z-[100]">
          <div className="w-2 h-2 bg-blue-400 rounded-full animate-ping" />
          <span className="text-xs font-medium text-blue-200">AI 偵測：你現在專注度極高，非常適合挑戰「數據處理」環節！</span>
        </div>
      )}
    </div>
  );
}
