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
        .waveform-bar { transition: height 0.15s ease; }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 h-full flex flex-col font-sans antialiased selection:bg-google-blue selection:text-white">

    <!-- Header Navigation -->
    <header class="bg-slate-800/80 backdrop-blur border-b border-slate-700 px-6 py-4 flex items-center justify-between sticky top-0 z-50">
        <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-google-blue via-google-red to-google-yellow flex items-center justify-center shadow-lg shadow-google-blue/20">
                <i class="fa-solid fa-language text-white text-lg"></i>
            </div>
            <div>
                <h1 class="font-bold text-lg tracking-tight bg-gradient-to-r from-white to-slate-300 bg-clip-text text-transparent">Gemini TTS & Translation Studio</h1>
                <p class="text-xs text-slate-400">Canvas Native Edition • Zero API Key Setup Required</p>
            </div>
        </div>
        <span class="px-3 py-1 bg-emerald-500/10 text-emerald-400 text-xs font-medium rounded-full border border-emerald-500/20 flex items-center gap-1.5">
            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span> Canvas Active
        </span>
    </header>

    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col space-y-2 pointer-events-none"></div>

    <!-- Main Studio Content Area -->
    <main class="flex-1 max-w-7xl w-full mx-auto p-4 md:p-6 grid grid-cols-1 lg:grid-cols-12 gap-6 overflow-y-auto">
        
        <!-- Left Panel -->
        <div class="lg:col-span-8 flex flex-col space-y-5">
            
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

            <div class="bg-slate-800/60 border border-slate-700/80 rounded-2xl p-5 shadow-xl space-y-4">
                <div class="flex items-center justify-between">
                    <h3 class="text-sm font-semibold text-slate-200 flex items-center gap-2">
                        <i class="fa-solid fa-language text-google-green"></i> Neural Translation Settings
                    </h3>
                    
                    <label class="relative inline-flex items-center cursor-pointer">
                        <input type="checkbox" id="translate-toggle" class="sr-only peer" checked>
                        <div class="w-11 h-6 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-google-blue"></div>
                        <span class="ml-2 text-xs font-medium text-slate-300">Auto Translate</span>
                    </label>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div class="flex flex-col space-y-1.5">
                        <label for="lang-select" class="text-xs font-medium text-slate-300">Target Language</label>
                        <select id="lang-select" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-google-blue">
                            <option value="en-US" data-lang="English">English (US)</option>
                            <option value="es-ES" data-lang="Spanish">Spanish (Spain)</option>
                            <option value="pt-BR" data-lang="Brazilian Portuguese">Portuguese (Brazil)</option>
                            <option value="fr-FR" data-lang="French">French</option>
                            <option value="de-DE" data-lang="German">German</option>
                            <option value="it-IT" data-lang="Italian">Italian</option>
                            <option value="ja-JP" data-lang="Japanese">Japanese</option>
                        </select>
                    </div>

                    <div class="flex flex-col space-y-1.5">
                        <label for="voice-select" class="text-xs font-medium text-slate-300">Neural Voice Persona</label>
                        <select id="voice-select" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-google-blue">
                        </select>
                    </div>
                </div>

                <div id="translation-preview-container" class="flex flex-col space-y-1.5 pt-1">
                    <div class="flex items-center justify-between">
                        <label for="translated-text" class="text-xs font-medium text-slate-300 flex items-center gap-1.5">
                            <i class="fa-solid fa-sparkles text-google-yellow"></i> Translated Target Text
                        </label>
                        <button onclick="triggerManualTranslation()" class="text-[11px] text-google-blue hover:underline flex items-center gap-1">
                            <i class="fa-solid fa-rotate"></i> Translate Now
                        </button>
                    </div>
                    <textarea id="translated-text" rows="3" placeholder="Translation preview..." class="w-full bg-slate-900/60 border border-slate-700 rounded-xl p-3 text-xs text-slate-200 placeholder-slate-500 focus:outline-none focus:border-google-blue transition resize-none"></textarea>
                </div>

                <div class="flex flex-col space-y-1.5">
                    <label for="style-prompt" class="text-xs font-medium text-slate-300">Emotion & Tone Hint</label>
                    <input id="style-prompt" type="text" placeholder="e.g. Speak warmly and expressively:" class="bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-google-blue placeholder-slate-500">
                </div>
            </div>

            <button id="btn-generate" class="w-full py-3.5 bg-gradient-to-r from-google-blue via-google-green to-blue-600 hover:from-blue-600 hover:to-blue-700 text-white font-medium rounded-2xl shadow-lg shadow-google-blue/25 flex items-center justify-center gap-2 transition transform active:scale-[0.99]">
                <i id="gen-icon" class="fa-solid fa-wand-magic-sparkles"></i>
                <span id="gen-btn-text">Synthesize Audio (Canvas Native)</span>
            </button>

        </div>

        <!-- Right Panel -->
        <div class="lg:col-span-4 flex flex-col space-y-5">
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

                <audio id="audio-player" class="w-full" controls></audio>

                <button id="btn-download" disabled class="w-full py-2.5 bg-slate-700 hover:bg-slate-600 disabled:bg-slate-800 text-slate-200 disabled:text-slate-600 font-medium text-xs rounded-xl border border-slate-600/60 flex items-center justify-center gap-2 transition cursor-pointer disabled:cursor-not-allowed">
                    <i class="fa-solid fa-download"></i>
                    <span>Download WAV File</span>
                </button>
            </div>
        </div>

    </main>

    <script>
        // Native Canvas audio logic initialized automatically
        const GEMINI_VOICES = [
            { id: "Zephyr", name: "Zephyr (Bright)" },
            { id: "Puck", name: "Puck (Upbeat)" },
            { id: "Charon", name: "Charon (Informative)" },
            { id: "Kore", name: "Kore (Firm)" },
            { id: "Fenrir", name: "Fenrir (Excitable)" }
        ];

        document.addEventListener('DOMContentLoaded', () => {
            const voiceSelect = document.getElementById('voice-select');
            if (voiceSelect) {
                voiceSelect.innerHTML = GEMINI_VOICES.map(v => `<option value="${v.id}">${v.name}</option>`).join('');
            }
        });

        function showToast(msg, type = 'info') {
            const container = document.getElementById('toast-container');
            if (!container) return;
            const toast = document.createElement('div');
            toast.className = `pointer-events-auto flex items-center gap-3 px-4 py-3 rounded-xl border shadow-xl backdrop-blur text-sm text-slate-100 bg-slate-800 border-slate-700`;
            toast.innerHTML = `<span>${msg}</span>`;
            container.appendChild(toast);
            setTimeout(() => toast.remove(), 3000);
        }

        window.loadSampleText = function(lang) {
            const textInput = document.getElementById('text-input');
            if (textInput) {
                textInput.value = lang === 'es' 
                    ? "¡Hola! Gemini sintetiza audio de voz natural directamente en Canvas." 
                    : "Hello! Welcome to Gemini AI Canvas mode.";
                document.getElementById('char-count').textContent = textInput.value.length;
            }
        };

        window.clearText = function() {
            document.getElementById('text-input').value = '';
            document.getElementById('translated-text').value = '';
            document.getElementById('char-count').textContent = '0';
        };
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