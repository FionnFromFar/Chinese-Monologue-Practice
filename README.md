# Chinese-Monologue-Practice
Hosting a wabpage for my monologue practice
<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chinese Monologue Pronunciation Trainer</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            900: '#14532d',
                        },
                        accent: {
                            50: '#fef2f2',
                            500: '#ef4444',
                            600: '#dc2626',
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
                        kai: ['KaiTi', 'STKaiti', 'DFKai-SB', 'serif']
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Noto+Sans+SC:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', 'Noto Sans SC', sans-serif;
        }
        .chinese-text {
            font-family: 'Noto Sans SC', 'KaiTi', sans-serif;
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
        .active-sentence-glow {
            box-shadow: 0 0 0 2px #22c55e, 0 10px 25px -5px rgba(34, 197, 94, 0.15);
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-full flex flex-col justify-between selection:bg-brand-500 selection:text-white">

    <!-- Header Section -->
    <header class="bg-slate-800/80 backdrop-blur border-b border-slate-700/60 sticky top-0 z-50">
        <div class="max-w-5xl mx-auto px-4 py-4 sm:px-6 flex flex-col sm:flex-row items-center justify-between gap-4">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-600 to-emerald-400 flex items-center justify-center text-white shadow-lg shadow-brand-500/20 font-bold text-xl">
                    漢
                </div>
                <div>
                    <h1 class="text-xl font-bold text-white tracking-tight flex items-center gap-2">
                        Chinese Monologue Trainer
                        <span class="text-xs font-normal px-2 py-0.5 rounded-full bg-brand-500/20 text-brand-400 border border-brand-500/30">HKUST Speech</span>
                    </h1>
                    <p class="text-xs text-slate-400">Interactive Audio Pronunciation & Pinyin Guide</p>
                </div>
            </div>

            <!-- Voice Selection Dropdown -->
            <div class="w-full sm:w-auto flex items-center gap-2 bg-slate-900/80 p-1.5 rounded-lg border border-slate-700">
                <svg class="w-4 h-4 text-brand-400 ml-2 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 100-6 3 3 0 000 6z"></path>
                </svg>
                <select id="voiceSelect" class="bg-transparent text-xs text-slate-200 outline-none w-full sm:w-56 cursor-pointer py-1 pr-2 truncate">
                    <option value="" disabled selected class="bg-slate-800">Loading Chinese voices...</option>
                </select>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-5xl mx-auto px-4 py-6 sm:px-6 flex-1 w-full grid grid-cols-1 lg:grid-cols-12 gap-6">

        <!-- Left Column: Audio Controls & Player Settings (Stickied on Desktop) -->
        <div class="lg:col-span-5 space-y-5">
            
            <!-- Main Audio Control Panel -->
            <div class="bg-slate-800/90 rounded-2xl p-5 border border-slate-700/80 shadow-xl space-y-5 sticky top-24">
                
                <!-- Status Banner -->
                <div id="statusBadge" class="flex items-center justify-between bg-slate-900/60 px-3.5 py-2 rounded-xl border border-slate-700/50 text-xs">
                    <span class="flex items-center gap-2 text-slate-400">
                        <span id="statusDot" class="w-2.5 h-2.5 rounded-full bg-slate-500"></span>
                        <span id="statusText">Ready to Play</span>
                    </span>
                    <span id="sentenceCounter" class="text-slate-400 font-mono">0 / 6</span>
                </div>

                <!-- Primary Playback Controls -->
                <div class="flex items-center justify-center gap-3">
                    <button id="prevBtn" title="Previous Sentence" class="p-3 rounded-full bg-slate-700/70 hover:bg-slate-700 active:scale-95 text-slate-200 transition">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"></path></svg>
                    </button>

                    <button id="playBtn" title="Play / Resume" class="w-14 h-14 rounded-full bg-gradient-to-tr from-brand-600 to-emerald-500 hover:from-brand-500 hover:to-emerald-400 text-slate-950 flex items-center justify-center font-bold shadow-lg shadow-brand-500/25 active:scale-95 transition">
                        <svg id="playIcon" class="w-7 h-7 translate-x-0.5" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg>
                        <svg id="pauseIcon" class="w-7 h-7 hidden" fill="currentColor" viewBox="0 0 24 24"><path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/></svg>
                    </button>

                    <button id="stopBtn" title="Stop" class="p-3 rounded-full bg-slate-700/70 hover:bg-slate-700 active:scale-95 text-slate-200 transition">
                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M6 6h12v12H6z"/></svg>
                    </button>

                    <button id="nextBtn" title="Next Sentence" class="p-3 rounded-full bg-slate-700/70 hover:bg-slate-700 active:scale-95 text-slate-200 transition">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path></svg>
                    </button>
                </div>

                <!-- Speed / Rate Control Slider -->
                <div class="space-y-2 pt-2 border-t border-slate-700/50">
                    <div class="flex items-center justify-between text-xs">
                        <label for="rateSlider" class="font-medium text-slate-300 flex items-center gap-1.5">
                            <svg class="w-4 h-4 text-brand-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                            Speech Speed (Rate)
                        </label>
                        <span id="rateVal" class="font-mono text-brand-400 font-bold bg-brand-500/10 px-2 py-0.5 rounded border border-brand-500/20">1.0x</span>
                    </div>
                    <input type="range" id="rateSlider" min="0.5" max="1.5" step="0.05" value="1.0" class="w-full accent-brand-500 h-1.5 bg-slate-700 rounded-lg appearance-none cursor-pointer">
                    
                    <!-- Speed Presets -->
                    <div class="grid grid-cols-4 gap-1.5 pt-1">
                        <button data-rate="0.5" class="speed-preset text-xs py-1 rounded bg-slate-700/60 hover:bg-slate-700 text-slate-300 font-mono transition">0.5x (Slow)</button>
                        <button data-rate="0.75" class="speed-preset text-xs py-1 rounded bg-slate-700/60 hover:bg-slate-700 text-slate-300 font-mono transition">0.75x</button>
                        <button data-rate="1.0" class="speed-preset text-xs py-1 rounded bg-brand-500/20 text-brand-300 border border-brand-500/30 font-mono font-bold transition">1.0x (Normal)</button>
                        <button data-rate="1.25" class="speed-preset text-xs py-1 rounded bg-slate-700/60 hover:bg-slate-700 text-slate-300 font-mono transition">1.25x</button>
                    </div>
                </div>

                <!-- Voice Pitch Slider -->
                <div class="space-y-1.5 pt-2 border-t border-slate-700/50">
                    <div class="flex items-center justify-between text-xs">
                        <label for="pitchSlider" class="font-medium text-slate-400">Voice Pitch</label>
                        <span id="pitchVal" class="font-mono text-slate-400 text-[11px]">1.0</span>
                    </div>
                    <input type="range" id="pitchSlider" min="0.7" max="1.3" step="0.05" value="1.0" class="w-full accent-emerald-400 h-1 bg-slate-700 rounded-lg appearance-none cursor-pointer">
                </div>

                <!-- Display & Behavior Toggles -->
                <div class="pt-3 border-t border-slate-700/50 space-y-2.5 text-xs">
                    <div class="flex items-center justify-between">
                        <span class="text-slate-300 flex items-center gap-1.5">
                            <svg class="w-4 h-4 text-emerald-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path></svg>
                            Loop Active Sentence
                        </span>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="loopToggle" class="sr-only peer">
                            <div class="w-9 h-5 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-brand-600"></div>
                        </label>
                    </div>

                    <div class="flex items-center justify-between">
                        <span class="text-slate-300">Show Pinyin</span>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="pinyinToggle" checked class="sr-only peer">
                            <div class="w-9 h-5 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-brand-600"></div>
                        </label>
                    </div>

                    <div class="flex items-center justify-between">
                        <span class="text-slate-300">Show English Translation</span>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="englishToggle" checked class="sr-only peer">
                            <div class="w-9 h-5 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-brand-600"></div>
                        </label>
                    </div>
                </div>

            </div>
            
            <!-- Quick Tips Card -->
            <div class="bg-slate-800/50 rounded-xl p-4 border border-slate-700/50 text-xs text-slate-400 space-y-2">
                <div class="font-semibold text-slate-300 flex items-center gap-1">
                    <span>💡 Pronunciation Practice Tip</span>
                </div>
                <p>Listen at <strong>0.75x speed</strong> to examine sentence rhythm and third-tone sandhi (e.g. <em>nǐ hǎo</em> $\rightarrow$ <em>ní hǎo</em>). Click any sentence card directly to play it instantly!</p>
            </div>
        </div>

        <!-- Right Column: Interactive Monologue Sentence Breakdown -->
        <div class="lg:col-span-7 space-y-4">
            
            <!-- Section Title & Word Breakdown Toggle -->
            <div class="flex items-center justify-between">
                <h2 class="text-sm font-semibold uppercase tracking-wider text-slate-400">
                    Monologue Script (6 Sentences)
                </h2>
                <button id="toggleBreakdownBtn" class="text-xs text-brand-400 hover:text-brand-300 underline font-medium">
                    Show Word Vocabulary Breakdown
                </button>
            </div>

            <!-- Sentences List Container -->
            <div id="sentencesContainer" class="space-y-4">
                <!-- Javascript will populate sentence cards dynamically -->
            </div>

        </div>

    </main>

    <!-- Footer -->
    <footer class="bg-slate-900 border-t border-slate-800 py-4 text-center text-xs text-slate-500">
        <p>HKUST Chinese Language Speech Monologue Companion • Powered by Web Speech API</p>
    </footer>

    <!-- Application Script -->
    <script>
        // Monologue Data with Pinyin, Hanzi, English, and Word Breakdowns
        const MONOLOGUE_DATA = [
            {
                id: 0,
                hanzi: "你好！我叫 Fionn，我的中文名字是飞龙。",
                pinyin: "Nǐ hǎo! Wǒ jiào Fionn, wǒ de zhōngwén míngzi shì Fēilóng.",
                english: "Hello! My name is Fionn, my Chinese name is Fei Long.",
                words: [
                    { hanzi: "你好", pinyin: "nǐ hǎo", en: "hello" },
                    { hanzi: "我", pinyin: "wǒ", en: "I / me" },
                    { hanzi: "叫", pinyin: "jiào", en: "to be called" },
                    { hanzi: "Fionn", pinyin: "Fionn", en: "Fionn (name)" },
                    { hanzi: "我的", pinyin: "wǒ de", en: "my" },
                    { hanzi: "中文", pinyin: "zhōngwén", en: "Chinese" },
                    { hanzi: "名字", pinyin: "míngzi", en: "name" },
                    { hanzi: "是", pinyin: "shì", en: "is / am" },
                    { hanzi: "飞龙", pinyin: "fēilóng", en: "Fei Long" }
                ]
            },
            {
                id: 1,
                hanzi: "我是英国人，但是我在香港科技大学学习。",
                pinyin: "Wǒ shì Yīngguórén, dànshì wǒ zài Xiānggǎng Kējì Dàxué xuéxí.",
                english: "I am British, but I study at Hong Kong University of Science and Technology (HKUST).",
                words: [
                    { hanzi: "我", pinyin: "wǒ", en: "I" },
                    { hanzi: "是", pinyin: "shì", en: "am" },
                    { hanzi: "英国人", pinyin: "yīngguórén", en: "British person" },
                    { hanzi: "但是", pinyin: "dànshì", en: "but" },
                    { hanzi: "我", pinyin: "wǒ", en: "I" },
                    { hanzi: "在", pinyin: "zài", en: "at" },
                    { hanzi: "香港科技大学", pinyin: "xiānggǎng kējì dàxué", en: "HKUST" },
                    { hanzi: "学习", pinyin: "xuéxí", en: "study" }
                ]
            },
            {
                id: 2,
                hanzi: "因为我才学了两个月的中文，所以我说中文说得还不太好。",
                pinyin: "Yīnwèi wǒ cái xué le liǎng gè yuè de zhōngwén, suǒyǐ wǒ shuō zhōngwén shuō de hái bù tài hǎo.",
                english: "Because I've only been learning Chinese for two months, I speak Chinese not very well yet.",
                words: [
                    { hanzi: "因为", pinyin: "yīnwèi", en: "because" },
                    { hanzi: "我", pinyin: "wǒ", en: "I" },
                    { hanzi: "才", pinyin: "cái", en: "only / just" },
                    { hanzi: "学了", pinyin: "xué le", en: "learned" },
                    { hanzi: "两个月", pinyin: "liǎng gè yuè", en: "2 months" },
                    { hanzi: "的", pinyin: "de", en: "of" },
                    { hanzi: "中文", pinyin: "zhōngwén", en: "Chinese" },
                    { hanzi: "所以", pinyin: "suǒyǐ", en: "so" },
                    { hanzi: "我", pinyin: "wǒ", en: "I" },
                    { hanzi: "说", pinyin: "shuō", en: "speak" },
                    { hanzi: "中文", pinyin: "zhōngwén", en: "Chinese" },
                    { hanzi: "说得", pinyin: "shuō de", en: "speak (degree)" },
                    { hanzi: "还", pinyin: "hái", en: "still / yet" },
                    { hanzi: "不太好", pinyin: "bù tài hǎo", en: "not very well" }
                ]
            },
            {
                id: 3,
                hanzi: "我觉得学中文很难，最难的是写汉字。",
                pinyin: "Wǒ juéde xué zhōngwén hěn nán, zuì nán de shì xiě hànzì.",
                english: "I think learning Chinese is very hard, the hardest part is writing Chinese characters.",
                words: [
                    { hanzi: "我", pinyin: "wǒ", en: "I" },
                    { hanzi: "觉得", pinyin: "juéde", en: "feel / think" },
                    { hanzi: "学", pinyin: "xué", en: "learning" },
                    { hanzi: "中文", pinyin: "zhōngwén", en: "Chinese" },
                    { hanzi: "很难", pinyin: "hěn nán", en: "very hard" },
                    { hanzi: "最", pinyin: "zuì", en: "most" },
                    { hanzi: "难", pinyin: "nán", en: "difficult" },
                    { hanzi: "的", pinyin: "de", en: "thing" },
                    { hanzi: "是", pinyin: "shì", en: "is" },
                    { hanzi: "写", pinyin: "xiě", en: "write" },
                    { hanzi: "汉字", pinyin: "hànzì", en: "characters" }
                ]
            },
            {
                id: 4,
                hanzi: "我住在学生宿舍，所以我早上十点起床、洗澡，然后走去上课。",
                pinyin: "Wǒ zhù zài xuésheng sùshè, suǒyǐ wǒ zǎoshang shí diǎn qǐchuáng, xǐzǎo, ránhòu zǒu qù shàngkè.",
                english: "I live in student dormitories, so I wake up at 10am, shower, and then walk to attend class.",
                words: [
                    { hanzi: "我", pinyin: "wǒ", en: "I" },
                    { hanzi: "住在", pinyin: "zhù zài", en: "live in" },
                    { hanzi: "学生宿舍", pinyin: "xuésheng sùshè", en: "student dorm" },
                    { hanzi: "所以", pinyin: "suǒyǐ", en: "so" },
                    { hanzi: "我", pinyin: "wǒ", en: "I" },
                    { hanzi: "早上", pinyin: "zǎoshang", en: "morning" },
                    { hanzi: "十点", pinyin: "shí diǎn", en: "10 o'clock" },
                    { hanzi: "起床", pinyin: "qǐchuáng", en: "get up" },
                    { hanzi: "洗澡", pinyin: "xǐzǎo", en: "shower" },
                    { hanzi: "然后", pinyin: "ránhòu", en: "then" },
                    { hanzi: "走去", pinyin: "zǒu qù", en: "walk to" },
                    { hanzi: "上课", pinyin: "shàngkè", en: "attend class" }
                ]
            },
            {
                id: 5,
                hanzi: "除了中文以外，我还学物理。",
                pinyin: "Chúle zhōngwén yǐwài, wǒ hái xué wùlǐ.",
                english: "Besides Chinese, I also study physics.",
                words: [
                    { hanzi: "除了", pinyin: "chúle", en: "besides" },
                    { hanzi: "中文", pinyin: "zhōngwén", en: "Chinese" },
                    { hanzi: "以外", pinyin: "yǐwài", en: "apart from" },
                    { hanzi: "我", pinyin: "wǒ", en: "I" },
                    { hanzi: "还", pinyin: "hái", en: "also" },
                    { hanzi: "学", pinyin: "xué", en: "study" },
                    { hanzi: "物理", pinyin: "wùlǐ", en: "physics" }
                ]
            }
        ];

        // State variables
        let synth = window.speechSynthesis;
        let voices = [];
        let currentVoice = null;
        let currentSentenceIdx = 0;
        let isPlaying = false;
        let isPaused = false;
        let showBreakdown = false;

        // DOM elements
        const sentencesContainer = document.getElementById('sentencesContainer');
        const playBtn = document.getElementById('playBtn');
        const playIcon = document.getElementById('playIcon');
        const pauseIcon = document.getElementById('pauseIcon');
        const stopBtn = document.getElementById('stopBtn');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
        const rateSlider = document.getElementById('rateSlider');
        const rateVal = document.getElementById('rateVal');
        const pitchSlider = document.getElementById('pitchSlider');
        const pitchVal = document.getElementById('pitchVal');
        const voiceSelect = document.getElementById('voiceSelect');
        const statusBadge = document.getElementById('statusBadge');
        const statusDot = document.getElementById('statusDot');
        const statusText = document.getElementById('statusText');
        const sentenceCounter = document.getElementById('sentenceCounter');
        const loopToggle = document.getElementById('loopToggle');
        const pinyinToggle = document.getElementById('pinyinToggle');
        const englishToggle = document.getElementById('englishToggle');
        const toggleBreakdownBtn = document.getElementById('toggleBreakdownBtn');

        // Initialize Speech Voices
        function populateVoiceList() {
            if (!synth) return;
            voices = synth.getVoices();
            voiceSelect.innerHTML = '';

            // Filter for Chinese voices first
            const zhVoices = voices.filter(v => v.lang.includes('zh') || v.lang.includes('CN') || v.lang.includes('HK') || v.lang.includes('TW') || v.name.toLowerCase().includes('chinese'));
            const otherVoices = voices.filter(v => !zhVoices.includes(v));

            if (voices.length === 0) {
                const option = document.createElement('option');
                option.textContent = 'Default Browser Chinese Voice';
                voiceSelect.appendChild(option);
                return;
            }

            // Populate options grouped
            const groupZh = document.createElement('optgroup');
            groupZh.label = 'Mandarin / Chinese Voices';

            zhVoices.forEach((voice) => {
                const option = document.createElement('option');
                option.textContent = `${voice.name} (${voice.lang})`;
                option.setAttribute('data-name', voice.name);
                option.setAttribute('data-lang', voice.lang);
                if (voice.default || voice.lang === 'zh-CN') {
                    option.selected = true;
                    currentVoice = voice;
                }
                groupZh.appendChild(option);
            });

            voiceSelect.appendChild(groupZh);

            // Add other voices as fallback group if needed
            if (otherVoices.length > 0) {
                const groupOther = document.createElement('optgroup');
                groupOther.label = 'Other System Voices';
                otherVoices.forEach((voice) => {
                    const option = document.createElement('option');
                    option.textContent = `${voice.name} (${voice.lang})`;
                    option.setAttribute('data-name', voice.name);
                    option.setAttribute('data-lang', voice.lang);
                    groupOther.appendChild(option);
                });
                voiceSelect.appendChild(groupOther);
            }

            if (!currentVoice && zhVoices.length > 0) {
                currentVoice = zhVoices[0];
            } else if (!currentVoice && voices.length > 0) {
                currentVoice = voices[0];
            }
        }

        if (speechSynthesis.onvoiceschanged !== undefined) {
            speechSynthesis.onvoiceschanged = populateVoiceList;
        }

        // Render Sentences UI
        function renderSentences() {
            sentencesContainer.innerHTML = '';
            MONOLOGUE_DATA.forEach((data, index) => {
                const card = document.createElement('div');
                card.id = `sentence-card-${index}`;
                card.className = `p-5 rounded-2xl bg-slate-800/60 border border-slate-700/60 transition-all duration-300 cursor-pointer hover:border-slate-600 ${
                    index === currentSentenceIdx ? 'active-sentence-glow bg-slate-800 border-brand-500/50' : ''
                }`;
                card.onclick = () => playSingleSentence(index);

                // Sentence Header (Index & Quick Audio Button)
                const headerHtml = `
                    <div class="flex items-center justify-between mb-2">
                        <span class="text-xs font-mono font-medium text-slate-400 bg-slate-900/60 px-2.5 py-1 rounded-md border border-slate-700/50">
                            Sentence ${index + 1} of ${MONOLOGUE_DATA.length}
                        </span>
                        <button onclick="event.stopPropagation(); playSingleSentence(${index})" class="flex items-center gap-1.5 text-xs text-brand-400 hover:text-brand-300 bg-brand-500/10 hover:bg-brand-500/20 px-3 py-1 rounded-full border border-brand-500/20 transition">
                            <svg class="w-3.5 h-3.5" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg>
                            <span>Listen</span>
                        </button>
                    </div>
                `;

                // Hanzi text
                const hanziHtml = `
                    <div class="text-xl sm:text-2xl font-medium text-slate-100 chinese-text leading-relaxed tracking-wide my-2">
                        ${data.hanzi}
                    </div>
                `;

                // Pinyin text
                const pinyinHtml = `
                    <div class="sentence-pinyin text-sm sm:text-base font-mono text-emerald-400 my-1 font-medium ${pinyinToggle.checked ? '' : 'hidden'}">
                        ${data.pinyin}
                    </div>
                `;

                // English translation
                const englishHtml = `
                    <div class="sentence-english text-xs sm:text-sm text-slate-400 mt-2 font-sans italic border-t border-slate-700/40 pt-2 ${englishToggle.checked ? '' : 'hidden'}">
                        "${data.english}"
                    </div>
                `;

                // Word Breakdown pills
                let wordsHtml = '';
                if (showBreakdown) {
                    const pills = data.words.map(w => `
                        <div class="inline-flex flex-col bg-slate-900/80 border border-slate-700 px-2.5 py-1.5 rounded-lg text-center">
                            <span class="chinese-text text-sm font-semibold text-slate-200">${w.hanzi}</span>
                            <span class="text-[11px] font-mono text-brand-400">${w.pinyin}</span>
                            <span class="text-[10px] text-slate-400">${w.en}</span>
                        </div>
                    `).join('');
                    wordsHtml = `
                        <div class="mt-3 pt-3 border-t border-slate-700/50 flex flex-wrap gap-1.5">
                            ${pills}
                        </div>
                    `;
                }

                card.innerHTML = headerHtml + hanziHtml + pinyinHtml + englishHtml + wordsHtml;
                sentencesContainer.appendChild(card);
            });
            updateSentenceCounter();
        }

        // Highlight Active Sentence
        function highlightSentence(index) {
            MONOLOGUE_DATA.forEach((_, i) => {
                const card = document.getElementById(`sentence-card-${i}`);
                if (card) {
                    if (i === index) {
                        card.classList.add('active-sentence-glow', 'bg-slate-800', 'border-brand-500/50');
                        card.classList.remove('bg-slate-800/60', 'border-slate-700/60');
                        // Scroll smoothly into view
                        card.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
                    } else {
                        card.classList.remove('active-sentence-glow', 'bg-slate-800', 'border-brand-500/50');
                        card.classList.add('bg-slate-800/60', 'border-slate-700/60');
                    }
                }
            });
            currentSentenceIdx = index;
            updateSentenceCounter();
        }

        function updateSentenceCounter() {
            sentenceCounter.textContent = `${currentSentenceIdx + 1} / ${MONOLOGUE_DATA.length}`;
        }

        // Playback Engine
        function speakCurrentSentence() {
            if (!synth) return;
            synth.cancel(); // Clear queue

            const item = MONOLOGUE_DATA[currentSentenceIdx];
            if (!item) return;

            const utterance = new SpeechSynthesisUtterance(item.hanzi);
            utterance.rate = parseFloat(rateSlider.value);
            utterance.pitch = parseFloat(pitchSlider.value);

            if (currentVoice) {
                utterance.voice = currentVoice;
            } else {
                utterance.lang = 'zh-CN';
            }

            utterance.onstart = () => {
                isPlaying = true;
                isPaused = false;
                updateUIState('playing');
                highlightSentence(currentSentenceIdx);
            };

            utterance.onend = () => {
                if (!isPlaying) return;

                if (loopToggle.checked) {
                    // Repeat current sentence
                    speakCurrentSentence();
                } else if (currentSentenceIdx < MONOLOGUE_DATA.length - 1) {
                    // Advance to next
                    currentSentenceIdx++;
                    speakCurrentSentence();
                } else {
                    // Reached end of monologue
                    stopSpeech();
                    updateUIState('finished');
                }
            };

            utterance.onerror = (e) => {
                console.error("Speech error:", e);
                updateUIState('idle');
            };

            synth.speak(utterance);
        }

        function playSingleSentence(index) {
            currentSentenceIdx = index;
            speakCurrentSentence();
        }

        function togglePlayPause() {
            if (!synth) return;

            if (isPlaying && !isPaused) {
                synth.pause();
                isPaused = true;
                updateUIState('paused');
            } else if (isPaused) {
                synth.resume();
                isPaused = false;
                isPlaying = true;
                updateUIState('playing');
            } else {
                speakCurrentSentence();
            }
        }

        function stopSpeech() {
            if (!synth) return;
            synth.cancel();
            isPlaying = false;
            isPaused = false;
            updateUIState('idle');
        }

        function nextSentence() {
            if (currentSentenceIdx < MONOLOGUE_DATA.length - 1) {
                currentSentenceIdx++;
            } else {
                currentSentenceIdx = 0;
            }
            if (isPlaying) {
                speakCurrentSentence();
            } else {
                highlightSentence(currentSentenceIdx);
            }
        }

        function prevSentence() {
            if (currentSentenceIdx > 0) {
                currentSentenceIdx--;
            } else {
                currentSentenceIdx = MONOLOGUE_DATA.length - 1;
            }
            if (isPlaying) {
                speakCurrentSentence();
            } else {
                highlightSentence(currentSentenceIdx);
            }
        }

        // Update UI Status Indicators
        function updateUIState(state) {
            if (state === 'playing') {
                playIcon.classList.add('hidden');
                pauseIcon.classList.remove('hidden');
                statusDot.className = 'w-2.5 h-2.5 rounded-full bg-brand-500 animate-pulse';
                statusText.textContent = `Speaking Sentence ${currentSentenceIdx + 1}...`;
                statusBadge.className = 'flex items-center justify-between bg-brand-500/10 px-3.5 py-2 rounded-xl border border-brand-500/30 text-xs text-brand-300';
            } else if (state === 'paused') {
                playIcon.classList.remove('hidden');
                pauseIcon.classList.add('hidden');
                statusDot.className = 'w-2.5 h-2.5 rounded-full bg-amber-400';
                statusText.textContent = 'Paused';
                statusBadge.className = 'flex items-center justify-between bg-amber-500/10 px-3.5 py-2 rounded-xl border border-amber-500/30 text-xs text-amber-300';
            } else if (state === 'finished') {
                playIcon.classList.remove('hidden');
                pauseIcon.classList.add('hidden');
                statusDot.className = 'w-2.5 h-2.5 rounded-full bg-emerald-400';
                statusText.textContent = 'Finished Monologue';
                statusBadge.className = 'flex items-center justify-between bg-emerald-500/10 px-3.5 py-2 rounded-xl border border-emerald-500/30 text-xs text-emerald-300';
            } else { // Idle
                playIcon.classList.remove('hidden');
                pauseIcon.classList.add('hidden');
                statusDot.className = 'w-2.5 h-2.5 rounded-full bg-slate-500';
                statusText.textContent = 'Ready to Play';
                statusBadge.className = 'flex items-center justify-between bg-slate-900/60 px-3.5 py-2 rounded-xl border border-slate-700/50 text-xs text-slate-400';
            }
        }

        // Event Listeners
        playBtn.addEventListener('click', togglePlayPause);
        stopBtn.addEventListener('click', stopSpeech);
        nextBtn.addEventListener('click', nextSentence);
        prevBtn.addEventListener('click', prevSentence);

        // Rate Slider
        rateSlider.addEventListener('input', (e) => {
            const val = parseFloat(e.target.value).toFixed(2);
            rateVal.textContent = `${val}x`;
            // Update active preset button highlight
            document.querySelectorAll('.speed-preset').forEach(btn => {
                if (parseFloat(btn.dataset.rate) === parseFloat(val)) {
                    btn.className = 'speed-preset text-xs py-1 rounded bg-brand-500/20 text-brand-300 border border-brand-500/30 font-mono font-bold transition';
                } else {
                    btn.className = 'speed-preset text-xs py-1 rounded bg-slate-700/60 hover:bg-slate-700 text-slate-300 font-mono transition';
                }
            });
            if (isPlaying && !isPaused) {
                speakCurrentSentence(); // Apply speed change dynamically
            }
        });

        // Speed Preset Buttons
        document.querySelectorAll('.speed-preset').forEach(btn => {
            btn.addEventListener('click', () => {
                const rate = btn.dataset.rate;
                rateSlider.value = rate;
                rateSlider.dispatchEvent(new Event('input'));
            });
        });

        // Pitch Slider
        pitchSlider.addEventListener('input', (e) => {
            pitchVal.textContent = parseFloat(e.target.value).toFixed(2);
        });

        // Voice Select
        voiceSelect.addEventListener('change', () => {
            const selectedOption = voiceSelect.selectedOptions[0];
            const name = selectedOption.getAttribute('data-name');
            currentVoice = voices.find(v => v.name === name) || null;
            if (isPlaying) {
                speakCurrentSentence();
            }
        });

        // Display Toggles
        pinyinToggle.addEventListener('change', (e) => {
            document.querySelectorAll('.sentence-pinyin').forEach(el => {
                if (e.target.checked) el.classList.remove('hidden');
                else el.classList.add('hidden');
            });
        });

        englishToggle.addEventListener('change', (e) => {
            document.querySelectorAll('.sentence-english').forEach(el => {
                if (e.target.checked) el.classList.remove('hidden');
                else el.classList.add('hidden');
            });
        });

        toggleBreakdownBtn.addEventListener('click', () => {
            showBreakdown = !showBreakdown;
            toggleBreakdownBtn.textContent = showBreakdown ? 'Hide Word Vocabulary Breakdown' : 'Show Word Vocabulary Breakdown';
            renderSentences();
            highlightSentence(currentSentenceIdx);
        });

        // Initial setup on window load
        window.onload = function() {
            populateVoiceList();
            renderSentences();
            highlightSentence(0);
        };
    </script>
</body>
</html>
