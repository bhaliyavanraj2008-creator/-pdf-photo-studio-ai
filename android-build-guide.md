```html
<!DOCTYPE html>
<html lang="gu" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="theme-color" content="#090d16">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <title>PDF Photo Studio AI | Complete Audited Super Studio</title>

    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['"Noto Sans Gujarati"', 'Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>

    <!-- Google Fonts: Inter + Noto Sans Gujarati -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Noto+Sans+Gujarati:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- PDF-Lib -->
    <script src="https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js"></script>
    <!-- PDF.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
    <!-- JSZip for Batch Export & Buildable Android APK/AAB ZIP -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>

    <style>
        * { -webkit-tap-highlight-color: transparent; box-sizing: border-box; }
        body { font-family: 'Noto Sans Gujarati', 'Inter', sans-serif; overflow-x: hidden; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        .checkerboard {
            background-image: linear-gradient(45deg, #64748b26 25%, transparent 25%),
                              linear-gradient(-45deg, #64748b26 25%, transparent 25%),
                              linear-gradient(45deg, transparent 75%, #64748b26 75%),
                              linear-gradient(-45deg, transparent 75%, #64748b26 75%);
            background-size: 16px 16px;
            background-position: 0 0, 0 8px, 8px -8px, -8px 0;
        }
        .glass {
            background: rgba(15, 23, 42, 0.78);
            backdrop-filter: blur(14px);
            -webkit-backdrop-filter: blur(14px);
        }
        html:not(.dark) .glass {
            background: rgba(255, 255, 255, 0.88);
        }
        @keyframes fadeUp {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .view-enter { animation: fadeUp 0.2s ease-out forwards; }
        input[type="range"] { accent-color: #6366f1; }
        .perspective-3d {
            transition: transform 0.15s ease-out;
            transform-style: preserve-3d;
        }
        @media print {
            body * { visibility: hidden; }
            #printZone, #printZone * { visibility: visible; }
            #printZone { position: fixed; inset: 0; background: white; color: black; padding: 24px; }
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-900 dark:bg-[#090d16] dark:text-slate-100 min-h-screen flex flex-col pb-24 transition-colors duration-200">

    <!-- Hidden Print Zone for Direct PDF / Document Print -->
    <div id="printZone" class="hidden"></div>

    <!-- Toast Notification Stack -->
    <div id="toastContainer" class="fixed top-16 right-4 left-4 sm:left-auto sm:w-80 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <!-- =========================================================
         TOP GLASS HEADER BAR
    ========================================================== -->
    <header class="sticky top-0 z-40 glass border-b border-slate-200 dark:border-slate-800/80 px-3 sm:px-5 py-2.5">
        <div class="max-w-6xl mx-auto flex items-center justify-between gap-2">
            <!-- Brand -->
            <div class="flex items-center gap-2.5 cursor-pointer min-w-0" onclick="switchTab('home')">
                <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-indigo-600 via-violet-600 to-fuchsia-500 flex items-center justify-center text-white shadow-lg shadow-indigo-500/30 shrink-0">
                    <i data-lucide="sparkles" class="w-5 h-5"></i>
                </div>
                <div class="min-w-0">
                    <div class="flex items-center gap-1.5">
                        <h1 class="text-sm sm:text-base font-extrabold tracking-tight truncate bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 bg-clip-text text-transparent">
                            PDF Photo Studio AI
                        </h1>
                        <span id="planBadge" onclick="event.stopPropagation(); openModal('planModal')" class="px-1.5 py-0.5 rounded-md text-[10px] font-extrabold bg-amber-500/20 text-amber-500 border border-amber-500/30 cursor-pointer shrink-0">
                            FREE
                        </span>
                    </div>
                    <p class="text-[10px] text-slate-500 dark:text-slate-400 truncate" data-i18n="tagline">Audited All-in-One AI Super Studio</p>
                </div>
            </div>

            <!-- Right Controls -->
            <div class="flex items-center gap-1 sm:gap-2 shrink-0">
                <!-- Feature Audit Checklist Trigger -->
                <button onclick="openAuditModal()" title="Full 74-Feature Audit Checklist" class="px-2.5 py-1.5 rounded-xl bg-indigo-600/15 text-indigo-500 hover:bg-indigo-600 hover:text-white text-[11px] font-extrabold flex items-center gap-1 transition">
                    <i data-lucide="check-circle-2" class="w-3.5 h-3.5"></i>
                    <span class="hidden md:inline">Audit (74/74)</span>
                </button>
                <!-- Language Toggle -->
                <button onclick="toggleLanguage()" class="px-2 py-1.5 rounded-xl bg-slate-200/80 dark:bg-slate-800 hover:bg-indigo-500/20 text-[11px] font-bold transition">
                    <span id="langToggleLabel">ગુજ / EN</span>
                </button>
                <!-- Personal AI Assistant -->
                <button onclick="openModal('assistantModal')" title="Personal AI Assistant" class="p-2 rounded-xl bg-slate-200/80 dark:bg-slate-800 text-indigo-500 hover:bg-indigo-600 hover:text-white transition">
                    <i data-lucide="bot" class="w-4 h-4"></i>
                </button>
                <!-- Theme Toggle -->
                <button onclick="toggleTheme()" class="p-2 rounded-xl bg-slate-200/80 dark:bg-slate-800 text-slate-700 dark:text-amber-400 transition">
                    <i id="themeIcon" data-lucide="sun" class="w-4 h-4"></i>
                </button>
                <!-- APK & AAB Builder CTA -->
                <button onclick="openModal('apkModal')" class="px-2.5 py-1.5 rounded-xl bg-gradient-to-r from-emerald-600 to-teal-600 text-white text-[11px] font-bold flex items-center gap-1 shadow-md shadow-emerald-500/20">
                    <i data-lucide="smartphone" class="w-3.5 h-3.5"></i>
                    <span>APK/AAB</span>
                </button>
            </div>
        </div>
    </header>

    <!-- =========================================================
         PERSONAL AI QUICK COMMAND & VOICE BAR
    ========================================================== -->
    <div class="max-w-6xl w-full mx-auto px-3 sm:px-5 pt-3">
        <div class="flex items-center gap-2 bg-white dark:bg-slate-900/90 border border-slate-200 dark:border-slate-800 rounded-2xl px-3 py-2 shadow-sm">
            <i data-lucide="sparkles" class="w-4 h-4 text-indigo-500 shrink-0"></i>
            <input id="globalAiCommandInput" type="text" onkeydown="if(event.key==='Enter') executeGlobalAiCommand()" placeholder="AI ને કમાન્ડ આપો (દા.ત. 'Passport Maker', 'Compress PDF', 'Collage', 'Logo', 'Audit')..." class="flex-1 bg-transparent text-xs focus:outline-none">
            <button onclick="startGlobalVoiceCommand()" class="p-1.5 rounded-xl bg-slate-100 dark:bg-slate-800 text-indigo-500 hover:bg-indigo-600 hover:text-white transition" title="Voice Command (ગુજરાતી / हिंदी / English)">
                <i data-lucide="mic" class="w-3.5 h-3.5"></i>
            </button>
            <button onclick="executeGlobalAiCommand()" class="px-3 py-1.5 rounded-xl bg-indigo-600 text-white text-[11px] font-bold">Run</button>
        </div>
    </div>

    <!-- =========================================================
         MAIN WORKSPACE CONTAINER
    ========================================================== -->
    <main class="flex-1 max-w-6xl w-full mx-auto px-3 sm:px-5 pt-4 space-y-5">

        <!-- =====================================================
             TAB 1: HOME DASHBOARD
        ====================================================== -->
        <section id="tab-home" class="tab-section view-enter space-y-5">
            <!-- Hero Banner -->
            <div class="rounded-3xl bg-gradient-to-br from-indigo-600 via-violet-600 to-fuchsia-600 p-5 sm:p-7 text-white shadow-xl relative overflow-hidden">
                <div class="relative z-10 max-w-2xl space-y-2.5">
                    <div class="flex flex-wrap items-center gap-2">
                        <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-white/15 backdrop-blur-md text-[11px] font-bold">
                            <i data-lucide="shield-check" class="w-3.5 h-3.5 text-emerald-300"></i>
                            <span>100% Audited • Privacy-First Local + Cloud AI</span>
                        </span>
                        <button onclick="openAuditModal()" class="px-3 py-1 rounded-full bg-slate-950/40 hover:bg-slate-950/60 text-[11px] font-bold border border-white/25 transition">
                            View Full Feature Checklist &rarr;
                        </button>
                    </div>
                    <h2 class="text-xl sm:text-3xl font-extrabold leading-tight" data-i18n="heroTitle">
                        PDF, Photo, Video, Audio અને Creative Design — સંપૂર્ણ Working AI Super App!
                    </h2>
                    <p class="text-xs sm:text-sm text-indigo-100 leading-relaxed" data-i18n="heroSub">
                        Merge/Split/Compress/Lock/Scan PDF, AI PDF Chat & Voice, 16K Photo Upscale, Crop/Collage/Passport/BG Gen, Photo→Video Reels & Buildable Android APK/AAB.
                    </p>
                    <div class="pt-2 flex flex-wrap gap-2">
                        <button onclick="switchTab('photo')" class="px-4 py-2.5 rounded-xl bg-white text-indigo-700 font-extrabold text-xs shadow hover:bg-indigo-50 transition">Photo AI Studio</button>
                        <button onclick="switchTab('pdf')" class="px-4 py-2.5 rounded-xl bg-slate-950/35 border border-white/25 text-white font-bold text-xs hover:bg-slate-950/50 transition">PDF AI Suite</button>
                        <button onclick="switchTab('video')" class="px-4 py-2.5 rounded-xl bg-slate-950/35 border border-white/25 text-white font-bold text-xs hover:bg-slate-950/50 transition">Video & Audio AI</button>
                        <button onclick="switchTab('creative')" class="px-4 py-2.5 rounded-xl bg-slate-950/35 border border-white/25 text-white font-bold text-xs hover:bg-slate-950/50 transition">Creative Studio</button>
                    </div>
                </div>
                <i data-lucide="layers" class="w-48 h-48 absolute -right-6 -bottom-8 text-white/10 rotate-12"></i>
            </div>

            <!-- Quick Tool Launcher Grid -->
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
                <div onclick="switchTab('photo'); selectPhotoSub('crop')" class="cursor-pointer p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-indigo-500 transition space-y-1.5 shadow-sm">
                    <div class="w-9 h-9 rounded-xl bg-indigo-500/15 text-indigo-500 flex items-center justify-center"><i data-lucide="crop" class="w-4 h-4"></i></div>
                    <h3 class="text-xs sm:text-sm font-extrabold">Crop, Rotate & Resize</h3>
                    <p class="text-[11px] text-slate-500 dark:text-slate-400">1:1, 4:5, 16:9, Passport & Flip</p>
                </div>
                <div onclick="switchTab('photo'); selectPhotoSub('passport')" class="cursor-pointer p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-sky-500 transition space-y-1.5 shadow-sm">
                    <div class="w-9 h-9 rounded-xl bg-sky-500/15 text-sky-500 flex items-center justify-center"><i data-lucide="layout-grid" class="w-4 h-4"></i></div>
                    <h3 class="text-xs sm:text-sm font-extrabold">Passport & Collage</h3>
                    <p class="text-[11px] text-slate-500 dark:text-slate-400">6–30 Copies Sheet & Multi-Grid</p>
                </div>
                <div onclick="switchTab('pdf'); selectPdfSub('tools')" class="cursor-pointer p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-rose-500 transition space-y-1.5 shadow-sm">
                    <div class="w-9 h-9 rounded-xl bg-rose-500/15 text-rose-500 flex items-center justify-center"><i data-lucide="file-lock-2" class="w-4 h-4"></i></div>
                    <h3 class="text-xs sm:text-sm font-extrabold">Compress, Lock & Scan</h3>
                    <p class="text-[11px] text-slate-500 dark:text-slate-400">Merge, Split, Sign, PDF↔Img</p>
                </div>
                <div onclick="switchTab('pdf'); selectPdfSub('chat')" class="cursor-pointer p-4 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 hover:border-emerald-500 transition space-y-1.5 shadow-sm">
                    <div class="w-9 h-9 rounded-xl bg-emerald-500/15 text-emerald-500 flex items-center justify-center"><i data-lucide="messages-square" class="w-4 h-4"></i></div>
                    <h3 class="text-xs sm:text-sm font-extrabold">AI PDF Chat & Voice</h3>
                    <p class="text-[11px] text-slate-500 dark:text-slate-400">Multi-PDF Q&A, Translate, OCR</p>
                </div>
            </div>

            <!-- Smart File Manager & Editing History -->
            <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 p-4 space-y-3">
                <div class="flex items-center justify-between">
                    <h3 class="text-sm font-extrabold flex items-center gap-2">
                        <i data-lucide="folder-kanban" class="w-4 h-4 text-indigo-500"></i>
                        <span>Smart File Manager & Local Editing History</span>
                    </h3>
                    <button onclick="clearFileHistory()" class="text-[11px] font-bold text-rose-500 hover:underline">Clear History</button>
                </div>
                <div id="fileHistoryList" class="grid grid-cols-1 sm:grid-cols-2 gap-2 text-xs">
                    <p class="col-span-full text-slate-500 py-3 text-center">કોઈ ફાઇલ હજુ સુધી સેવ થઈ નથી. કોઈ પણ ટૂલ વાપરો!</p>
                </div>
            </div>
        </section>

        <!-- =====================================================
             TAB 2: PHOTO AI STUDIO (ALL TOOLS COMPLETE)
        ====================================================== -->
        <section id="tab-photo" class="tab-section hidden view-enter space-y-4">
            <!-- Top Action Toolbar -->
            <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 p-3 flex flex-wrap items-center justify-between gap-2">
                <div class="flex flex-wrap items-center gap-1.5">
                    <label class="cursor-pointer px-3 py-2 rounded-xl bg-indigo-600 hover:bg-indigo-500 text-white text-xs font-bold flex items-center gap-1.5 shadow">
                        <i data-lucide="upload" class="w-3.5 h-3.5"></i>
                        <span>ફોટો અપલોડ</span>
                        <input type="file" id="photoInput" accept="image/*" class="hidden">
                    </label>
                    <button onclick="photoUndo()" class="p-2 rounded-xl bg-slate-100 dark:bg-slate-800 text-xs font-bold" title="Undo"><i data-lucide="undo-2" class="w-4 h-4"></i></button>
                    <button onclick="photoRedo()" class="p-2 rounded-xl bg-slate-100 dark:bg-slate-800 text-xs font-bold" title="Redo"><i data-lucide="redo-2" class="w-4 h-4"></i></button>
                    <button onclick="photoReset()" class="p-2 rounded-xl bg-slate-100 dark:bg-slate-800 text-xs font-bold" title="Reset"><i data-lucide="rotate-ccw" class="w-4 h-4"></i></button>
                    <button onpointerdown="showOriginalPreview(true)" onpointerup="showOriginalPreview(false)" onpointerleave="showOriginalPreview(false)" class="px-2.5 py-2 rounded-xl bg-slate-100 dark:bg-slate-800 text-[11px] font-bold select-none">
                        Hold: Before/After
                    </button>
                </div>
                <div class="flex items-center gap-1.5">
                    <select id="photoExportFormat" class="bg-slate-100 dark:bg-slate-800 rounded-xl px-2 py-2 text-xs font-bold">
                        <option value="image/png">PNG</option>
                        <option value="image/jpeg">JPG</option>
                        <option value="image/webp">WEBP</option>
                    </select>
                    <button onclick="sharePhotoFile()" class="p-2 rounded-xl bg-sky-600 text-white" title="Share Photo"><i data-lucide="share-2" class="w-4 h-4"></i></button>
                    <button onclick="exportPhotoFile()" class="px-3.5 py-2 rounded-xl bg-emerald-600 hover:bg-emerald-500 text-white text-xs font-bold flex items-center gap-1.5 shadow">
                        <i data-lucide="download" class="w-3.5 h-3.5"></i> Save
                    </button>
                </div>
            </div>

            <!-- Interactive Canvas Workspace (with optional 3D Parallax Tilt) -->
            <div id="photoStageWrapper" class="checkerboard rounded-2xl border border-slate-200 dark:border-slate-800 p-3 flex flex-col items-center justify-center min-h-[260px] max-h-[48vh] relative overflow-hidden">
                <canvas id="photoCanvas" class="max-w-full max-h-[40vh] rounded-lg shadow-2xl cursor-crosshair perspective-3d"></canvas>
                <div class="mt-2 flex items-center gap-2">
                    <span id="photoResolutionTag" class="px-2.5 py-0.5 rounded-full bg-slate-900/85 text-white text-[10px] font-mono">640 × 420 px</span>
                    <button id="toggle3dTiltBtn" onclick="toggle3dParallaxMode()" class="px-2.5 py-0.5 rounded-full bg-indigo-600/85 text-white text-[10px] font-bold">3D Tilt: OFF</button>
                </div>
                <div id="photoBusyOverlay" class="hidden absolute inset-0 bg-slate-950/75 backdrop-blur-sm flex flex-col items-center justify-center text-white gap-2">
                    <i data-lucide="loader-2" class="w-8 h-8 animate-spin text-indigo-400"></i>
                    <span class="text-xs font-bold">AI Processing...</span>
                </div>
            </div>

            <!-- Photo Sub-Tool Navigation Pills -->
            <div class="flex gap-1.5 overflow-x-auto no-scrollbar pb-1">
                <button onclick="selectPhotoSub('pro')" id="p-pill-pro" class="p-pill shrink-0 px-3 py-2 rounded-xl text-xs font-bold bg-indigo-600 text-white">Filters & Enhance</button>
                <button onclick="selectPhotoSub('crop')" id="p-pill-crop" class="p-pill shrink-0 px-3 py-2 rounded-xl text-xs font-bold bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800">Crop, Rotate & Resize</button>
                <but
