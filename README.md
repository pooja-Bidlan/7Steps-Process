import React, { useState, useEffect } from 'react';
import { 
  LayoutDashboard, Users, Dumbbell, Activity, Utensils, 
  TrendingUp, ShieldCheck, MapPin, Building2, 
  Settings, CreditCard, BrainCircuit, HeartPulse, 
  LogOut, ChevronRight, Search, Bell, Zap, 
  AlertCircle, Clock, Shield, Share2, Globe, Database,
  FileText, Download, UserPlus, Star, BarChart3,
  Stethoscope, BatteryCharging, ZapOff, Info
} from 'lucide-react';
import { 
  LineChart, Line, AreaChart, Area, XAxis, YAxis, CartesianGrid, 
  Tooltip, ResponsiveContainer, BarChart, Bar, Cell, PieChart, Pie,
  RadarChart, PolarGrid, PolarAngleAxis, PolarRadiusAxis, Radar
} from 'recharts';

// --- ENTERPRISE CONSTANTS ---

const NATIONAL_CHAINS = [
  { name: 'Cult.fit', branches: 600, cityPresence: 45, valuation: '₹12,000 Cr', growth: 18 },
  { name: "Gold's Gym India", branches: 150, cityPresence: 95, valuation: '₹800 Cr', growth: 5 },
  { name: 'Anytime Fitness India', branches: 120, cityPresence: 35, valuation: '₹500 Cr', growth: 12 },
  { name: 'Talwalkars', branches: 140, cityPresence: 80, valuation: '₹300 Cr', growth: -2 },
  { name: 'Zyming Network (Ours)', branches: 15, cityPresence: 1, valuation: '₹45 Cr', growth: 450 },
];

const FARIDABAD_GYMS = [
  { id: 'h1', name: "Hype The Gym", sector: "Sector 28", rating: 4.9, score: 117, members: 1200, trainers: 18, reach: 92, fee: 3000, color: "#ef4444" },
  { id: 'a1', name: "Anytime Fitness", sector: "Sector 28", rating: 4.8, score: 110, members: 850, trainers: 12, reach: 88, fee: 2500, color: "#8b5cf6" },
  { id: 'e1', name: "Equilibrium Pro", sector: "Sector 9", rating: 4.7, score: 112, members: 2100, trainers: 25, reach: 95, fee: 2800, color: "#3b82f6" },
  { id: 'ef1', name: "Everyday Fitness Iconic", sector: "Sector 16", rating: 4.8, score: 116, members: 950, trainers: 14, reach: 91, fee: 3500, color: "#10b981" },
  { id: 'c1', name: "93 Cross Fitness", sector: "Sector 82", rating: 4.15, score: 105, members: 600, trainers: 10, reach: 76, fee: 3200, color: "#f59e0b" },
];

const MOCK_TRAINERS = [
  { id: 1, name: "Vikram Singh", specialty: "Bodybuilding", clientReach: 145, gymRating: 4.9, shareFactor: 8.5, retention: 94 },
  { id: 2, name: "Priya Sharma", specialty: "Yoga/Mobility", clientReach: 210, gymRating: 5.0, shareFactor: 9.2, retention: 98 },
  { id: 3, name: "Anil Kapoor", specialty: "CrossFit", clientReach: 95, gymRating: 4.7, shareFactor: 7.1, retention: 85 },
];

// --- SHARED COMPONENTS ---

const Sidebar = ({ activeTab, setActiveTab }: { activeTab: string, setActiveTab: (tab: string) => void }) => {
  const menuItems = [
    { id: 'dashboard', label: 'Executive Command', icon: LayoutDashboard },
    { id: 'branches', label: 'Branch Network', icon: Building2 },
    { id: 'members', label: 'Member 360', icon: Users },
    { id: 'trainer-os', label: 'Trainer OS', icon: Dumbbell },
    { id: 'fatigue-index', label: 'AI Fatigue Engine', icon: BrainCircuit },
    { id: 'growth', label: 'Growth Forecast', icon: TrendingUp },
    { id: 'security', label: 'GRC & DPDP', icon: ShieldCheck },
    { id: 'billing', label: 'Enterprise Billing', icon: CreditCard },
  ];

  return (
    <div className="w-64 bg-slate-900 h-screen text-slate-300 flex flex-col fixed left-0 top-0 z-50">
      <div className="p-6 border-b border-slate-800 flex items-center gap-2">
        <Zap className="text-indigo-500 w-6 h-6 fill-current" />
        <span className="text-xl font-bold text-white tracking-tighter">ZYMING AI</span>
      </div>
      <nav className="flex-1 py-4 px-3 space-y-1 overflow-y-auto">
        {menuItems.map((item) => (
          <button
            key={item.id}
            onClick={() => setActiveTab(item.id)}
            className={`w-full flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium transition-all ${
              activeTab === item.id ? 'bg-indigo-600 text-white shadow-lg shadow-indigo-600/20' : 'hover:bg-slate-800'
            }`}
          >
            <item.icon className="w-4 h-4" />
            {item.label}
          </button>
        ))}
      </nav>
      <div className="p-4 border-t border-slate-800">
        <div className="bg-slate-800/50 p-3 rounded-xl flex items-center gap-3">
          <div className="w-8 h-8 rounded-full bg-indigo-500 flex items-center justify-center text-xs font-bold">IN</div>
          <div className="flex-1 min-w-0">
            <p className="text-xs font-bold text-white truncate">National Admin</p>
            <p className="text-[10px] text-slate-500">IND-CORE-ACCESS</p>
          </div>
          <LogOut className="w-4 h-4 text-slate-500" />
        </div>
      </div>
    </div>
  );
};

// --- ENRICHED DASHBOARD COMPONENTS ---

const AIFatigueEngine = () => {
  const [activeMetric, setActiveMetric] = useState<string | null>(null);

  // Fatigue Scoring Parameters (Enterprise Weights)
  const params = [
    { id: 'sleep', label: 'Sleep Volume', val: 7.2, target: 8, weight: 25, color: '#6366f1', icon: Clock, desc: 'Weighted average of last 3 nights including latency.' },
    { id: 'hrv', label: 'HRV Stability', val: 92, target: 100, weight: 25, color: '#10b981', icon: HeartPulse, desc: 'Heart Rate Variability consistency vs. 30-day baseline.' },
    { id: 'volume', label: 'Volume Load', val: 84, target: 100, weight: 15, color: '#3b82f6', icon: Activity, desc: 'Cumulative weight x reps vs. recovery capacity.' },
    { id: 'soreness', label: 'Soreness index', val: 4, target: 10, weight: 10, color: '#f43f5e', icon: Activity, desc: 'User-reported myofascial pain scale (DOMS tracking).' },
    { id: 'steps', label: 'Daily Steps', val: 12400, target: 10000, weight: 10, color: '#f59e0b', icon: Activity, desc: 'Non-exercise activity thermogenesis (NEAT) level.' },
    { id: 'rest', label: 'Rest Cycles', val: 1, target: 2, weight: 10, color: '#8b5cf6', icon: ZapOff, desc: 'Frequency of active recovery days in current microcycle.' },
    { id: 'intensity', label: 'RPE Intensity', val: 8.5, target: 10, weight: 5, color: '#f97316', icon: Zap, desc: 'Average Rate of Perceived Exertion per session.' },
  ];

  // Calculate Weighted Fatigue Score
  const calculateScore = () => {
    const weightedSum = params.reduce((acc, p) => {
      const performance = Math.min((p.val / p.target) * 100, 100);
      return acc + (performance * (p.weight / 100));
    }, 0);
    return Math.round(weightedSum);
  };

  const score = calculateScore();
  const status = score > 80 ? { label: 'Optimal', color: 'text-emerald-500', bg: 'bg-emerald-500', desc: 'Body is ready for high-intensity max-effort load.' } 
               : score > 60 ? { label: 'Fatigued', color: 'text-amber-500', bg: 'bg-amber-500', desc: 'Central Nervous System stress detected. Switch to hypertrophy.' }
               : { label: 'Recovery', color: 'text-rose-500', bg: 'bg-rose-500', desc: 'High burnout risk. Mandatory rest or deload week required.' };

  return (
    <div className="space-y-6 animate-in fade-in duration-500">
      <div className="flex justify-between items-end">
        <div>
          <h2 className="text-3xl font-black text-slate-900 tracking-tight italic uppercase">Fatigue Engine <span className="text-indigo-600">AI</span></h2>
          <p className="text-slate-500 text-sm font-medium tracking-widest uppercase">Proprietary Neural Performance Scoring (v4.2)</p>
        </div>
        <div className="flex gap-2">
          <button className="bg-white border border-slate-200 p-2.5 rounded-xl text-slate-400 hover:text-indigo-600 transition-colors shadow-sm"><Download className="w-5 h-5" /></button>
          <button className="bg-indigo-600 text-white px-6 py-2.5 rounded-xl text-sm font-black shadow-lg shadow-indigo-600/20">Sync Wearable Node</button>
        </div>
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        {/* Main Score UI */}
        <div className="lg:col-span-2 bg-gradient-to-br from-slate-900 via-indigo-950 to-slate-950 p-10 rounded-[2.5rem] text-white shadow-2xl relative overflow-hidden">
          <div className="relative z-10 flex flex-col md:flex-row items-center justify-between gap-12">
            <div className="space-y-6 max-w-sm text-center md:text-left">
              <div className="inline-flex items-center gap-2 px-3 py-1 bg-white/10 rounded-full border border-white/20 backdrop-blur-md">
                <BrainCircuit className="w-4 h-4 text-indigo-400" />
                <span className="text-[10px] font-black uppercase tracking-widest">Neural Cluster Access</span>
              </div>
              <h3 className="text-5xl font-black italic tracking-tighter leading-none">RECOVERY <br/><span className="text-indigo-400">INDEX</span></h3>
              <p className="text-slate-400 text-sm font-medium leading-relaxed">{status.desc}</p>
              <div className="flex items-center gap-3 p-3 bg-white/5 border border-white/10 rounded-2xl">
                <div className={`w-3 h-3 rounded-full ${status.bg} shadow-[0_0_12px_rgba(255,255,255,0.3)] animate-pulse`} />
                <span className={`font-black uppercase tracking-widest text-sm ${status.color}`}>{status.label} PERFORMANCE</span>
              </div>
            </div>

            <div className="relative">
              <div className="w-64 h-64 rounded-full border-[12px] border-white/5 flex items-center justify-center relative shadow-[0_0_80px_rgba(99,102,241,0.15)]">
                <div className="absolute inset-0 flex items-center justify-center">
                  <div className="text-center">
                    <span className="text-8xl font-black italic tracking-tighter tabular-nums">{score}</span>
                    <p className="text-[10px] font-black uppercase tracking-[0.3em] text-indigo-400 mt-2">Score</p>
                  </div>
                </div>
                <svg className="w-full h-full -rotate-90 filter drop-shadow-[0_0_15px_rgba(99,102,241,0.6)]">
                  <circle cx="128" cy="128" r="116" fill="none" stroke="rgba(255,255,255,0.03)" strokeWidth="12" />
                  <circle 
                    cx="128" cy="128" r="116" 
                    fill="none" 
                    stroke="#6366f1" 
                    strokeWidth="12" 
                    strokeDasharray="728.8" 
                    strokeDashoffset={728.8 * (1 - score/100)} 
                    strokeLinecap="round"
                    className="transition-all duration-1000 ease-in-out"
                  />
                </svg>
              </div>
            </div>
          </div>
          {/* Decorative gradients */}
          <div className="absolute -top-24 -right-24 w-96 h-96 bg-indigo-500/10 blur-[120px] rounded-full" />
          <div className="absolute -bottom-24 -left-24 w-72 h-72 bg-blue-500/10 blur-[100px] rounded-full" />
        </div>

        {/* AI Action Alerts */}
        <div className="bg-white p-8 rounded-[2.5rem] border border-slate-200 shadow-sm flex flex-col">
          <h4 className="text-xs font-black text-slate-400 uppercase tracking-widest mb-6">Prescriptive Training Shifts</h4>
          <div className="space-y-4 flex-1">
            <div className="flex gap-4 items-start p-4 bg-amber-50 rounded-2xl border border-amber-100">
              <ZapOff className="w-5 h-5 text-amber-600 mt-0.5" />
              <div>
                <p className="text-xs font-black text-amber-900 uppercase">Volume Cap Triggered</p>
                <p className="text-[11px] text-amber-700 mt-1 font-medium leading-relaxed">System has detected <span className="font-bold underline">CNS overload</span>. Limit all sets to RPE 7 today.</p>
              </div>
            </div>
            <div className="flex gap-4 items-start p-4 bg-indigo-50 rounded-2xl border border-indigo-100">
              <BatteryCharging className="w-5 h-5 text-indigo-600 mt-0.5" />
              <div>
                <p className="text-xs font-black text-indigo-900 uppercase">Supercompensation Peak</p>
                <p className="text-[11px] text-indigo-700 mt-1 font-medium leading-relaxed">Optimal window for <span className="font-bold underline">PR attempt</span>. Glycogen replenishment is at 98%.</p>
              </div>
            </div>
            <div className="flex gap-4 items-start p-4 bg-slate-50 rounded-2xl border border-slate-200">
              <Stethoscope className="w-5 h-5 text-slate-600 mt-0.5" />
              <div>
                <p className="text-xs font-black text-slate-900 uppercase">Joint Integrity Note</p>
                <p className="text-[11px] text-slate-600 mt-1 font-medium leading-relaxed">Soreness reported in posterior chain. Swap Squats for Lunges.</p>
              </div>
            </div>
          </div>
          <button className="mt-6 w-full py-3 bg-slate-900 text-white rounded-xl text-xs font-black uppercase tracking-widest hover:bg-indigo-600 transition-colors">
            Generate Recovery Protocol
          </button>
        </div>
      </div>

      {/* Parameter Grid */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        {params.map(p => (
          <div 
            key={p.id} 
            onMouseEnter={() => setActiveMetric(p.id)}
            onMouseLeave={() => setActiveMetric(null)}
            className="bg-white p-5 rounded-3xl border border-slate-200 shadow-sm hover:shadow-xl transition-all group relative cursor-help"
          >
            <div className="flex justify-between items-start mb-4">
              <div className="p-2.5 rounded-xl bg-slate-50 text-slate-600 group-hover:bg-indigo-600 group-hover:text-white transition-colors">
                <p.icon className="w-4 h-4" />
              </div>
              <span className="text-[10px] font-black text-slate-300 uppercase tracking-widest">W: {p.weight}%</span>
            </div>
            <p className="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">{p.label}</p>
            <div className="flex items-baseline gap-1">
              <span className="text-2xl font-black text-slate-900">{p.val}</span>
              <span className="text-xs font-bold text-slate-400 italic">/ {p.target}</span>
            </div>
            <div className="mt-4 h-1.5 bg-slate-50 rounded-full overflow-hidden">
              <div className="h-full transition-all duration-1000" style={{ backgroundColor: p.color, width: `${(p.val / p.target) * 100}%` }} />
            </div>
            
            {activeMetric === p.id && (
              <div className="absolute inset-0 bg-slate-900/95 backdrop-blur-sm rounded-3xl p-6 text-white z-20 flex flex-col justify-center animate-in fade-in zoom-in-95 duration-200">
                <p className="text-[10px] font-black text-indigo-400 uppercase tracking-[0.2em] mb-2">{p.label} Analysis</p>
                <p className="text-xs leading-relaxed font-medium">{p.desc}</p>
                <div className="mt-4 pt-4 border-t border-white/10 flex items-center gap-2">
                  <Info className="w-3 h-3 text-indigo-400" />
                  <span className="text-[10px] font-bold">Influences {p.weight}% of Final Recovery Score</span>
                </div>
              </div>
            )}
          </div>
        ))}
      </div>
    </div>
  );
};

const Member360Dashboard = () => (
  <div className="space-y-6">
    <div className="flex justify-between items-center">
      <h2 className="text-2xl font-black text-slate-900">Member 360 Intelligence</h2>
      <div className="flex gap-2">
        <button className="bg-white border border-slate-200 px-4 py-2 rounded-xl text-sm font-bold flex items-center gap-2">
          <Share2 className="w-4 h-4" /> Sharing Insights
        </button>
        <button className="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-bold">New Assessment</button>
      </div>
    </div>

    <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <div className="lg:col-span-2 bg-white p-6 rounded-3xl border border-slate-200 shadow-sm">
        <h3 className="font-bold text-slate-900 mb-6 uppercase text-xs tracking-widest">Growth & Retention Funnel</h3>
        <div className="h-64">
          <ResponsiveContainer width="100%" height="100%">
            <AreaChart data={[{n:'Jan', m:1200, r:980},{n:'Feb', m:1450, r:1100},{n:'Mar', m:1800, r:1550}]}>
              <CartesianGrid strokeDasharray="3 3" vertical={false} />
              <XAxis dataKey="n" />
              <YAxis />
              <Tooltip />
              <Area type="monotone" dataKey="m" stroke="#6366f1" fill="#6366f1" fillOpacity={0.1} />
              <Area type="monotone" dataKey="r" stroke="#10b981" fill="#10b981" fillOpacity={0.1} />
            </AreaChart>
          </ResponsiveContainer>
        </div>
        <div className="flex gap-4 mt-4">
          <div className="flex items-center gap-2 text-xs font-bold"><span className="w-3 h-3 bg-indigo-500 rounded-full"></span> New Members</div>
          <div className="flex items-center gap-2 text-xs font-bold"><span className="w-3 h-3 bg-emerald-500 rounded-full"></span> Renewals</div>
        </div>
      </div>

      <div className="bg-slate-900 p-6 rounded-3xl text-white shadow-xl shadow-indigo-900/20">
        <h3 className="font-bold mb-6 text-indigo-400 uppercase text-xs tracking-widest">Transformation Reach</h3>
        <div className="space-y-4">
          <div className="p-4 bg-white/5 rounded-2xl border border-white/10">
            <p className="text-xs text-slate-400 font-bold uppercase mb-1">Social Referrals</p>
            <p className="text-2xl font-black text-white">458 <span className="text-emerald-400 text-sm">↑ 12%</span></p>
          </div>
          <div className="p-4 bg-white/5 rounded-2xl border border-white/10">
            <p className="text-xs text-slate-400 font-bold uppercase mb-1">Transformation Shares</p>
            <p className="text-2xl font-black text-white">2.4k <span className="text-indigo-400 text-sm">Active</span></p>
          </div>
          <button className="w-full py-3 bg-indigo-600 rounded-xl font-bold text-sm hover:bg-indigo-700 transition-colors">
            View Viral Metrics
          </button>
        </div>
      </div>
    </div>
  </div>
);

const TrainerOSDashboard = () => (
  <div className="space-y-6">
    <div className="flex justify-between items-center">
      <h2 className="text-2xl font-black text-slate-900">Trainer OS: Global Reach</h2>
      <p className="text-slate-500 text-sm">Cross-gym trainer sharing & expertise distribution</p>
    </div>

    <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
      {MOCK_TRAINERS.map(t => (
        <div key={t.id} className="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm relative overflow-hidden group">
          <div className="flex items-center gap-4 mb-6">
            <div className="w-12 h-12 bg-slate-100 rounded-2xl flex items-center justify-center font-bold text-indigo-600">{t.name[0]}</div>
            <div>
              <h4 className="font-bold text-slate-900">{t.name}</h4>
              <p className="text-xs text-slate-500">{t.specialty}</p>
            </div>
          </div>
          <div className="grid grid-cols-2 gap-4 mb-6">
            <div>
              <p className="text-[10px] font-black text-slate-400 uppercase">Total Reach</p>
              <p className="text-lg font-black text-slate-900">{t.clientReach}</p>
            </div>
            <div>
              <p className="text-[10px] font-black text-slate-400 uppercase">Retention</p>
              <p className="text-lg font-black text-emerald-600">{t.retention}%</p>
            </div>
          </div>
          <div className="w-full h-1 bg-slate-100 rounded-full overflow-hidden mb-4">
            <div className="h-full bg-indigo-500" style={{width: `${t.shareFactor * 10}%`}}></div>
          </div>
          <div className="flex justify-between items-center text-[10px] font-black text-slate-400 uppercase">
            <span>Market Share Factor</span>
            <span className="text-indigo-600">{t.shareFactor} / 10</span>
          </div>
          <div className="absolute top-4 right-4 text-amber-500 flex items-center gap-1 font-bold text-sm">
            <Star className="w-3 h-3 fill-current" /> {t.gymRating}
          </div>
        </div>
      ))}
    </div>
  </div>
);

const GrowthForecastDashboard = () => (
  <div className="space-y-6">
    <div className="flex justify-between items-center">
      <h2 className="text-2xl font-black text-slate-900 italic">National Growth Forecast</h2>
      <div className="bg-emerald-50 text-emerald-600 px-3 py-1 rounded-full text-xs font-bold border border-emerald-100 uppercase tracking-widest">India Market Live</div>
    </div>

    <div className="grid grid-cols-1 lg:grid-cols-4 gap-4">
      {NATIONAL_CHAINS.map(c => (
        <div key={c.name} className="bg-white p-5 rounded-2xl border border-slate-200 hover:shadow-lg transition-all border-l-4 border-l-indigo-500">
          <h4 className="font-black text-slate-900 mb-1">{c.name}</h4>
          <div className="flex justify-between text-xs font-bold text-slate-500 mb-3">
            <span>{c.branches} Branches</span>
            <span className={c.growth > 0 ? 'text-emerald-500' : 'text-rose-500'}>{c.growth > 0 ? '+' : ''}{c.growth}% Growth</span>
          </div>
          <div className="flex items-center justify-between text-[10px] font-black text-slate-400 uppercase tracking-widest">
            <span>{c.cityPresence} Cities</span>
            <span className="text-slate-900">{c.valuation}</span>
          </div>
        </div>
      ))}
    </div>

    <div className="bg-white p-8 rounded-[2rem] border border-slate-200 shadow-sm">
      <h3 className="font-black text-slate-900 mb-8 uppercase text-xs tracking-[0.2em]">Market Share Distribution (Ours vs Rivals)</h3>
      <div className="h-80">
        <ResponsiveContainer width="100%" height="100%">
          <BarChart data={NATIONAL_CHAINS}>
            <CartesianGrid strokeDasharray="3 3" vertical={false} />
            <XAxis dataKey="name" tick={{fontSize: 10, fontWeight: 'bold'}} />
            <YAxis hide />
            <Tooltip cursor={{fill: 'transparent'}} />
            <Bar dataKey="branches" radius={[10, 10, 0, 0]}>
              {NATIONAL_CHAINS.map((entry, index) => (
                <Cell key={`cell-${index}`} fill={entry.name.includes('Zyming') ? '#6366f1' : '#cbd5e1'} />
              ))}
            </Bar>
          </BarChart>
        </ResponsiveContainer>
      </div>
    </div>
  </div>
);

const SecurityGRCDashboard = () => (
  <div className="space-y-6">
    <div className="flex justify-between items-center">
      <h2 className="text-2xl font-black text-slate-900">GRC: Governance, Risk & DPDP</h2>
      <div className="flex items-center gap-2 text-xs font-bold text-slate-500">
        <Shield className="w-4 h-4 text-emerald-500" /> Compliance Tier: Grade A+
      </div>
    </div>

    <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div className="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm space-y-4">
        <div className="flex items-center gap-3 mb-2">
          <Globe className="w-5 h-5 text-indigo-500" />
          <h4 className="font-black text-slate-900 uppercase text-xs tracking-widest">DPDP Act Compliance</h4>
        </div>
        <div className="space-y-3">
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Consent Storage</span>
            <span className="text-emerald-600 font-black">Active (100%)</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Data Localisation</span>
            <span className="text-emerald-600 font-black">Verified (INDIA)</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Right to Erasure</span>
            <span className="text-indigo-600 font-black">Supported</span>
          </div>
        </div>
        <div className="pt-4 border-t border-slate-100">
          <button className="w-full py-2 bg-slate-50 text-slate-600 rounded-xl text-xs font-bold uppercase tracking-widest border border-slate-100">View DPDP Audit</button>
        </div>
      </div>

      <div className="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm space-y-4">
        <div className="flex items-center gap-3 mb-2">
          <Database className="w-5 h-5 text-amber-500" />
          <h4 className="font-black text-slate-900 uppercase text-xs tracking-widest">Backups & Recovery</h4>
        </div>
        <div className="space-y-3">
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Snapshot Frequency</span>
            <span className="text-slate-900 font-black">Every 6 Hours</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Geo-Redundancy</span>
            <span className="text-emerald-600 font-black">Mumbai/Delhi</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Last Restore Test</span>
            <span className="text-slate-900 font-black">4 Hours Ago</span>
          </div>
        </div>
        <div className="pt-4 border-t border-slate-100">
          <button className="w-full py-2 bg-slate-50 text-slate-600 rounded-xl text-xs font-bold uppercase tracking-widest border border-slate-100">Initialize DR Drill</button>
        </div>
      </div>

      <div className="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm space-y-4">
        <div className="flex items-center gap-3 mb-2">
          <FileText className="w-5 h-5 text-rose-500" />
          <h4 className="font-black text-slate-900 uppercase text-xs tracking-widest">SLA Performance</h4>
        </div>
        <div className="space-y-3">
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Uptime Metric</span>
            <span className="text-emerald-600 font-black">99.998%</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Support TAT</span>
            <span className="text-slate-900 font-black">12 Minutes</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="text-slate-500 font-medium">Security Scans</span>
            <span className="text-slate-900 font-black">1.2M Weekly</span>
          </div>
        </div>
        <div className="pt-4 border-t border-slate-100">
          <button className="w-full py-2 bg-slate-50 text-slate-600 rounded-xl text-xs font-bold uppercase tracking-widest border border-slate-100">Download SLA Report</button>
        </div>
      </div>
    </div>
  </div>
);

const BillingDashboard = () => (
  <div className="space-y-6">
    <div className="flex justify-between items-center">
      <h2 className="text-2xl font-black text-slate-900 italic">Enterprise SaaS Billing Engine</h2>
      <div className="flex gap-2">
        <button className="bg-white border border-slate-200 p-2 rounded-xl text-slate-400 hover:text-indigo-600 transition-colors"><Download className="w-5 h-5" /></button>
        <button className="bg-indigo-600 text-white px-6 py-2 rounded-xl text-sm font-black shadow-lg shadow-indigo-600/20">Upgrade Network</button>
      </div>
    </div>

    <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <div className="bg-white p-8 rounded-[2rem] border border-slate-200 shadow-sm relative overflow-hidden group">
        <div className="relative z-10">
          <p className="text-[10px] font-black text-slate-400 uppercase tracking-[0.2em] mb-4">Total Network Revenue Share</p>
          <p className="text-4xl font-black text-slate-900 italic">₹4.8L <span className="text-sm font-bold text-slate-400">/ Monthly</span></p>
          <div className="mt-8 pt-8 border-t border-slate-100 space-y-4">
            <div className="flex justify-between text-xs font-bold">
              <span className="text-slate-500 uppercase tracking-widest">PMPM Component</span>
              <span className="text-slate-900">₹2.4L (₹19/Member)</span>
            </div>
            <div className="flex justify-between text-xs font-bold">
              <span className="text-slate-500 uppercase tracking-widest">Platform Fees</span>
              <span className="text-slate-900">₹1.2L (Fixed)</span>
            </div>
            <div className="flex justify-between text-xs font-bold">
              <span className="text-slate-500 uppercase tracking-widest">Franchise Royalty (2%)</span>
              <span className="text-indigo-600">₹1.2L</span>
            </div>
          </div>
        </div>
        <div className="absolute -bottom-8 -right-8 w-32 h-32 bg-indigo-500/5 rounded-full blur-2xl group-hover:bg-indigo-500/10 transition-colors"></div>
      </div>

      <div className="lg:col-span-2 bg-slate-900 p-8 rounded-[2rem] text-white shadow-2xl relative overflow-hidden">
        <h3 className="font-black text-indigo-400 uppercase tracking-widest text-[10px] mb-8">PMPM Scaling Forecast</h3>
        <div className="h-48">
          <ResponsiveContainer width="100%" height="100%">
            <BarChart data={NATIONAL_CHAINS}>
              <Bar dataKey="branches" fill="#6366f1" radius={[4, 4, 0, 0]} />
            </BarChart>
          </ResponsiveContainer>
        </div>
        <div className="mt-6 flex justify-between items-center p-4 bg-white/5 rounded-2xl border border-white/10">
          <div>
            <p className="text-[10px] font-black text-slate-500 uppercase">Growth Opportunity</p>
            <p className="text-xs font-medium text-slate-300">Adding <span className="text-indigo-400 font-bold">10 more branches</span> increases PMPM yield by 14.5% due to volume tiers.</p>
          </div>
          <ChevronRight className="w-5 h-5 text-indigo-400" />
        </div>
      </div>
    </div>
  </div>
);

// --- MAIN APP ---

export default function App() {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const timer = setTimeout(() => setLoading(false), 800);
    return () => clearTimeout(timer);
  }, []);

  if (loading) {
    return (
      <div className="h-screen w-full flex flex-col items-center justify-center bg-slate-950">
        <div className="w-20 h-20 border-4 border-indigo-500/20 border-t-indigo-500 rounded-full animate-spin mb-6" />
        <p className="text-indigo-400 font-black tracking-[0.5em] text-[10px] uppercase">Booting National Core</p>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-slate-50 font-sans">
      <Sidebar activeTab={activeTab} setActiveTab={setActiveTab} />
      
      <main className="ml-64 min-h-screen flex flex-col">
        <header className="bg-white/80 backdrop-blur-md border-b border-slate-200 h-16 sticky top-0 z-40 px-8 flex justify-between items-center shadow-sm">
          <div className="flex items-center gap-4 text-slate-400 text-[10px] font-black uppercase tracking-widest">
            <span className="text-slate-900 border-b-2 border-indigo-500">IND-NODE-01</span>
            <ChevronRight className="w-3 h-3" />
            <span className="text-slate-900">National-Net</span>
            <ChevronRight className="w-3 h-3" />
            <span className="text-indigo-600">{activeTab.replace('-', ' ')}</span>
          </div>
          <div className="flex items-center gap-4">
            <div className="w-10 h-10 rounded-xl bg-slate-50 border border-slate-200 flex items-center justify-center text-slate-400 relative">
              <Bell className="w-5 h-5" />
              <span className="absolute top-2 right-2 w-2 h-2 bg-rose-500 rounded-full border border-white" />
            </div>
            <div className="bg-indigo-600 text-white px-4 py-1.5 rounded-xl text-[10px] font-black flex items-center gap-2 shadow-lg shadow-indigo-600/20">
              <Zap className="w-3 h-3 fill-current" /> SAAS LIVE
            </div>
          </div>
        </header>

        <div className="p-8 flex-1 max-w-[1600px] w-full mx-auto">
          {activeTab === 'dashboard' && (
            <div className="space-y-8 animate-in fade-in slide-in-from-bottom-4 duration-700">
               <div className="flex flex-col md:flex-row justify-between items-center gap-4">
                  <div>
                    <h1 className="text-3xl font-black text-slate-900 tracking-tight">Executive Command Tower</h1>
                    <p className="text-slate-500 text-sm font-medium">India's Central Fitness Intelligence Hub</p>
                  </div>
                  <div className="flex gap-2">
                    <button className="bg-white border border-slate-200 px-4 py-2.5 rounded-xl text-sm font-bold text-slate-700 flex items-center gap-2 shadow-sm">
                      <MapPin className="w-4 h-4 text-indigo-500" /> All-India
                    </button>
                    <button className="bg-indigo-600 px-6 py-2.5 rounded-xl text-sm font-bold text-white shadow-lg shadow-indigo-600/20 hover:bg-indigo-700 transition-all">
                      Generate PDF Insights
                    </button>
                  </div>
               </div>
               
               <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                  <div className="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                    <TrendingUp className="w-5 h-5 text-indigo-500 mb-4" />
                    <p className="text-[10px] font-black text-slate-400 uppercase tracking-widest">Network ARR</p>
                    <h3 className="text-2xl font-black text-slate-900 italic">₹12.4 Cr</h3>
                  </div>
                  <div className="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                    <Users className="w-5 h-5 text-blue-500 mb-4" />
                    <p className="text-[10px] font-black text-slate-400 uppercase tracking-widest">Active Members</p>
                    <h3 className="text-2xl font-black text-slate-900 italic">85,200</h3>
                  </div>
                  <div className="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                    <ShieldCheck className="w-5 h-5 text-emerald-500 mb-4" />
                    <p className="text-[10px] font-black text-slate-400 uppercase tracking-widest">GRC Compliance</p>
                    <h3 className="text-2xl font-black text-slate-900 italic">Grade A+</h3>
                  </div>
                  <div className="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                    <Activity className="w-5 h-5 text-rose-500 mb-4" />
                    <p className="text-[10px] font-black text-slate-400 uppercase tracking-widest">SLA Uptime</p>
                    <h3 className="text-2xl font-black text-slate-900 italic">99.99%</h3>
                  </div>
               </div>

               <GrowthForecastDashboard />
            </div>
          )}
          
          {activeTab === 'branches' && (
             <div className="space-y-6">
                <div className="flex justify-between items-center">
                  <h2 className="text-2xl font-black text-slate-900 tracking-tight">Branch Network Reach</h2>
                  <div className="relative">
                    <Search className="w-4 h-4 absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" />
                    <input type="text" placeholder="Locate branch node..." className="pl-10 pr-4 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:ring-2 focus:ring-indigo-500 outline-none w-64" />
                  </div>
                </div>
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                  {FARIDABAD_GYMS.map(g => (
                    <div key={g.id} className="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm group hover:border-indigo-500 transition-all">
                      <div className="flex justify-between items-start mb-6">
                        <div className="w-12 h-12 bg-slate-50 rounded-2xl flex items-center justify-center text-slate-400 group-hover:text-indigo-600 transition-colors">
                          <Building2 className="w-6 h-6" />
                        </div>
                        <div className="text-right">
                          <span className="text-[10px] font-black bg-indigo-50 text-indigo-600 px-2 py-0.5 rounded-full border border-indigo-100 uppercase tracking-widest">Reach Index: {g.reach}</span>
                        </div>
                      </div>
                      <h4 className="font-black text-slate-900 text-lg mb-1">{g.name}</h4>
                      <p className="text-xs font-bold text-slate-500 uppercase tracking-widest mb-4"><MapPin className="w-3 h-3 inline mr-1" /> {g.sector}</p>
                      <div className="grid grid-cols-2 gap-4 py-4 border-t border-slate-100">
                        <div>
                          <p className="text-[10px] font-black text-slate-400 uppercase">Members</p>
                          <p className="font-bold text-slate-900">{g.members}</p>
                        </div>
                        <div>
                          <p className="text-[10px] font-black text-slate-400 uppercase">Trainers</p>
                          <p className="font-bold text-slate-900">{g.trainers}</p>
                        </div>
                      </div>
                    </div>
                  ))}
                </div>
             </div>
          )}

          {activeTab === 'members' && <Member360Dashboard />}
          {activeTab === 'trainer-os' && <TrainerOSDashboard />}
          {activeTab === 'fatigue-index' && <AIFatigueEngine />}
          {activeTab === 'growth' && <GrowthForecastDashboard />}
          {activeTab === 'security' && <SecurityGRCDashboard />}
          {activeTab === 'billing' && <BillingDashboard />}

          {!['dashboard', 'branches', 'members', 'trainer-os', 'growth', 'security', 'billing', 'fatigue-index'].includes(activeTab) && (
            <div className="flex flex-col items-center justify-center py-32 text-slate-400">
              <Settings className="w-16 h-16 mb-4 opacity-20 animate-[spin_10s_linear_infinite]" />
              <h3 className="text-xl font-black text-slate-900 italic tracking-tight uppercase">Module Initializing</h3>
              <p className="text-sm mt-2 font-medium">The {activeTab.replace('-', ' ')} node is under national cluster synchronization.</p>
              <button onClick={() => setActiveTab('dashboard')} className="mt-8 px-6 py-2 bg-indigo-600 text-white rounded-xl text-xs font-black uppercase tracking-widest">Return to Tower</button>
            </div>
          )}
        </div>
        
        <footer className="px-8 py-8 border-t border-slate-200 bg-white/30 text-slate-400 text-[10px] font-black flex flex-col md:flex-row justify-between items-center gap-4">
          <div className="flex items-center gap-4 uppercase tracking-widest">
            <p>© 2026 Zyming AI India Infrastructure</p>
            <span className="hidden md:inline text-slate-200">|</span>
            <p className="italic">DPDP 2023 Compliant Platform</p>
          </div>
          <div className="flex gap-8 uppercase tracking-widest">
            <span className="hover:text-indigo-600 cursor-pointer transition-colors">GDPR/DPDP Audit</span>
            <span className="hover:text-indigo-600 cursor-pointer transition-colors">SLA Console</span>
            <span className="hover:text-indigo-600 cursor-pointer transition-colors">Cloud Node: MUM-1</span>
          </div>
        </footer>
      </main>
    </div>
  );
}
