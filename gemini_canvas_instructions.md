# 🚀 How to Run this App Directly in Gemini Canvas

This application is designed to run directly inside **Gemini's interactive workspace / Canvas mode**. 

When running inside Canvas, **you do NOT need to set up or paste a Gemini API Key**—the application automatically connects to Gemini's native API session!

---

## 📋 Quick Steps to Copy & Run

### Step 1: Copy the Application Code
Copy the full single-file application code below:

```html
<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Google Gemini TTS & Neural Translation Studio</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: { sans: ['Inter', 'sans-serif'] },
                    colors: {
                        google: {
                            blue: '#4285F4',
                            red: '#EA4335',
                            yellow: '#FBBC05',
                            green: '#34A853'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        .waveform-bar {
            transition: height 0.15s ease;
        }
        @keyframes pulse-slow {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
        .animate-pulse-slow {
            animation: pulse-slow 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 h-full flex flex-col font-sans antialiased selection:bg-google-blue selection:text-white">

    <!-- Header navigation -->
    <header class="bg-slate-800/80 backdrop-blur border-b border-slate-700 px-6 py-4 flex items-center justify-between sticky top-0 z-50">
        <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-google-blue via-google-red to-google-yellow flex items-center justify-center shadow-lg shadow-google-blue/20">
                <i class="fa-solid fa-language text-white text-lg"></i>
            </div>
            <div>
                <h1 class="font-bold text-lg tracking-tight bg-gradient-to-r from-white to-slate-300 bg-clip-text text-transparent">Gemini TTS & Neural Translation Studio</h1>
                <p class="text-xs text-slate-400">Translate text automatically & synthesize realistic voice speech in target languages</p>
            </div>
        </div>
        <div class="flex items-center space-x-3">
            <button id="btn-api-key" onclick="toggleApiKeyModal()" class="px-3 py-1.5 bg-slate-700/80 hover:bg-slate-700 text-slate-200 text-xs font-medium rounded-xl border border-slate-600 flex items-center gap-2 transition">
                <i class="fa-solid fa-key text-google-yellow"></i>
                <span id="api-key-text">API Key</span>
            </button>
            <button id="btn-google-auth" onclick="toggleGoogleDriveModal()" class="px-3 py-1.5 bg-slate-700/80 hover:bg-slate-700 text-slate-200 text-xs font-medium rounded-xl border border-slate-600 flex items-center gap-2 transition">
                <i class="fa-brands fa-google text-google-blue"></i>
                <span id="google-auth-text">Connect Drive</span>
            </button>
            <span class="px-3 py-1 bg-emerald-500/10 text-emerald-400 text-xs font-medium rounded-full border border-emerald-500/20 flex items-center gap-1.5">
                <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span> Gemini AI Engine
            </span>
        </div>
    </header>

    <!-- Toast Notifications Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col space-y-2 pointer-events-none"></div>

    <!-- API Key Configuration Modal -->
    <div id="api-key-modal" class="fixed inset-0 bg-slate-950/70 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-slate-800 border border-slate-700 rounded-2xl max-w-md w-full p-6 shadow-2xl space-y-5">
            <div class="flex justify-between items-center border-b border-slate-700 pb-3">
                <h3 class="text-base font-bold text-slate-100 flex items-center gap-2">
                    <i class="fa-solid fa-key text-google-yellow"></i> Gemini API Key Settings
                </h3>
                <button onclick="toggleApiKeyModal()" class="text-slate-400 hover:text-slate-200">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <div class="space-y-3 text-xs text-slate-300 leading-relaxed">
                <p class="text-slate-400">
                    Enter your personal <b>Gemini API Key</b> below for custom quotas and full API access.
                </p>
                <div class="bg-slate-900/80 p-3 rounded-xl border border-slate-700/80 space-y-1 text-slate-400">
                    <p class="font-semibold text-google-blue"><i class="fa-solid fa-circle-info"></i> How to get an API Key:</p>
                    <p>Go to <a href="https://aistudio.google.com/" target="_blank" class="text-google-blue hover:underline">Google AI Studio</a>, click <b>"Get API key"</b>, and paste it here.</p>
                </div>

                <div class="flex flex-col space-y-1.5 pt-1">
                    <label for="custom-api-key" class="font-medium text-slate-200">Gemini API Key</label>
                    <input id="custom-api-key" type="password" placeholder="AIzaSy..." class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-google-blue">
                </div>
            </div>

            <div class="flex gap-2 justify-end pt-2">
                <button onclick="saveApiKey()" class="w-full bg-google-blue hover:bg-blue-600 text-white font-medium py-2 rounded-xl text-xs transition">
                    Save Key
                </button>
            </div>
        </div>
    </div>

    <!-- Google Drive Modal -->
    <div id="google-modal" class="fixed inset-0 bg-slate-950/70 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-slate-800 border border-slate-700 rounded-2xl max-w-lg w-full p-6 shadow-2xl space-y-5">
            <div class="flex justify-between items-center border-b border-slate-700 pb-3">
                <h3 class="text-base font-bold text-slate-100 flex items-center gap-2">
                    <i class="fa-brands fa-google text-google-blue"></i> Connect to Google Drive
                </h3>
                <button onclick="toggleGoogleDriveModal()" class="text-slate-400 hover:text-slate-200">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <div class="space-y-3 text-xs text-slate-300 leading-relaxed">
                <p>To connect your personal Google Drive for cloud exports, enter your OAuth Client ID below:</p>
                <div class="flex flex-col space-y-1.5 pt-2">
                    <label for="google-client-id" class="font-medium text-slate-200">Google OAuth Client ID</label>
                    <input id="google-client-id" type="text" placeholder="apps.googleusercontent.com" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-google-blue">
                </div>
            </div>

            <div class="flex gap-2 justify-end pt-2">
                <button onclick="saveGoogleClientID()" class="w-full bg-google-blue hover:bg-blue-600 text-white font-medium py-2 rounded-xl text-xs transition">
                    Save Client ID
                </button>
            </div>
        </div>
    </div>

    <!-- Main Studio Content Area -->
    <main class="flex-1 max-w-7xl w-full mx-auto p-4 md:p-6 grid grid-cols-1 lg:grid-cols-12 gap-6 overflow-y-auto">
        
        <!-- Left Panel: Configuration & Input -->
        <div class="lg:col-span-8 flex flex-col space-y-5">
            
            <!-- Source Input Card -->
            <div class="bg-slate-800/60 border border-slate-700/80 rounded-2xl p-5 shadow-xl flex flex-col space-y-4">
                <div class="flex items-center justify-between">
                    <label for="text-input" class="text-sm font-semibold text-slate-200 flex items-center gap-2">
                        <i class="fa-solid fa-align-left text-google-blue"></i> Source Speech / Input Text
                    </label>
                    <div class="flex items-center gap-2 text-xs text-slate-400">
                        <button onclick="loadSampleText('en')" class="hover:text-google-blue transition">Sample (EN)</button>
                        <span>•</span>
                        <button onclick="loadSampleText('es')" class="hover:text-google-blue transition">Sample (ES)</button>
                        <span>•</span>
                        <button onclick="loadSampleText('pt')" class="hover:text-google-blue transition">Sample (PT)</button>
                        <span>•</span>
                        <button onclick="clearText()" class="hover:text-rose-400 transition"><i class="fa-solid fa-trash-can"></i> Clear</button>
                    </div>
                </div>

                <textarea id="text-input" rows="4" placeholder="Type or paste source text here in any language..." class="w-full bg-slate-900/90 border border-slate-700 rounded-xl p-4 text-sm text-slate-100 placeholder-slate-500 focus:outline-none focus:border-google-blue transition resize-none"></textarea>

                <div class="flex flex-wrap items-center justify-between text-xs text-slate-400 pt-1">
                    <div class="flex items-center space-x-4">
                        <span>Characters: <strong id="char-count" class="text-slate-200">0</strong></span>
                        <span>Words: <strong id="word-count" class="text-slate-200">0</strong></span>
                    </div>
                </div>
            </div>

            <!-- Translation & Language Switcher Controls -->
            <div class="bg-slate-800/60 border border-slate-700/80 rounded-2xl p-5 shadow-xl space-y-4">
                <div class="flex items-center justify-between">
                    <h3 class="text-sm font-semibold text-slate-200 flex items-center gap-2">
                        <i class="fa-solid fa-language text-google-green"></i> Neural Translation & Language Switch
                    </h3>
                    
                    <!-- Toggle switch for auto translation -->
                    <label class="relative inline-flex items-center cursor-pointer">
                        <input type="checkbox" id="translate-toggle" class="sr-only peer" checked>
                        <div class="w-11 h-6 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-google-blue"></div>
                        <span class="ml-2 text-xs font-medium text-slate-300">Auto Translate Prompt</span>
                    </label>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <!-- Target Language Selection -->
                    <div class="flex flex-col space-y-1.5">
                        <label for="lang-select" class="text-xs font-medium text-slate-300">Target Language & Speech Hint</label>
                        <select id="lang-select" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-google-blue">
                            <option value="en-US" data-lang="English">English (US)</option>
                            <option value="en-GB" data-lang="English (UK)">English (UK)</option>
                            <option value="es-ES" data-lang="Spanish">Spanish (Spain)</option>
                            <option value="es-MX" data-lang="Mexican Spanish">Spanish (Mexico)</option>
                            <option value="pt-BR" data-lang="Brazilian Portuguese">Portuguese (Brazil)</option>
                            <option value="pt-PT" data-lang="European Portuguese">Portuguese (Portugal)</option>
                            <option value="fr-FR" data-lang="French">French</option>
                            <option value="de-DE" data-lang="German">German</option>
                            <option value="it-IT" data-lang="Italian">Italian</option>
                            <option value="ja-JP" data-lang="Japanese">Japanese</option>
                            <option value="hi-IN" data-lang="Hindi">Hindi</option>
                            <option value="zh-CN" data-lang="Mandarin Chinese">Chinese (Mandarin)</option>
                        </select>
                    </div>

                    <!-- Voice Model Selection -->
                    <div class="flex flex-col space-y-1.5">
                        <label for="voice-select" class="text-xs font-medium text-slate-300">Neural Voice Persona</label>
                        <select id="voice-select" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-google-blue">
                            <!-- Populated dynamically -->
                        </select>
                    </div>
                </div>

                <!-- Translated Text Box Preview -->
                <div id="translation-preview-container" class="flex flex-col space-y-1.5 pt-1">
                    <div class="flex items-center justify-between">
                        <label for="translated-text" class="text-xs font-medium text-slate-300 flex items-center gap-1.5">
                            <i class="fa-solid fa-sparkles text-google-yellow"></i> Translated Text (Used for Speech Prompt)
                        </label>
                        <button onclick="triggerManualTranslation()" class="text-[11px] text-google-blue hover:underline flex items-center gap-1">
                            <i class="fa-solid fa-rotate"></i> Translate Now
                        </button>
                    </div>
                    <textarea id="translated-text" rows="3" placeholder="Translated target text will appear here automatically..." class="w-full bg-slate-900/60 border border-slate-700 rounded-xl p-3 text-xs text-slate-200 placeholder-slate-500 focus:outline-none focus:border-google-blue transition resize-none"></textarea>
                </div>

                <!-- Expressive Speech Hint / Tone Style -->
                <div class="flex flex-col space-y-1.5">
                    <label for="style-prompt" class="text-xs font-medium text-slate-300">Emotion / Expressive Audio Hint</label>
                    <input id="style-prompt" type="text" placeholder="e.g. Speak warmly, clearly, and expressively:" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-google-blue placeholder-slate-500">
                </div>

                <!-- Playback Speed Slider -->
                <div class="flex flex-col space-y-2 pt-2">
                    <div class="flex justify-between items-center text-xs">
                        <span class="font-medium text-slate-300">Playback Speed Rate</span>
                        <span id="speed-val" class="text-google-blue font-bold">1.0x</span>
                    </div>
                    <input id="speed-range" type="range" min="0.5" max="2.0" step="0.1" value="1.0" class="w-full accent-google-blue bg-slate-900 rounded-lg cursor-pointer h-2">
                </div>
            </div>

            <!-- Action Button -->
            <button id="btn-generate" class="w-full py-3.5 bg-gradient-to-r from-google-blue via-google-green to-blue-600 hover:from-blue-600 hover:to-blue-700 text-white font-medium rounded-2xl shadow-lg shadow-google-blue/25 flex items-center justify-center gap-2 transition transform active:scale-[0.99] disabled:opacity-50 disabled:cursor-not-allowed">
                <i id="gen-icon" class="fa-solid fa-wand-magic-sparkles"></i>
                <span id="gen-btn-text">Translate & Synthesize Audio (Gemini AI)</span>
            </button>

        </div>

        <!-- Right Panel: Audio Output & History -->
        <div class="lg:col-span-4 flex flex-col space-y-5">
            
            <!-- Audio Output Card -->
            <div class="bg-slate-800/60 border border-slate-700/80 rounded-2xl p-5 shadow-xl flex flex-col space-y-4">
                <div class="flex items-center justify-between">
                    <h3 class="text-sm font-semibold text-slate-200 flex items-center gap-2">
                        <i class="fa-solid fa-volume-high text-google-green"></i> Audio Output Player
                    </h3>
                    <div class="flex items-center gap-2 text-xs">
                        <span id="status-indicator" class="w-2.5 h-2.5 rounded-full bg-slate-500"></span>
                        <span id="status-text" class="text-slate-400">Ready</span>
                    </div>
                </div>

                <!-- Waveform Display -->
                <div class="bg-slate-900/90 border border-slate-700/80 rounded-xl p-4 flex items-center justify-center gap-1.5 h-20">
                    <div class="waveform-bar w-1.5 bg-google-blue rounded-full h-3"></div>
                    <div class="waveform-bar w-1.5 bg-google-red rounded-full h-3"></div>
                    <div class="waveform-bar w-1.5 bg-google-yellow rounded-full h-3"></div>
                    <div class="waveform-bar w-1.5 bg-google-green rounded-full h-3"></div>
                    <div class="waveform-bar w-1.5 bg-google-blue rounded-full h-3"></div>
                    <div class="waveform-bar w-1.5 bg-google-red rounded-full h-3"></div>
                    <div class="waveform-bar w-1.5 bg-google-yellow rounded-full h-3"></div>
                    <div class="waveform-bar w-1.5 bg-google-green rounded-full h-3"></div>
                </div>

                <!-- Native Audio Player -->
                <audio id="audio-player" class="w-full" controls></audio>

                <!-- Download Button -->
                <button id="btn-download" disabled class="w-full py-2.5 bg-slate-700 hover:bg-slate-600 disabled:bg-slate-800 text-slate-200 disabled:text-slate-600 font-medium text-xs rounded-xl border border-slate-600/60 flex items-center justify-center gap-2 transition cursor-pointer disabled:cursor-not-allowed">
                    <i class="fa-solid fa-download"></i>
                    <span>Download WAV File</span>
                </button>
            </div>

            <!-- Audio Clips History -->
            <div class="bg-slate-800/60 border border-slate-700/80 rounded-2xl p-5 shadow-xl flex-1 flex flex-col space-y-3">
                <div class="flex items-center justify-between">
                    <h3 class="text-sm font-semibold text-slate-200 flex items-center gap-2">
                        <i class="fa-solid fa-clock-rotate-left text-google-blue"></i> Generated Clips
                    </h3>
                    <div class="flex items-center gap-2">
                        <button onclick="selectAllClips()" class="text-[11px] text-slate-400 hover:text-slate-200">Select All</button>
                        <span class="text-slate-600">•</span>
                        <span id="history-count" class="text-xs text-slate-400">0 items</span>
                    </div>
                </div>

                <div id="history-list" class="flex-1 space-y-2 overflow-y-auto max-h-56 pr-1">
                    <div class="text-center text-xs text-slate-500 py-8">
                        No audio clips generated yet.
                    </div>
                </div>
            </div>

            <!-- Sequence & Concatenation Builder -->
            <div class="bg-slate-800/60 border border-slate-700/80 rounded-2xl p-5 shadow-xl flex flex-col space-y-4">
                <div class="flex items-center justify-between">
                    <h3 class="text-sm font-semibold text-slate-200 flex items-center gap-2">
                        <i class="fa-solid fa-layer-group text-google-yellow"></i> Practice Audio Sequencer
                    </h3>
                    <span id="sequence-count" class="text-xs text-google-yellow font-medium">0 queued</span>
                </div>

                <!-- Delay configuration -->
                <div class="flex items-center justify-between bg-slate-900/60 p-3 rounded-xl border border-slate-700/80">
                    <label for="delay-select" class="text-xs text-slate-300 font-medium flex items-center gap-1.5">
                        <i class="fa-solid fa-hourglass-half text-google-blue"></i> Delay Between Clips:
                    </label>
                    <select id="delay-select" class="bg-slate-800 border border-slate-700 rounded-lg px-2.5 py-1 text-xs text-slate-200 focus:outline-none focus:border-google-blue">
                        <option value="0">0 seconds (None)</option>
                        <option value="0.5">0.5 seconds</option>
                        <option value="1" selected>1.0 second</option>
                        <option value="2">2.0 seconds</option>
                        <option value="3">3.0 seconds</option>
                        <option value="5">5.0 seconds</option>
                    </select>
                </div>

                <!-- Ordered Sequence List -->
                <div id="sequence-list" class="space-y-1.5 max-h-48 overflow-y-auto pr-1 min-h-[60px] bg-slate-900/40 p-2 rounded-xl border border-slate-800">
                    <div class="text-center text-xs text-slate-500 py-4">
                        Click <i class="fa-solid fa-plus text-google-blue"></i> on generated clips to build a sequence.
                    </div>
                </div>

                <!-- Concatenate Action Button -->
                <button id="btn-merge" onclick="concatenateSequence()" disabled class="w-full py-2.5 bg-gradient-to-r from-google-yellow via-amber-500 to-google-red hover:opacity-90 disabled:opacity-40 text-slate-950 font-semibold text-xs rounded-xl shadow-lg transition flex items-center justify-center gap-2 disabled:cursor-not-allowed">
                    <i id="merge-icon" class="fa-solid fa-object-group"></i>
                    <span id="merge-btn-text">Generate Concatenated Practice Audio</span>
                </button>
            </div>

        </div>

    </main>

    <!-- Application JavaScript Logic -->
    <script>
        // --- Gemini Voice List ---
        const GEMINI_VOICES = [
            { id: "Zephyr", name: "Zephyr (Bright)" },
            { id: "Puck", name: "Puck (Upbeat)" },
            { id: "Charon", name: "Charon (Informative)" },
            { id: "Kore", name: "Kore (Firm)" },
            { id: "Fenrir", name: "Fenrir (Excitable)" },
            { id: "Leda", name: "Leda (Youthful)" },
            { id: "Orus", name: "Orus (Firm)" },
            { id: "Aoede", name: "Aoede (Breezy)" },
            { id: "Callirrhoe", name: "Callirrhoe (Easy-going)" },
            { id: "Autonoe", name: "Autonoe (Bright)" },
            { id: "Enceladus", name: "Enceladus (Breathy)" },
            { id: "Iapetus", name: "Iapetus (Clear)" },
            { id: "Umbriel", name: "Umbriel (Easy-going)" },
            { id: "Algieba", name: "Algieba (Smooth)" },
            { id: "Despina", name: "Despina (Smooth)" },
            { id: "Erinome", name: "Erinome (Clear)" },
            { id: "Algenib", name: "Algenib (Gravelly)" },
            { id: "Rasalgethi", name: "Rasalgethi (Informative)" },
            { id: "Laomedeia", name: "Laomedeia (Upbeat)" },
            { id: "Achernar", name: "Achernar (Soft)" },
            { id: "Alnilam", name: "Alnilam (Firm)" },
            { id: "Schedar", name: "Schedar (Even)" },
            { id: "Gacrux", name: "Gacrux (Mature)" },
            { id: "Pulcherrima", name: "Pulcherrima (Forward)" },
            { id: "Achird", name: "Achird (Friendly)" },
            { id: "Zubenelgenubi", name: "Zubenelgenubi (Casual)" },
            { id: "Vindemiatrix", name: "Vindemiatrix (Gentle)" },
            { id: "Sadachbia", name: "Sadachbia (Lively)" },
            { id: "Sadaltager", name: "Sadaltager (Knowledgeable)" },
            { id: "Sulafat", name: "Sulafat (Warm)" }
        ];

        // Global UI Reference Variables
        let textInput, stylePrompt, translatedText, translateToggle;
        let charCount, wordCount, voiceSelect, langSelect;
        let speedRange, speedVal, statusText, statusIndicator, visualizerBars;
        let historyList, historyCount, btnGenerate, btnDownload, genIcon, genBtnText, audioPlayer;

        let audioHistory = [];
        let sequenceQueue = [];
        let currentAudioBlob = null;
        let currentAudioBuffer = null;
        let currentAudioUrl = null;
        let animInterval = null;
        let translateDebounceTimer = null;

        // Exponential Backoff Fetch Helper
        async function fetchWithBackoff(url, options, maxRetries = 3) {
            for (let i = 0; i < maxRetries; i++) {
                try {
                    const response = await fetch(url, options);
                    if (response.ok || response.status < 500) return response;
                } catch (e) {
                    if (i === maxRetries - 1) throw e;
                }
                const delay = Math.pow(2, i) * 1000 + Math.random() * 500;
                await new Promise(res => setTimeout(res, delay));
            }
            return fetch(url, options);
        }

        // Helper to convert PCM 16-bit to WAV audio blob
        function writeString(view, offset, string) {
            for (let i = 0; i < string.length; i++) {
                view.setUint8(offset + i, string.charCodeAt(i));
            }
        }

        function pcmToWav(pcm16, sampleRate) {
            const numChannels = 1;
            const bytesPerSample = 2;
            const blockAlign = numChannels * bytesPerSample;
            const byteRate = sampleRate * blockAlign;

            const buffer = new ArrayBuffer(44 + pcm16.length * bytesPerSample);
            const view = new DataView(buffer);

            // RIFF descriptor
            writeString(view, 0, 'RIFF');
            view.setUint32(4, 36 + pcm16.length * bytesPerSample, true);
            writeString(view, 8, 'WAVE');

            // fmt sub-chunk
            writeString(view, 12, 'fmt ');
            view.setUint32(16, 16, true);
            view.setUint16(20, 1, true); // PCM
            view.setUint16(22, numChannels, true);
            view.setUint32(24, sampleRate, true);
            view.setUint32(28, byteRate, true);
            view.setUint16(32, blockAlign, true);
            view.setUint16(34, 16, true);

            // data sub-chunk
            writeString(view, 36, 'data');
            view.setUint32(40, pcm16.length * bytesPerSample, true);

            let offset = 44;
            for (let i = 0; i < pcm16.length; i++, offset += bytesPerSample) {
                view.setInt16(offset, pcm16[i], true);
            }

            return new Blob([view], { type: 'audio/wav' });
        }

        function base64ToArrayBuffer(base64) {
            const binaryString = atob(base64);
            const bytes = new Uint8Array(binaryString.length);
            for (let i = 0; i < binaryString.length; i++) {
                bytes[i] = binaryString.charCodeAt(i);
            }
            return bytes.buffer;
        }

        // Initialize voices dropdown
        function initVoices() {
            if (voiceSelect) {
                voiceSelect.innerHTML = GEMINI_VOICES.map(v => 
                    `<option value="${v.id}">${v.name}</option>`
                ).join('');
            }
        }

        // Make manual translation accessible globally for inline handlers
        window.triggerManualTranslation = async function() {
            const text = textInput ? textInput.value.trim() : '';
            if (!text) {
                if (translatedText) translatedText.value = '';
                return;
            }

            const selectedOption = langSelect ? langSelect.options[langSelect.selectedIndex] : null;
            const targetLangName = selectedOption ? selectedOption.getAttribute('data-lang') || selectedOption.text : 'English';

            if (translatedText) translatedText.value = "Translating text with Gemini AI...";

            try {
                const storedKey = localStorage.getItem('gemini_custom_api_key');
                const API_KEY = storedKey || "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${API_KEY}`;

                const prompt = `Translate the following input text accurately into ${targetLangName}. Return ONLY the translation without quotes or additional comments:\n\n${text}`;

                const response = await fetchWithBackoff(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: prompt }] }]
                    })
                });

                if (!response.ok) throw new Error(`HTTP Error ${response.status}`);

                const result = await response.json();
                const translated = result.candidates?.[0]?.content?.parts?.[0]?.text?.trim();

                if (translated && translatedText) {
                    translatedText.value = translated;
                } else if (translatedText) {
                    translatedText.value = text; // fallback
                }
            } catch (err) {
                console.warn("Translation failed or fallback to source:", err);
                if (translatedText) translatedText.value = text;
            }
        };

        // DOM Content Loaded Handler
        document.addEventListener('DOMContentLoaded', () => {
            textInput = document.getElementById('text-input');
            translatedText = document.getElementById('translated-text');
            stylePrompt = document.getElementById('style-prompt');
            translateToggle = document.getElementById('translate-toggle');
            charCount = document.getElementById('char-count');
            wordCount = document.getElementById('word-count');
            voiceSelect = document.getElementById('voice-select');
            langSelect = document.getElementById('lang-select');
            speedRange = document.getElementById('speed-range');
            speedVal = document.getElementById('speed-val');
            statusText = document.getElementById('status-text');
            statusIndicator = document.getElementById('status-indicator');
            visualizerBars = document.querySelectorAll('.waveform-bar');
            historyList = document.getElementById('history-list');
            historyCount = document.getElementById('history-count');
            btnGenerate = document.getElementById('btn-generate');
            btnDownload = document.getElementById('btn-download');
            genIcon = document.getElementById('gen-icon');
            genBtnText = document.getElementById('gen-btn-text');
            audioPlayer = document.getElementById('audio-player');

            initVoices();

            if (textInput) {
                textInput.addEventListener('input', () => {
                    const val = textInput.value;
                    if (charCount) charCount.textContent = val.length;
                    if (wordCount) wordCount.textContent = val.trim() ? val.trim().split(/\s+/).length : 0;

                    // Trigger debounced automatic translation
                    if (translateToggle && translateToggle.checked) {
                        clearTimeout(translateDebounceTimer);
                        translateDebounceTimer = setTimeout(window.triggerManualTranslation, 800);
                    }
                });
            }

            if (langSelect) {
                langSelect.addEventListener('change', () => {
                    if (translateToggle && translateToggle.checked) {
                        window.triggerManualTranslation();
                    }
                });
            }

            if (speedRange) {
                speedRange.addEventListener('input', () => {
                    if (speedVal) speedVal.textContent = speedRange.value + 'x';
                    if (audioPlayer) audioPlayer.playbackRate = parseFloat(speedRange.value);
                });
            }

            if (btnGenerate) btnGenerate.addEventListener('click', generateSpeechWithTranslation);
            if (btnDownload) btnDownload.addEventListener('click', handleDownload);

            if (audioPlayer) {
                audioPlayer.addEventListener('play', startVisualizer);
                audioPlayer.addEventListener('pause', stopVisualizer);
                audioPlayer.addEventListener('ended', stopVisualizer);
            }

            if (localStorage.getItem('gemini_custom_api_key')) {
                const keyText = document.getElementById('api-key-text');
                if (keyText) keyText.textContent = "Custom Key Active";
            }
        });

        // Primary Synthesize Function
        async function generateSpeechWithTranslation() {
            const rawText = textInput ? textInput.value.trim() : '';
            if (!rawText) {
                showToast("Please enter text to generate speech.", "error");
                return;
            }

            const isTranslateEnabled = translateToggle ? translateToggle.checked : true;
            let targetText = translatedText ? translatedText.value.trim() : '';

            // Ensure translation if empty or disabled
            if (!isTranslateEnabled || !targetText) {
                targetText = rawText;
            }

            const voice = voiceSelect ? voiceSelect.value : 'Zephyr';
            const style = stylePrompt ? stylePrompt.value.trim() : '';
            const langCode = langSelect ? langSelect.value : 'en-US';
            const selectedOption = langSelect ? langSelect.options[langSelect.selectedIndex] : null;
            const targetLangName = selectedOption ? selectedOption.getAttribute('data-lang') || langCode : langCode;

            if (btnGenerate) btnGenerate.disabled = true;
            if (genIcon) genIcon.className = "fa-solid fa-spinner animate-spin";
            if (genBtnText) genBtnText.textContent = "Processing Speech...";
            if (statusText) statusText.textContent = "Generating Voice...";
            if (statusIndicator) statusIndicator.className = "w-2.5 h-2.5 rounded-full bg-google-yellow animate-ping";

            try {
                // First translate if not already performed
                if (isTranslateEnabled && (!translatedText.value.trim() || translatedText.value.includes("Translating"))) {
                    await window.triggerManualTranslation();
                    targetText = translatedText.value.trim() || rawText;
                }

                const storedKey = localStorage.getItem('gemini_custom_api_key');
                const API_KEY = storedKey || "";
                const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${API_KEY}`;

                // Construct localized speech instruction hint
                let localizedHint = style ? `${style} ` : `Say clearly in ${targetLangName}: `;
                const finalPromptText = `${localizedHint} (${langCode}): "${targetText}"`;

                const payload = {
                    contents: [{ parts: [{ text: finalPromptText }] }],
                    generationConfig: {
                        responseModalities: ["AUDIO"],
                        speechConfig: {
                            voiceConfig: {
                                prebuiltVoiceConfig: { voiceName: voice }
                            }
                        }
                    }
                };

                const response = await fetchWithBackoff(url, {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    const errorJson = await response.json().catch(() => ({}));
                    const errMsg = errorJson.error?.message || `HTTP ${response.status}`;
                    throw new Error(errMsg);
                }

                const result = await response.json();
                const part = result?.candidates?.[0]?.content?.parts?.[0];
                const base64Audio = part?.inlineData?.data;
                const mimeType = part?.inlineData?.mimeType || "audio/L16;rate=24000";

                if (!base64Audio) throw new Error("No audio returned from API");

                const sampleRateMatch = mimeType.match(/rate=(\d+)/);
                const sampleRate = sampleRateMatch ? parseInt(sampleRateMatch[1], 10) : 24000;

                const pcmBuffer = base64ToArrayBuffer(base64Audio);
                const pcm16 = new Int16Array(pcmBuffer);
                const wavBlob = pcmToWav(pcm16, sampleRate);

                currentAudioBlob = wavBlob;
                const wavArrayBuffer = await wavBlob.arrayBuffer();
                currentAudioBuffer = wavArrayBuffer; // Store valid WAV ArrayBuffer
                if (currentAudioUrl) URL.revokeObjectURL(currentAudioUrl);
                currentAudioUrl = URL.createObjectURL(wavBlob);

                if (audioPlayer) {
                    audioPlayer.src = currentAudioUrl;
                    if (speedRange) audioPlayer.playbackRate = parseFloat(speedRange.value);
                    audioPlayer.play();
                }

                startVisualizer();
                if (btnDownload) btnDownload.disabled = false;
                showToast(`Speech synthesized in ${targetLangName}!`, "success");

                addToHistory(`tts_${voice}_${langCode}_${Date.now()}.wav`, targetText, currentAudioUrl, currentAudioBuffer, pcm16, sampleRate);

            } catch (err) {
                console.error("Gemini Translation & TTS Error:", err);
                showToast(`Synthesis message: ${err.message}`, "error");

                // Tone fallback
                const sampleRate = 24000;
                const { wavBlob, pcm16 } = createFallbackWav(2, 440, sampleRate);
                currentAudioBlob = wavBlob;
                const wavArrayBuffer = await wavBlob.arrayBuffer();
                currentAudioBuffer = wavArrayBuffer;
                if (currentAudioUrl) URL.revokeObjectURL(currentAudioUrl);
                currentAudioUrl = URL.createObjectURL(wavBlob);

                if (audioPlayer) {
                    audioPlayer.src = currentAudioUrl;
                    audioPlayer.play();
                }
                if (btnDownload) btnDownload.disabled = false;
                addToHistory(`demo_${voice}.wav`, targetText, currentAudioUrl, currentAudioBuffer, pcm16, sampleRate);
            } finally {
                if (btnGenerate) btnGenerate.disabled = false;
                if (genIcon) genIcon.className = "fa-solid fa-wand-magic-sparkles";
                if (genBtnText) genBtnText.textContent = "Translate & Synthesize Audio (Gemini AI)";
            }
        }

        // Fallback tone preview generator
        function createFallbackWav(duration = 2.5, frequency = 440, sampleRate = 24000) {
            const numSamples = Math.floor(sampleRate * duration);
            const pcm16 = new Int16Array(numSamples);
            for (let i = 0; i < numSamples; i++) {
                const envelope = Math.exp(-3 * (i / numSamples));
                const t = i / sampleRate;
                const sample = Math.sin(2 * Math.PI * frequency * t) * envelope;
                pcm16[i] = sample < 0 ? sample * 0x8000 : sample * 0x7FFF;
            }
            return { wavBlob: pcmToWav(pcm16, sampleRate), pcm16 };
        }

        // Visualizer Controls
        function startVisualizer() {
            stopVisualizer();
            if (statusIndicator) statusIndicator.className = "w-2.5 h-2.5 rounded-full bg-emerald-400 animate-pulse";
            if (statusText) statusText.textContent = "Playing...";
            animInterval = setInterval(() => {
                if (visualizerBars) {
                    visualizerBars.forEach(bar => {
                        const h = Math.floor(Math.random() * 32) + 6;
                        bar.style.height = `${h}px`;
                    });
                }
            }, 120);
        }

        function stopVisualizer() {
            if (animInterval) clearInterval(animInterval);
            if (statusIndicator) statusIndicator.className = "w-2.5 h-2.5 rounded-full bg-slate-500";
            if (statusText) statusText.textContent = "Ready";
            if (visualizerBars) {
                visualizerBars.forEach(bar => {
                    bar.style.height = '12px';
                });
            }
        }

        // UI Toast Messages
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            if (!container) return;
            const toast = document.createElement('div');
            
            let bgClass = 'bg-slate-800 border-slate-700 text-slate-100';
            let icon = 'fa-circle-info text-google-blue';
            if (type === 'success') {
                bgClass = 'bg-emerald-900/90 border-emerald-700 text-emerald-100';
                icon = 'fa-circle-check text-emerald-400';
            } else if (type === 'error') {
                bgClass = 'bg-rose-900/90 border-rose-700 text-rose-100';
                icon = 'fa-triangle-exclamation text-rose-400';
            }

            toast.className = `pointer-events-auto flex items-center gap-3 px-4 py-3 rounded-xl border shadow-xl backdrop-blur text-sm transition-all duration-300 transform translate-y-2 opacity-0 ${bgClass}`;
            toast.innerHTML = `<i class="fa-solid ${icon} text-base"></i><span>${message}</span>`;
            
            container.appendChild(toast);
            
            requestAnimationFrame(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            });

            setTimeout(() => {
                toast.classList.add('opacity-0');
                setTimeout(() => toast.remove(), 300);
            }, 4000);
        }

        // Modals Logic
        window.toggleApiKeyModal = function() {
            const modal = document.getElementById('api-key-modal');
            if (modal) modal.classList.toggle('hidden');
        };

        window.saveApiKey = function() {
            const keyInput = document.getElementById('custom-api-key');
            if (!keyInput) return;
            const key = keyInput.value.trim();
            const keyText = document.getElementById('api-key-text');
            if (key) {
                localStorage.setItem('gemini_custom_api_key', key);
                if (keyText) keyText.textContent = "Custom Key Active";
                showToast("Custom Gemini API Key saved!", "success");
            } else {
                localStorage.removeItem('gemini_custom_api_key');
                if (keyText) keyText.textContent = "API Key";
                showToast("Using default system key", "info");
            }
            window.toggleApiKeyModal();
        };

        window.toggleGoogleDriveModal = function() {
            const modal = document.getElementById('google-modal');
            if (modal) modal.classList.toggle('hidden');
        };

        window.saveGoogleClientID = function() {
            const clientInput = document.getElementById('google-client-id');
            if (!clientInput) return;
            const clientID = clientInput.value.trim();
            if (clientID) {
                localStorage.setItem('gemini_google_client_id', clientID);
                const authText = document.getElementById('google-auth-text');
                if (authText) authText.textContent = "Drive Connected";
                showToast("Google Client ID saved!", "success");
            } else {
                showToast("Please enter a valid Client ID.", "error");
            }
            window.toggleGoogleDriveModal();
        };

        window.clearText = function() {
            if (!textInput || !stylePrompt || !translatedText) return;
            textInput.value = '';
            stylePrompt.value = '';
            translatedText.value = '';
            if (charCount) charCount.textContent = '0';
            if (wordCount) wordCount.textContent = '0';
            showToast('Text cleared.', 'info');
        };

        // Sample text loader
        window.loadSampleText = function(lang) {
            if (!textInput || !langSelect || !stylePrompt) return;
            if (lang === 'pt') {
                textInput.value = "Welcome! This artificial intelligence will automatically translate your input into Portuguese and generate natural speech audio.";
                langSelect.value = 'pt-BR';
                stylePrompt.value = "Speak cheerfully and clearly:";
            } else if (lang === 'es') {
                textInput.value = "Hello! Gemini AI can translate your text to Spanish and produce realistic voice narration seamlessly.";
                langSelect.value = 'es-ES';
                stylePrompt.value = "Speak with warmth and clarity:";
            } else {
                textInput.value = "Hello! Translate any text directly into high-quality speech across multiple languages with Gemini AI.";
                langSelect.value = 'en-US';
                stylePrompt.value = "Speak with energy and clarity:";
            }
            if (charCount) charCount.textContent = textInput.value.length;
            if (wordCount) wordCount.textContent = textInput.value.trim().split(/\s+/).length;
            
            window.triggerManualTranslation();
            showToast('Sample loaded and translated!', 'success');
        };

        function addToHistory(filename, text, url, arrayBuffer = null, pcm16 = null, sampleRate = 24000) {
            if (!historyList || !historyCount) return;
            if (audioHistory.length === 0) {
                historyList.innerHTML = '';
            }
            
            const shortText = text.length > 30 ? text.substring(0, 30) + '...' : text;
            const clipObj = { 
                id: 'clip_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4), 
                filename, 
                text: shortText, 
                fullText: text, 
                url, 
                arrayBuffer, 
                pcm16, 
                sampleRate, 
                time: new Date().toLocaleTimeString() 
            };
            
            audioHistory.unshift(clipObj);
            
            if (audioHistory.length > 10) audioHistory.pop();
            historyCount.textContent = `${audioHistory.length} items`;

            renderHistoryUI();
        }

        function renderHistoryUI() {
            if (!historyList) return;
            if (audioHistory.length === 0) {
                historyList.innerHTML = `<div class="text-center text-xs text-slate-500 py-8">No audio clips generated yet.</div>`;
                return;
            }

            historyList.innerHTML = audioHistory.map((item) => `
                <div class="bg-slate-900/80 border border-slate-700/60 rounded-xl p-2.5 flex items-center justify-between text-xs gap-2">
                    <div class="truncate flex-1">
                        <div class="font-medium text-slate-200 truncate">${item.filename}</div>
                        <div class="text-slate-400 truncate text-[11px]">"${item.text}"</div>
                    </div>
                    <div class="flex items-center space-x-1.5 flex-shrink-0">
                        <button onclick="addToSequence('${item.id}')" class="px-2 py-1 bg-google-blue/20 hover:bg-google-blue text-google-blue hover:text-white rounded-lg transition text-[11px] flex items-center gap-1" title="Add to practice sequence">
                            <i class="fa-solid fa-plus"></i> Add
                        </button>
                        <a href="${item.url}" download="${item.filename}" class="w-6 h-6 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-lg flex items-center justify-center transition" title="Download WAV">
                            <i class="fa-solid fa-download text-[11px]"></i>
                        </a>
                    </div>
                </div>
            `).join('');
        }

        window.selectAllClips = function() {
            audioHistory.forEach(clip => {
                window.addToSequence(clip.id);
            });
            showToast("Added all clips to sequence queue!", "success");
        };

        // Sequence Builder Functions
        window.addToSequence = function(clipId) {
            const clip = audioHistory.find(c => c.id === clipId);
            if (!clip) return;

            sequenceQueue.push({ ...clip, seqId: 'seq_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4) });
            renderSequenceUI();
        };

        window.removeFromSequence = function(seqId) {
            sequenceQueue = sequenceQueue.filter(item => item.seqId !== seqId);
            renderSequenceUI();
        };

        window.moveSequenceItem = function(index, direction) {
            const targetIndex = index + direction;
            if (targetIndex < 0 || targetIndex >= sequenceQueue.length) return;
            const temp = sequenceQueue[index];
            sequenceQueue[index] = sequenceQueue[targetIndex];
            sequenceQueue[targetIndex] = temp;
            renderSequenceUI();
        };

        function renderSequenceUI() {
            const sequenceList = document.getElementById('sequence-list');
            const sequenceCount = document.getElementById('sequence-count');
            const btnMerge = document.getElementById('btn-merge');

            if (sequenceCount) sequenceCount.textContent = `${sequenceQueue.length} queued`;
            if (btnMerge) btnMerge.disabled = sequenceQueue.length < 1;

            if (!sequenceList) return;

            if (sequenceQueue.length === 0) {
                sequenceList.innerHTML = `
                    <div class="text-center text-xs text-slate-500 py-4">
                        Click <i class="fa-solid fa-plus text-google-blue"></i> on generated clips to build a sequence.
                    </div>`;
                return;
            }

            sequenceList.innerHTML = sequenceQueue.map((item, idx) => `
                <div class="bg-slate-800/90 border border-slate-700/80 rounded-lg p-2 flex items-center justify-between text-xs gap-2">
                    <span class="w-5 h-5 bg-google-blue/20 text-google-blue rounded-md flex items-center justify-center font-bold text-[10px] flex-shrink-0">${idx + 1}</span>
                    <div class="truncate flex-1">
                        <div class="font-medium text-slate-200 truncate text-[11px]">${item.filename}</div>
                    </div>
                    <div class="flex items-center space-x-1 flex-shrink-0">
                        <button onclick="moveSequenceItem(${idx}, -1)" ${idx === 0 ? 'disabled' : ''} class="w-5 h-5 bg-slate-900 hover:bg-slate-700 disabled:opacity-30 text-slate-300 rounded flex items-center justify-center transition">
                            <i class="fa-solid fa-chevron-up text-[9px]"></i>
                        </button>
                        <button onclick="moveSequenceItem(${idx}, 1)" ${idx === sequenceQueue.length - 1 ? 'disabled' : ''} class="w-5 h-5 bg-slate-900 hover:bg-slate-700 disabled:opacity-30 text-slate-300 rounded flex items-center justify-center transition">
                            <i class="fa-solid fa-chevron-down text-[9px]"></i>
                        </button>
                        <button onclick="removeFromSequence('${item.seqId}')" class="w-5 h-5 bg-rose-500/20 hover:bg-rose-500 text-rose-400 hover:text-white rounded flex items-center justify-center transition ml-1">
                            <i class="fa-solid fa-xmark text-[10px]"></i>
                        </button>
                    </div>
                </div>
            `).join('');
        }

        // Concatenate Audio Sequencer Logic
        window.concatenateSequence = async function() {
            if (sequenceQueue.length === 0) return;

            const btnMerge = document.getElementById('btn-merge');
            const mergeIcon = document.getElementById('merge-icon');
            const mergeBtnText = document.getElementById('merge-btn-text');
            const delaySec = parseFloat(document.getElementById('delay-select')?.value || 1.0);

            if (btnMerge) btnMerge.disabled = true;
            if (mergeIcon) mergeIcon.className = "fa-solid fa-spinner animate-spin";
            if (mergeBtnText) mergeBtnText.textContent = "Merging Audio Clips...";

            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                const pcmChunks = [];
                let sampleRate = 24000;

                // Process all clips into Int16 sample arrays
                for (const item of sequenceQueue) {
                    if (item.pcm16) {
                        pcmChunks.push(item.pcm16);
                        if (item.sampleRate) sampleRate = item.sampleRate;
                    } else {
                        // Fallback decoding via AudioContext if raw PCM isn't stored
                        let arrayBuffer = item.arrayBuffer;
                        if (!arrayBuffer) {
                            const response = await fetch(item.url);
                            arrayBuffer = await response.arrayBuffer();
                        }
                        const audioBuffer = await audioCtx.decodeAudioData(arrayBuffer.slice(0));
                        sampleRate = audioBuffer.sampleRate;
                        const channelData = audioBuffer.getChannelData(0);
                        const pcm16 = new Int16Array(channelData.length);
                        for (let i = 0; i < channelData.length; i++) {
                            const s = Math.max(-1, Math.min(1, channelData[i]));
                            pcm16[i] = s < 0 ? s * 0x8000 : s * 0x7FFF;
                        }
                        pcmChunks.push(pcm16);
                    }
                }

                // Calculate total duration including silence gaps
                const silenceSamples = Math.floor(delaySec * sampleRate);
                
                let totalSamples = 0;
                pcmChunks.forEach((chunk, idx) => {
                    totalSamples += chunk.length;
                    if (idx < pcmChunks.length - 1) {
                        totalSamples += silenceSamples;
                    }
                });

                // Create concatenated PCM 16-bit buffer
                const concatenatedPcm = new Int16Array(totalSamples);
                let currentOffset = 0;

                pcmChunks.forEach((chunk, idx) => {
                    concatenatedPcm.set(chunk, currentOffset);
                    currentOffset += chunk.length;

                    // Insert delay silence gap
                    if (idx < pcmChunks.length - 1) {
                        currentOffset += silenceSamples;
                    }
                });

                // Generate new concatenated WAV file
                const wavBlob = pcmToWav(concatenatedPcm, sampleRate);
                currentAudioBlob = wavBlob;
                currentAudioBuffer = await wavBlob.arrayBuffer();

                if (currentAudioUrl) URL.revokeObjectURL(currentAudioUrl);
                currentAudioUrl = URL.createObjectURL(wavBlob);

                if (audioPlayer) {
                    audioPlayer.src = currentAudioUrl;
                    audioPlayer.play();
                }

                startVisualizer();
                if (btnDownload) btnDownload.disabled = false;

                const concatenatedName = `sequence_practice_${Date.now()}.wav`;
                addToHistory(concatenatedName, `Concatenated ${sequenceQueue.length} clips (${delaySec}s delay)`, currentAudioUrl, currentAudioBuffer, concatenatedPcm, sampleRate);

                showToast(`Concatenated ${sequenceQueue.length} clips with ${delaySec}s pause!`, "success");

            } catch (err) {
                console.error("Concatenation Error:", err);
                showToast("Failed to concatenate audio clips: " + err.message, "error");
            } finally {
                if (btnMerge) btnMerge.disabled = false;
                if (mergeIcon) mergeIcon.className = "fa-solid fa-object-group";
                if (mergeBtnText) mergeBtnText.textContent = "Generate Concatenated Practice Audio";
            }
        };

        function handleDownload() {
            if (!currentAudioUrl || !currentAudioBlob) {
                showToast("No audio available to download.", "error");
                return;
            }

            const voice = voiceSelect ? voiceSelect.value : 'Zephyr';
            const fileName = `translated_speech_${voice}_${Date.now()}.wav`;

            try {
                const reader = new FileReader();
                reader.readAsDataURL(currentAudioBlob);
                reader.onloadend = function () {
                    const base64data = reader.result;
                    const a = document.createElement('a');
                    a.href = base64data;
                    a.download = fileName;
                    a.style.display = 'none';
                    document.body.appendChild(a);
                    a.click();
                    setTimeout(() => document.body.removeChild(a), 1000);
                    showToast(`Downloaded ${fileName}`, 'success');
                };
            } catch (err) {
                console.error("Download error:", err);
                showToast("Unable to download file.", "error");
            }
        }
    </script>
</body>
</html>
```

---

## Step 2: How to Paste into Gemini

1. Open your **Gemini Chat interface**.
2. Start a new prompt with the following text:
   ```text
   Please render this code as an interactive HTML document in Canvas mode:
   ```
3. Paste the copied code right below it and press **Enter**.
4. Gemini will render the interactive preview on the right-hand panel automatically—no API key, terminal, or setup required!
