\<!DOCTYPE html>
\<html lang="zh-TW">
\<head>
\<meta charset="UTF-8">
\<meta name="viewport" content="width=device-width, initial-scale=1.0">
\<title>理想狀態 - Archive Portal\</title>
\<script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)">\</script>
\<script src="[https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation/selfie_segmentation.js](https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation/selfie_segmentation.js)" crossorigin="anonymous">\</script>

\<style>
@import url('[https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;600;800&family=Noto+Sans+TC:wght@300;400;500;700;900&display=swap](https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;600;800\&family=Noto+Sans+TC:wght@300;400;500;700;900\&display=swap)');

\* {
&#x20; box-sizing: border-box;
}

body {
&#x20; font-family: 'JetBrains Mono', 'Noto Sans TC', monospace;
&#x20; background-color: #08090a;
&#x20; color: #d1d5db;
&#x20; overflow-x: hidden;
}

.bg-grid-dark {
&#x20; background-size: 32px 32px;
&#x20; background-image:
&#x20;   linear-gradient(to right, rgba(255,255,255,.012) 1px, transparent 1px),
&#x20;   linear-gradient(to bottom, rgba(255,255,255,.012) 1px, transparent 1px);
}

.glass-card {
&#x20; background: rgba(15,18,22,.85);
&#x20; backdrop-filter: blur(16px);
&#x20; border: 1px solid rgba(255,255,255,.08);
&#x20; transition: all .35s cubic-bezier(.16,1,.3,1);
}

.glass-card:hover {
&#x20; border-color: rgba(16,185,129,.4);
&#x20; transform: translateY(-4px);
&#x20; box-shadow:
&#x20;   0 16px 36px -10px rgba(0,0,0,.9),
&#x20;   0 0 24px rgba(16,185,129,.08);
}

.btn-white {
&#x20; background: #fff;
&#x20; color: #000;
&#x20; font-weight: 700;
&#x20; border: none;
&#x20; transition: all .25s ease;
}

.btn-white:hover {
&#x20; background: #e5e7eb;
&#x20; box-shadow: 0 0 16px rgba(255,255,255,.3);
}

.btn-neon {
&#x20; background: #39ff14;
&#x20; color: #000;
&#x20; font-weight: 800;
&#x20; border: none;
&#x20; transition: all .2s ease;
}

.btn-neon:hover {
&#x20; background: #55ff33;
&#x20; box-shadow: 0 0 20px rgba(57,255,20,.6);
}

.badge-emerald-active {
&#x20; background: rgba(16,185,129,.2) !important;
&#x20; color: #10b981 !important;
&#x20; font-weight: 700 !important;
&#x20; border: 1px solid rgba(16,185,129,.5) !important;
&#x20; box-shadow: 0 0 12px rgba(16,185,129,.25);
}

.badge-inactive {
&#x20; background: rgba(15,18,22,.85) !important;
&#x20; color: #6b7280 !important;
&#x20; font-weight: 500 !important;
&#x20; border: 1px solid rgba(255,255,255,.1) !important;
&#x20; box-shadow: none !important;
}

.stamp-red {
&#x20; border: 2px solid #ef4444;
&#x20; color: #ef4444;
&#x20; transform: rotate(-6deg);
&#x20; letter-spacing: .15em;
&#x20; text-shadow: 0 0 10px rgba(239,68,68,.3);
&#x20; box-shadow:
&#x20;   inset 0 0 12px rgba(239,68,68,.12),
&#x20;   0 0 8px rgba(239,68,68,.2);
}

@keyframes pulse-emerald {
&#x20; 0%,100% {
&#x20;   color: #10b981;
&#x20;   opacity: 1;
&#x20; }
&#x20; 50% {
&#x20;   color: #6ee7b7;
&#x20;   opacity: .7;
&#x20; }
}

.breath-counter-active {
&#x20; animation: pulse-emerald 2s infinite;
}

@keyframes mobileFade {
&#x20; 0%,45% { opacity: 0; }
&#x20; 55%,95% { opacity: 1; }
&#x20; 100% { opacity: 0; }
}

@keyframes mobileBgFade {
&#x20; 0%,45% {
&#x20;   background-color: rgba(0,0,0,.7);
&#x20;   backdrop-filter: blur(6px);
&#x20; }
&#x20; 55%,95% {
&#x20;   background-color: transparent;
&#x20;   backdrop-filter: none;
&#x20; }
&#x20; 100% {
&#x20;   background-color: rgba(0,0,0,.7);
&#x20;   backdrop-filter: blur(6px);
&#x20; }
}

@media (hover: none) {
&#x20; .mobile-auto-fade {
&#x20;   animation: mobileFade 6s infinite ease-in-out;
&#x20; }

&#x20; .mobile-auto-bg-fade {
&#x20;   animation: mobileBgFade 6s infinite ease-in-out;
&#x20; }
}

\#comparison-slider {
&#x20; touch-action: none;
}

.modal-backdrop {
&#x20; transition: opacity .3s cubic-bezier(.16,1,.3,1);
}

.modal-backdrop.hidden-modal {
&#x20; opacity: 0;
&#x20; pointer-events: none;
}

.modal-backdrop.active-modal {
&#x20; opacity: 1;
&#x20; pointer-events: auto;
}

.modal-content {
&#x20; transition:
&#x20;   transform .3s cubic-bezier(.16,1,.3,1),
&#x20;   opacity .3s cubic-bezier(.16,1,.3,1);
}

.hidden-modal .modal-content {
&#x20; transform: scale(.94) translateY(12px);
&#x20; opacity: 0;
}

.active-modal .modal-content {
&#x20; transform: scale(1) translateY(0);
&#x20; opacity: 1;
}

.clinical-input {
&#x20; display: block;
&#x20; width: 100%;
&#x20; max-width: 100%;
&#x20; min-width: 0;
&#x20; min-height: 44px;
&#x20; background-color: rgba(3, 7, 18, 0.92);
&#x20; border: 1px solid #1f2937;
&#x20; border-radius: 6px;
&#x20; padding: 10px 14px;
&#x20; font-size: 13px;
&#x20; line-height: 1.4;
&#x20; color: #ffffff;
&#x20; outline: none;
&#x20; appearance: none;
&#x20; -webkit-appearance: none;
&#x20; transition: border-color .2s ease, box-shadow .2s ease;
}

.clinical-input:focus {
&#x20; border-color: #10b981;
&#x20; box-shadow: 0 0 0 1px rgba(16, 185, 129, 0.25);
}

input[type="date"].clinical-input {
&#x20; color-scheme: dark;
&#x20; height: 44px;
}

input[type="date"].clinical-input::-webkit-date-and-time-value {
&#x20; text-align: left;
}

.archive-header-main {
&#x20; display: flex;
&#x20; flex-direction: column;
&#x20; gap: 24px;
}

.archive-title-group {
&#x20; display: flex;
&#x20; flex-direction: column;
&#x20; align-items: flex-start;
&#x20; min-width: 0;
&#x20; flex: 1;
}

.archive-eyebrow {
&#x20; font-size: 10px;
&#x20; letter-spacing: .25em;
&#x20; color: #6b7280;
&#x20; margin: 0 0 16px;
&#x20; padding: 0;
}

.archive-title {
&#x20; font-family: 'Noto Sans TC', sans-serif;
&#x20; font-size: 64px !important;
&#x20; line-height: 1.1;
&#x20; letter-spacing: -.04em;
&#x20; font-weight: 700;
&#x20; color: #f3f4f6;
&#x20; margin: 0 !important;
&#x20; padding: 0 !important;
&#x20; border: 0 !important;
&#x20; outline: 0 !important;
&#x20; box-shadow: none !important;
&#x20; text-decoration: none !important;
}

.archive-subtitle {
&#x20; font-size: clamp(13px,1.3vw,18px);
&#x20; line-height: 1.5;
&#x20; letter-spacing: .03em;
&#x20; color: #6b7280;
&#x20; margin: 8px 0 0 !important;
&#x20; padding: 0 !important;
&#x20; border: 0 !important;
&#x20; outline: 0 !important;
&#x20; box-shadow: none !important;
}

.archive-title-group hr,
.archive-title-divider,
.archive-title::before,
.archive-title::after,
.archive-subtitle::before,
.archive-subtitle::after {
&#x20; display: none !important;
&#x20; content: none !important;
}

.archive-header-actions {
&#x20; display: flex;
&#x20; flex-wrap: wrap;
&#x20; align-items: center;
&#x20; gap: 12px;
&#x20; flex-shrink: 0;
}

\#header-concept-button,
\#header-output-box {
&#x20; border-radius: 4px !important;
&#x20; font-size: 12px;
&#x20; line-height: 20px;
&#x20; min-height: 36px;
&#x20; padding: 8px 16px;
&#x20; white-space: nowrap;
}

.concept-modal-footer {
&#x20; display: flex !important;
&#x20; justify-content: center !important;
&#x20; align-items: center !important;
&#x20; width: 100% !important;
&#x20; margin-left: auto !important;
&#x20; margin-right: auto !important;
&#x20; text-align: center !important;
}

\#concept-close-button {
&#x20; display: inline-flex !important;
&#x20; align-items: center !important;
&#x20; justify-content: center !important;
&#x20; flex: 0 0 auto;
&#x20; margin-left: auto !important;
&#x20; margin-right: auto !important;
&#x20; padding: 8px 20px;
&#x20; background: #ffffff !important;
&#x20; background-color: #ffffff !important;
&#x20; color: #000000 !important;
&#x20; -webkit-text-fill-color: #000000 !important;
&#x20; border: 1px solid #ffffff !important;
&#x20; border-radius: 4px;
&#x20; font-weight: 700 !important;
&#x20; text-align: center !important;
&#x20; opacity: 1 !important;
&#x20; appearance: none;
&#x20; -webkit-appearance: none;
&#x20; cursor: pointer;
&#x20; transition: background-color .2s ease;
}

\#concept-close-button:hover,
\#concept-close-button:focus,
\#concept-close-button:active {
&#x20; background: #e5e7eb !important;
&#x20; background-color: #e5e7eb !important;
&#x20; color: #000000 !important;
&#x20; -webkit-text-fill-color: #000000 !important;
&#x20; border-color: #e5e7eb !important;
}

@media (min-width: 768px) {
&#x20; .archive-header-main {
&#x20;   flex-direction: row;
&#x20;   align-items: flex-end;
&#x20;   justify-content: space-between;
&#x20; }

&#x20; .archive-header-actions {
&#x20;   align-self: flex-end;
&#x20; }
}

@media (max-width: 767px) {
&#x20; .archive-title {
&#x20;   font-size: 56px !important;
&#x20; }

&#x20; .archive-subtitle {
&#x20;   font-size: clamp(12px,3.5vw,16px);
&#x20; }
}
\</style>
\</head>

\<body class="bg-grid-dark min-h-screen text-gray-300 relative selection:bg-emerald-500 selection:text-black">

\<div class="fixed top-0 left-1/2 -translate-x-1/2 w-full max-w-6xl h-80 bg-emerald-950/20 blur-[130px] pointer-events-none rounded-full">\</div>

\<div class="max-w-5xl mx-auto px-4 sm:px-6 py-8 md:py-14 relative z-10">

&#x20; \<!-- HEADER -->

&#x20; \<header class="border-b border-gray-700/70 pb-8 mb-12">

&#x20;   \<div class="flex flex-wrap items-center justify-between gap-3 mb-8 font-mono text-[10px] sm:text-xs tracking-[0.14em]">

&#x20;     \<div class="flex items-center gap-2 text-emerald-400">
&#x20;       \<span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse">\</span>
&#x20;       \<span>
&#x20;         SYSTEM RUNNING
&#x20;         \<span class="text-gray-600 mx-1">/\</span>
&#x20;         CLINICAL ARCHIVE PORTAL
&#x20;       \</span>
&#x20;     \</div>

&#x20;     \<span class="text-gray-500 tracking-[0.18em]">
&#x20;       ARCHIVE / 2026 / 001
&#x20;     \</span>

&#x20;   \</div>

&#x20;   \<div class="archive-header-main">

&#x20;     \<div class="archive-title-group">

&#x20;       \<div class="archive-eyebrow">
&#x20;         IDEAL STATE — ARCHIVE PORTAL
&#x20;       \</div>

&#x20;       \<h1 class="archive-title">理想狀態\</h1>

&#x20;       \<p class="archive-subtitle">
&#x20;         個人焦慮檔案與系統診斷報告
&#x20;         \<span class="text-gray-600">/\</span>
&#x20;         Archive Portal
&#x20;       \</p>

&#x20;     \</div>

&#x20;     \<div class="archive-header-actions">

&#x20;       \<button
&#x20;         id="header-concept-button"
&#x20;         onclick="openConceptModal()"
&#x20;         class="btn-white inline-flex items-center justify-center gap-2 shadow-sm font-mono"
&#x20;       \>
&#x20;         \<span>ⓘ\</span>
&#x20;         \<span>作品理念 (CONCEPT)\</span>
&#x20;       \</button>

&#x20;       \<div
&#x20;         id="header-output-box"
&#x20;         class="inline-flex items-center gap-3 font-mono text-gray-400 bg-gray-950/80 border border-gray-700/80"
&#x20;       \>
&#x20;         \<span class="text-gray-500">VALUABLE OUTPUT:\</span>
&#x20;         \<span id="header-output-counter" class="text-emerald-400 font-bold">
&#x20;           0.000 KG/H
&#x20;         \</span>
&#x20;       \</div>

&#x20;     \</div>

&#x20;   \</div>

&#x20; \</header>

&#x20; \<!-- 首頁 -->

&#x20; \<main id="portal-view" class="space-y-14">

&#x20;   \<!-- 原始四位委託人檔案目錄 -->

&#x20;   \<section class="space-y-8">

&#x20;     \<div class="flex justify-between items-end border-b border-gray-800/60 pb-3 gap-4">

&#x20;       \<div>
&#x20;         \<h2 class="text-lg font-bold text-white tracking-wide">
&#x20;           委託人檔案目錄
&#x20;         \</h2>

&#x20;         \<p class="text-xs text-gray-500">
&#x20;           點擊個案卡片，檢視完整「臨床診斷報告與生命累積呼吸數據」
&#x20;         \</p>
&#x20;       \</div>

&#x20;       \<span class="text-xs text-gray-600 font-mono shrink-0">
&#x20;         TOTAL: 04 CASES
&#x20;       \</span>

&#x20;     \</div>

&#x20;     \<div
&#x20;       id="cards-container"
&#x20;       class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6"
&#x20;     \>\</div>

&#x20;   \</section>

&#x20;   \<!-- 現場觀眾病例獨立區塊 -->

&#x20;   \<section class="border-t border-gray-800/80 pt-12 space-y-8">

&#x20;     \<div class="flex flex-wrap justify-between items-end border-b border-gray-800/60 pb-3 gap-3">

&#x20;       \<div>
&#x20;         \<span class="text-[10px] font-mono text-emerald-400 tracking-widest block mb-1">
&#x20;           PUBLIC ARCHIVE EXTENSION / 現場觀眾延伸檔案
&#x20;         \</span>

&#x20;         \<h2 class="text-lg font-bold text-white tracking-wide">
&#x20;           現場觀眾病例檔案目錄
&#x20;         \</h2>

&#x20;         \<p class="text-xs text-gray-500">
&#x20;           跨裝置同步收錄觀眾提交之個人焦慮病例（上傳後 3 分鐘內可於原裝置修改或刪除）
&#x20;         \</p>
&#x20;       \</div>

&#x20;       \<span id="visitor-total-label" class="text-xs text-gray-600 font-mono shrink-0">
&#x20;         VISITOR CASES: 00
&#x20;       \</span>

&#x20;     \</div>

&#x20;     \<div
&#x20;       id="visitor-cards-container"
&#x20;       class="grid grid-cols-2 md:grid-cols-2 lg:grid-cols-4 gap-3.5 sm:gap-6"
&#x20;     \>\</div>

&#x20;     \<!-- 現場觀眾病例登錄表單 -->

&#x20;     \<div id="visitor-case-section" class="glass-card rounded-xl p-5 sm:p-6 md:p-8 space-y-6 w-full overflow-hidden">

&#x20;       \<div class="flex flex-col sm:flex-row sm:items-end justify-between border-b border-gray-800/80 pb-4 gap-2">

&#x20;         \<div class="min-w-0">
&#x20;           \<span class="text-[10px] font-mono text-emerald-400 tracking-widest block mb-1">
&#x20;             VISITOR INTAKE TERMINAL / 現場病例登錄
&#x20;           \</span>

&#x20;           \<h3 id="form-mode-title" class="text-base sm:text-lg font-bold text-white tracking-wide">
&#x20;             書寫你的病例檔案
&#x20;           \</h3>

&#x20;           \<p class="text-xs text-gray-500 mt-1 leading-relaxed">
&#x20;             只需填寫焦慮自白即可建立病例（其餘皆可選填）；上傳後 3 分鐘內僅限原上傳裝置可編輯或刪除
&#x20;           \</p>
&#x20;         \</div>

&#x20;         \<span id="next-case-preview" class="text-xs font-mono text-emerald-400 shrink-0">
&#x20;           NEXT ID: CASE #05
&#x20;         \</span>

&#x20;       \</div>

&#x20;       \<form id="visitor-case-form" class="space-y-5 font-mono text-xs w-full">

&#x20;         \<div class="w-full">
&#x20;           \<label for="visitor-statement" class="block text-[10px] text-emerald-400 font-bold mb-1.5">
&#x20;             本體焦慮自白紀錄 (ANXIETY STATEMENT) \* 必填
&#x20;           \</label>
&#x20;           \<textarea
&#x20;             id="visitor-statement"
&#x20;             rows="4"
&#x20;             required
&#x20;             placeholder="寫下你面對空閒、休息或沒有產出時的真實感受與焦慮自白..."
&#x20;             class="clinical-input font-sans leading-relaxed"
&#x20;           \>\</textarea>
&#x20;         \</div>

&#x20;         \<div class="grid grid-cols-1 sm:grid-cols-3 gap-4 w-full">

&#x20;           \<div class="w-full min-w-0">
&#x20;             \<label for="visitor-name" class="block text-[10px] text-gray-400 mb-1.5">
&#x20;               委託人姓名 / 代號 (選填)
&#x20;             \</label>
&#x20;             \<input
&#x20;               type="text"
&#x20;               id="visitor-name"
&#x20;               placeholder="例如：王Ｏ明（留空自動編號）"
&#x20;               class="clinical-input"
&#x20;             \>
&#x20;           \</div>

&#x20;           \<div class="w-full min-w-0">
&#x20;             \<label for="visitor-birth" class="block text-[10px] text-gray-400 mb-1.5">
&#x20;               出生日期 (選填，計算呼吸數)
&#x20;             \</label>
&#x20;             \<input
&#x20;               type="date"
&#x20;               id="visitor-birth"
&#x20;               class="clinical-input"
&#x20;             \>
&#x20;           \</div>

&#x20;           \<div class="w-full min-w-0">
&#x20;             \<label for="visitor-anxiety" class="block text-[10px] text-gray-400 mb-1.5">
&#x20;               焦慮指數 (選填)
&#x20;             \</label>
&#x20;             \<input
&#x20;               type="text"
&#x20;               id="visitor-anxiety"
&#x20;               placeholder="例如：89.5% (極高)"
&#x20;               class="clinical-input"
&#x20;             \>
&#x20;           \</div>

&#x20;         \</div>

&#x20;         \<!-- 拍照與選取照片建檔區（選填） -->

&#x20;         \<div class="bg-gray-950/75 border border-gray-800/90 rounded-lg p-4 sm:p-5 space-y-4 w-full">

&#x20;           \<div class="space-y-1">
&#x20;             \<span class="block text-[11px] text-gray-200 font-bold">
&#x20;               現場本體影像拍攝 / 選取照片 (選填 CAMERA / PHOTO)
&#x20;             \</span>
&#x20;             \<span class="block text-[10px] text-gray-500 font-sans leading-relaxed">
&#x20;               點擊下方按鈕可拍照或從相簿選取照片，系統將自動去背並套用黑白灰階與微調曝光
&#x20;             \</span>
&#x20;           \</div>

&#x20;           \<div class="w-full">

&#x20;             \<label
&#x20;               for="visitor-photo-input"
&#x20;               class="btn-white w-full py-2.5 px-4 rounded text-xs inline-flex items-center justify-center gap-2 cursor-pointer text-center"
&#x20;             \>
&#x20;               \<span>📷\</span>
&#x20;               \<span>拍照 / 選取照片\</span>
&#x20;             \</label>

&#x20;             \<input
&#x20;               type="file"
&#x20;               id="visitor-photo-input"
&#x20;               accept="image/\*"
&#x20;               class="hidden"
&#x20;             \>

&#x20;           \</div>

&#x20;           \<div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 pt-2 border-t border-gray-800/70">

&#x20;             \<button
&#x20;               type="button"
&#x20;               id="privacy-blur-toggle"
&#x20;               onclick="togglePrivacyBlur()"
&#x20;               class="w-full sm:w-auto px-3.5 py-2 rounded text-xs font-mono inline-flex items-center justify-center gap-2 border transition-all cursor-pointer bg-gray-900/90 text-gray-300 border-gray-700 hover:border-emerald-500/60"
&#x20;             \>
&#x20;               \<span id="privacy-blur-indicator" class="w-2 h-2 rounded-full bg-gray-500">\</span>
&#x20;               \<span id="privacy-blur-label">隱私模糊保護：關閉 (點擊模糊化臉部)\</span>
&#x20;             \</button>

&#x20;             \<button
&#x20;               type="button"
&#x20;               id="clear-photo-btn"
&#x20;               onclick="clearCapturedPhoto()"
&#x20;               class="hidden w-full sm:w-auto px-3.5 py-2 rounded border border-red-900/60 bg-red-950/20 text-red-400 hover:bg-red-950/40 text-xs text-center"
&#x20;             \>
&#x20;               清除已選照片
&#x20;             \</button>

&#x20;           \</div>

&#x20;           \<div id="photo-processing-status" class="hidden text-[11px] text-emerald-400 flex items-center gap-2">
&#x20;             \<span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping">\</span>
&#x20;             \<span>正在進行人像去背、黑白灰階與曝光微調處理...\</span>
&#x20;           \</div>

&#x20;           \<div id="photo-preview-box" class="hidden pt-3 border-t border-gray-800/80 flex items-center gap-4">

&#x20;             \<img
&#x20;               id="photo-preview-img"
&#x20;               src=""
&#x20;               alt="Captured Preview"
&#x20;               class="w-16 h-20 object-cover rounded border border-emerald-500/40 bg-black shrink-0"
&#x20;             \>

&#x20;             \<div class="text-[10px] space-y-1 min-w-0">
&#x20;               \<span id="photo-preview-badge" class="text-emerald-400 font-bold block">
&#x20;                 ✓ 影像已完成去背與黑白曝光調整
&#x20;               \</span>
&#x20;               \<span class="text-gray-500 block font-sans leading-relaxed">
&#x20;                 已套用純黑底色與自然黑白灰階；可隨時切換上方「隱私模糊保護」確認遮蔽效果
&#x20;               \</span>
&#x20;             \</div>

&#x20;           \</div>

&#x20;         \</div>

&#x20;         \<div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 pt-1 w-full">

&#x20;           \<span id="visitor-form-feedback" class="text-[11px] text-emerald-400 min-h-[16px] leading-relaxed">\</span>

&#x20;           \<div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-2.5 w-full sm:w-auto shrink-0">

&#x20;             \<button
&#x20;               type="button"
&#x20;               id="cancel-edit-btn"
&#x20;               onclick="cancelEditMode()"
&#x20;               class="hidden w-full sm:w-auto px-4 py-2.5 rounded border border-gray-700 text-gray-300 hover:text-white transition-colors text-xs text-center"
&#x20;             \>
&#x20;               取消修改
&#x20;             \</button>

&#x20;             \<button
&#x20;               type="submit"
&#x20;               id="submit-case-btn"
&#x20;               class="btn-neon w-full sm:w-auto px-5 py-2.5 rounded text-xs inline-flex items-center justify-center gap-2 cursor-pointer text-center"
&#x20;             \>
&#x20;               \<span id="submit-case-btn-text">建立並加入觀眾病例\</span>
&#x20;               \<span>→\</span>
&#x20;             \</button>

&#x20;           \</div>

&#x20;         \</div>

&#x20;       \</form>

&#x20;     \</div>

&#x20;   \</section>

&#x20; \</main>

&#x20; \<!-- 個案詳情 -->

&#x20; \<main id="detail-view" class="hidden space-y-6">

&#x20;   \<div class="flex flex-wrap items-center justify-between gap-3">

&#x20;     \<button
&#x20;       onclick="showPortal()"
&#x20;       class="inline-flex items-center space-x-2 text-xs text-gray-300 hover:text-white transition-colors bg-gray-900/90 hover:bg-gray-800 px-4 py-2 rounded border border-gray-700"
&#x20;     \>
&#x20;       \<span>←\</span>
&#x20;       \<span>返回檔案目錄 (RETURN TO DIRECTORY)\</span>
&#x20;     \</button>

&#x20;     \<div id="detail-owner-controls" class="hidden flex flex-wrap items-center gap-2 font-mono text-xs">

&#x20;       \<span id="detail-edit-countdown" class="text-[11px] text-emerald-400 bg-emerald-950/30 border border-emerald-500/30 px-2.5 py-1.5 rounded">
&#x20;         可編輯剩餘 03:00
&#x20;       \</span>

&#x20;       \<button
&#x20;         type="button"
&#x20;         id="detail-edit-btn"
&#x20;         class="px-3.5 py-1.5 rounded bg-white text-black font-bold hover:bg-gray-200 transition-colors cursor-pointer"
&#x20;       \>
&#x20;         ✎ 修改病例
&#x20;       \</button>

&#x20;       \<button
&#x20;         type="button"
&#x20;         id="detail-delete-btn"
&#x20;         class="px-3.5 py-1.5 rounded bg-red-950/60 text-red-400 border border-red-800/70 hover:bg-red-900/60 font-bold transition-colors cursor-pointer"
&#x20;       \>
&#x20;         ✕ 刪除病例
&#x20;       \</button>

&#x20;     \</div>

&#x20;   \</div>

&#x20;   \<article class="glass-card rounded-xl p-5 sm:p-6 md:p-10 relative space-y-6">

&#x20;     \<div class="border-b border-gray-800 pb-2 space-y-1">

&#x20;       \<div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-2">

&#x20;         \<div>
&#x20;           \<span id="detail-id" class="inline-block text-white text-xs font-mono mb-2">
&#x20;             CASE #000
&#x20;           \</span>

&#x20;           \<h2 id="detail-name" class="text-2xl md:text-3xl font-bold text-white">
&#x20;             \---
&#x20;           \</h2>

&#x20;           \<p id="detail-role" class="text-xs font-mono text-gray-400 mt-1">
&#x20;             \---
&#x20;           \</p>
&#x20;         \</div>

&#x20;         \<div class="stamp-red px-4 py-1.5 rounded text-xs font-bold font-mono tracking-widest uppercase shrink-0">
&#x20;           AUTHORIZED IDLE
&#x20;         \</div>

&#x20;       \</div>

&#x20;       \<p class="text-[10px] font-mono text-gray-400 tracking-wide pb-1">
&#x20;         [SYSTEM NOTE] 個人特徵（髮型/皮肉）已於代理化過程中剝離。
&#x20;       \</p>

&#x20;     \</div>

&#x20;     \<div class="grid grid-cols-1 md:grid-cols-3 gap-6 items-stretch">

&#x20;       \<div
&#x20;         id="detail-slider-container"
&#x20;         class="w-full h-full min-h-[360px] md:min-h-full bg-gray-950 rounded-lg overflow-hidden border border-gray-800 relative select-none group"
&#x20;       \>

&#x20;         \<div
&#x20;           id="detail-no-image-fallback"
&#x20;           class="hidden absolute inset-0 flex flex-col items-center justify-center space-y-2 text-gray-700 z-20 bg-gray-950"
&#x20;         \>
&#x20;           \<svg
&#x20;             class="w-12 h-12 stroke-gray-700"
&#x20;             fill="none"
&#x20;             viewBox="0 0 24 24"
&#x20;             stroke="currentColor"
&#x20;           \>
&#x20;             \<path
&#x20;               stroke-linecap="round"
&#x20;               stroke-linejoin="round"
&#x20;               stroke-width="1.5"
&#x20;               d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
&#x20;             />
&#x20;           \</svg>
&#x20;           \<span class="text-[10px] font-mono text-gray-600">
&#x20;             NO SCAN DATA
&#x20;           \</span>
&#x20;         \</div>

&#x20;         \<!-- 觀眾病例專屬照片 -->
&#x20;         \<img
&#x20;           id="detail-visitor-img"
&#x20;           src=""
&#x20;           alt="Visitor Portrait"
&#x20;           class="hidden w-full h-full object-cover absolute inset-0 z-10 pointer-events-none"
&#x20;         \>

&#x20;         \<!-- 原始四位委託人專用結構 -->
&#x20;         \<img
&#x20;           id="detail-device-img"
&#x20;           src=""
&#x20;           alt="Proxy Device"
&#x20;           class="w-full h-full object-cover absolute inset-0 pointer-events-none z-0"
&#x20;         \>

&#x20;         \<div
&#x20;           id="slider-clip-wrapper"
&#x20;           class="absolute inset-0 overflow-hidden w-1/2 border-r-2 border-emerald-500 z-10"
&#x20;         \>
&#x20;           \<img
&#x20;             id="detail-principal-img"
&#x20;             src=""
&#x20;             alt="Principal"
&#x20;             class="w-full h-full object-cover pointer-events-none"
&#x20;             style="max-width:none;"
&#x20;           \>
&#x20;         \</div>

&#x20;         \<input
&#x20;           type="range"
&#x20;           min="0"
&#x20;           max="100"
&#x20;           value="50"
&#x20;           id="comparison-slider"
&#x20;           class="absolute inset-0 w-full h-full opacity-0 cursor-ew-resize z-30"
&#x20;         \>

&#x20;         \<div
&#x20;           id="slider-line"
&#x20;           class="absolute top-0 bottom-0 w-0.5 bg-emerald-500 pointer-events-none z-20 left-1/2 -translate-x-1/2 shadow-[0_0_8px_rgba(16,185,129,0.5)]"
&#x20;         \>
&#x20;           \<div class="w-6 h-6 rounded-full bg-emerald-500 text-black font-extrabold text-[10px] flex items-center justify-center absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 shadow-lg">
&#x20;             ◄►
&#x20;           \</div>
&#x20;         \</div>

&#x20;         \<span
&#x20;           id="badge-principal"
&#x20;           class="absolute bottom-2.5 left-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none badge-inactive"
&#x20;         \>
&#x20;           本體 PRINCIPAL
&#x20;         \</span>

&#x20;         \<span
&#x20;           id="badge-device"
&#x20;           class="absolute bottom-2.5 right-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none badge-inactive"
&#x20;         \>
&#x20;           裝置 PROXY DEVICE
&#x20;         \</span>

&#x20;       \</div>

&#x20;       \<div class="md:col-span-2 bg-gray-950/80 rounded-lg p-5 border border-gray-800/80 flex flex-col justify-between space-y-4 font-mono text-xs overflow-hidden">

&#x20;         \<div class="grid grid-cols-1 sm:grid-cols-2 gap-4 border-b border-gray-800/80 pb-4">

&#x20;           \<div>
&#x20;             \<span class="text-gray-500 text-[10px] block">
&#x20;               SERVICE TARGET (代理對象)
&#x20;             \</span>
&#x20;             \<span id="detail-target" class="font-bold text-gray-200">
&#x20;               \---
&#x20;             \</span>
&#x20;           \</div>

&#x20;           \<div>
&#x20;             \<span class="text-gray-500 text-[10px] block">
&#x20;               ANXIETY INDEX (焦慮指數)
&#x20;             \</span>
&#x20;             \<span id="detail-anxiety" class="font-bold text-red-400">
&#x20;               \---
&#x20;             \</span>
&#x20;           \</div>

&#x20;           \<div class="sm:col-span-2 bg-emerald-950/20 p-3.5 rounded border border-emerald-500/20 w-full overflow-hidden">

&#x20;             \<div class="flex justify-between items-center mb-1.5 gap-2">

&#x20;               \<span class="text-emerald-400 text-[10px] font-bold tracking-wider break-words leading-tight">
&#x20;                 LIFETIME CUMULATIVE RESPIRATION (出生至今生命累積總呼吸數)
&#x20;               \</span>

&#x20;               \<span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-ping shrink-0">\</span>

&#x20;             \</div>

&#x20;             \<div class="flex flex-wrap items-baseline gap-x-2 gap-y-1 my-1">

&#x20;               \<span
&#x20;                 id="detail-breath-counter"
&#x20;                 class="text-lg sm:text-xl md:text-2xl font-black text-emerald-400 breath-counter-active tracking-wider break-all"
&#x20;               \>
&#x20;                 0
&#x20;               \</span>

&#x20;               \<span class="text-[11px] text-gray-400 shrink-0">
&#x20;                 CYCLES (次)
&#x20;               \</span>

&#x20;             \</div>

&#x20;             \<p
&#x20;               id="detail-birth-info"
&#x20;               class="text-[10px] text-gray-500 mt-1 leading-normal break-words"
&#x20;             \>
&#x20;               自出生日期起算，依成人平靜狀態每分鐘 15 次呼吸即時動態估算
&#x20;             \</p>

&#x20;           \</div>

&#x20;           \<div>
&#x20;             \<span class="text-gray-500 text-[10px] block">
&#x20;               ECONOMIC OUTPUT (產出價值)
&#x20;             \</span>
&#x20;             \<span id="detail-output-counter" class="font-bold text-emerald-400">
&#x20;               0.000 KG/H (NULL)
&#x20;             \</span>
&#x20;           \</div>

&#x20;         \</div>

&#x20;         \<div>
&#x20;           \<span class="text-gray-500 text-[10px] block mb-1">
&#x20;             SYSTEM CLAUSE (授權條款)
&#x20;           \</span>

&#x20;           \<p class="text-gray-400 leading-relaxed text-[11px] font-sans">
&#x20;             本體因無法忍受空閒狀態產生之罪惡感，現已將「維持最低限度生物性呼吸」之權限，完全移交予代理個案執行。
&#x20;           \</p>
&#x20;         \</div>

&#x20;       \</div>

&#x20;     \</div>

&#x20;     \<div class="bg-gray-950/90 rounded-lg p-6 border border-gray-800/80 relative">

&#x20;       \<span class="text-[10px] font-mono text-gray-500 tracking-widest absolute top-3 right-4">
&#x20;         STATEMENT ARCHIVE
&#x20;       \</span>

&#x20;       \<h3 class="text-xs font-mono text-gray-400 tracking-wider mb-3">
&#x20;         【本體焦慮自白紀錄】
&#x20;       \</h3>

&#x20;       \<blockquote
&#x20;         id="detail-statement"
&#x20;         class="text-sm md:text-base text-gray-200 leading-relaxed font-sans italic border-l-2 border-emerald-500/60 pl-4 py-1"
&#x20;       \>
&#x20;         "---"
&#x20;       \</blockquote>

&#x20;     \</div>

&#x20;     \<div class="bg-gray-950/90 rounded-lg p-6 border border-gray-800/80 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 font-mono">
&#x20;       \<div>
&#x20;         \<span class="text-[10px] text-emerald-400 tracking-widest block mb-1">
&#x20;           OFFICIAL SIGNATURE / 官方授權簽署紀錄
&#x20;         \</span>
&#x20;         \<p class="text-[11px] text-gray-500 font-sans">
&#x20;           茲證明本檔案已完成代理化程序，本體與裝置之生物權限正式移交。
&#x20;         \</p>
&#x20;       \</div>

&#x20;       \<div class="flex items-center justify-center min-w-[160px] h-[120px] relative overflow-hidden shrink-0">
&#x20;         \<img id="detail-signature-img" src="" alt="Principal Signature" class="max-h-28 max-w-[180px] object-contain opacity-95 mix-blend-screen hidden">
&#x20;         \<span id="detail-signature-placeholder" class="text-[10px] text-gray-600 tracking-widest">
&#x20;           PENDING SEAL
&#x20;         \</span>
&#x20;       \</div>
&#x20;     \</div>

&#x20;   \</article>

&#x20; \</main>

&#x20; \<!-- 作品理念視窗 -->

&#x20; \<div
&#x20;   id="concept-modal"
&#x20;   class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/85 backdrop-blur-md modal-backdrop hidden-modal"
&#x20; \>

&#x20;   \<div class="bg-[#0b0d10] max-w-2xl w-full max-h-[85vh] rounded-xl border-0 p-6 md:p-8 overflow-y-auto space-y-6 text-sm relative shadow-2xl modal-content">

&#x20;     \<div class="border-b border-gray-800/60 pb-4">

&#x20;       \<span class="text-[10px] font-mono text-emerald-500 tracking-widest block">
&#x20;         CURATORIAL STATEMENT
&#x20;       \</span>

&#x20;       \<h2 class="text-xl font-bold text-white font-mono">
&#x20;         《理想狀態》作品理念
&#x20;       \</h2>

&#x20;     \</div>

&#x20;     \<div class="space-y-5 text-gray-300 font-sans leading-relaxed text-xs md:text-sm">

&#x20;       \<section class="space-y-1.5">

&#x20;         \<h3 class="text-emerald-500 font-mono font-bold text-xs">
&#x20;           01\. 假期的罪惡感
&#x20;         \</h3>

&#x20;         \<p class="text-gray-400">
&#x20;           在這個高度追求效率與生產力的當代社會中，「空閒」逐漸轉化為一種罪惡感的來源。這件作品源自於我在假期中的一段空閒時間，當時我意識到，在這段毫無自我價值提升的時光裡，我的內心竟不斷萌生出一種「時間正被浪費」的焦慮與罪惡感。這樣的狀態迫使我開始思考：「我們是不是習慣用產出與結果，來證明一個人的價值？」「如果一個人做某件事卻沒有產出任何價值，它的存在是否仍值得被承認？」
&#x20;         \</p>

&#x20;       \</section>

&#x20;       \<section class="space-y-1.5">

&#x20;         \<h3 class="text-emerald-500 font-mono font-bold text-xs">
&#x20;           02\. 生產一件代替我停止生產的作品
&#x20;         \</h3>

&#x20;         \<p class="text-gray-400">
&#x20;           對我而言，創作原本是一種創造價值與實現自我的方式。然而在過往的經驗中，當我沒有持續創作或完成任何事情時，便極易陷入生產力恐慌。為了進一步探討這種焦慮，我邀請了另外三位與我有類似經驗的人共同參與這件作品：既然創作對我來說是一種創造價值的行為，那麼，我們是否能「生產一件代替我們休息、只是在呼吸而沒有實際產出的作品」？我試圖透過這樣的矛盾，讓作品暫時代替我們完成一次真正的「休息」。它不需要完成任何事情，也不以產出任何結果為目的，只是不斷地呼吸，將原本被我視為缺乏價值的空白時間，轉化為作品本身的一部分。
&#x20;         \</p>

&#x20;       \</section>

&#x20;       \<section class="space-y-1.5">

&#x20;         \<h3 class="text-emerald-500 font-mono font-bold text-xs">
&#x20;           03\. 邀請與裝置代理
&#x20;         \</h3>

&#x20;         \<p class="text-gray-400">
&#x20;           我邀請另外三位與我有相同經驗感受的人進入本次創作，作品由四件臉部雕塑組成，並將我與參與者的個人特徵（頭髮）剝離，僅留存五官結構。為了將「呼吸」視覺化，我採用氣動裝置搭配塑膠薄膜包裹雕塑，利用裝置抽氣與放氣的特性，模擬人頭套著塑膠袋呼吸時的膨脹與收縮，進而製造雕塑正在呼吸的錯覺。藉由這次的邀請參與，作品進一步延伸出四組系列檔案。這些檔案具體記錄了：本體與代理裝置的視覺對比、個案焦慮指數、自出生起算之生命累積總呼吸數估算，以及各自的「本體焦慮自白紀錄」，構成一份份完整的個人焦慮檔案與系統診斷報告。
&#x20;         \</p>

&#x20;       \</section>

&#x20;     \</div>

&#x20;     \<div
&#x20;       class="concept-modal-footer border-t border-gray-800/60 pt-4"
&#x20;       style="display:flex !important;justify-content:center !important;align-items:center !important;width:100% !important;"
&#x20;     \>

&#x20;       \<button
&#x20;         id="concept-close-button"
&#x20;         type="button"
&#x20;         onclick="closeConceptModal()"
&#x20;         class="text-xs font-mono shadow-sm transition-all"
&#x20;         style="background-color:#ffffff !important;color:#000000 !important;-webkit-text-fill-color:#000000 !important;border:1px solid #ffffff !important;margin-left:auto !important;margin-right:auto !important;"
&#x20;       \>
&#x20;         關閉視窗 (CLOSE)
&#x20;       \</button>

&#x20;     \</div>

&#x20;   \</div>

&#x20; \</div>

&#x20; \<footer class="border-t border-gray-800/60 mt-16 pt-6 text-center md:flex justify-between items-center text-xs text-gray-600 font-mono">

&#x20;   \<p>© SHAOQI SU. ALL RIGHTS RESERVED.\</p>

&#x20;   \<p class="mt-2 md:mt-0">
&#x20;     ARTWORK BY SHAOQI SU
&#x20;   \</p>

&#x20; \</footer>

\</div>

\<script>
/\* Firebase Realtime Database 跨裝置即時同步設定 \*/
const FIREBASE_DB_URL = "[https://ideal-state-default-rtdb.asia-southeast1.firebasedatabase.app](https://ideal-state-default-rtdb.asia-southeast1.firebasedatabase.app)";
const FIREBASE_CASES_ENDPOINT = \`${FIREBASE_DB_URL}/visitor_cases.json\`;

/\* 原始四位個案資料 \*/
const BASE_PARTICIPANTS = [
&#x20; {
&#x20;   id: "01",
&#x20;   name: "蘇Ｏ齊",
&#x20;   role: "代理呼吸員 01 號",
&#x20;   target: "蘇Ｏ齊 (本體)",
&#x20;   anxiety: "87.4% (極高)",
&#x20;   birthDate: "2005-02-26",
&#x20;   imageUrl: "[https://cdn.phototourl.com/member/2026-10-25-96d3b302-64c7-4d0a-92c3-dae869f734ea.jpg](https://cdn.phototourl.com/member/2026-10-25-96d3b302-64c7-4d0a-92c3-dae869f734ea.jpg)",
&#x20;   deviceImageUrl: "[https://cdn.phototourl.com/member/2026-10-27-116824ad-2295-48ab-bfc1-92ff1656d47d.jpg](https://cdn.phototourl.com/member/2026-10-27-116824ad-2295-48ab-bfc1-92ff1656d47d.jpg)",
&#x20;   signatureUrl: "[https://cdn.phototourl.com/member/2026-11-12-7dcaeab3-e243-42a6-b786-45621df6d49b.png](https://cdn.phototourl.com/member/2026-11-12-7dcaeab3-e243-42a6-b786-45621df6d49b.png)",
&#x20;   statement: "我發現自己根本無法忍受純粹的放空。只要一停下來，那種『時間正被浪費』的罪惡感就會把我吞噬。我做這個替身，不是為了讓自己舒服地去休息，而是因為我停不下來——我需要一個實體掛在那裡，替我承擔什麼都不做、只剩下呼吸的死寂。"
&#x20; },
&#x20; {
&#x20;   id: "02",
&#x20;   name: "張Ｏ禎",
&#x20;   role: "代理呼吸員 02 號",
&#x20;   target: "張Ｏ禎 (本體)",
&#x20;   anxiety: "91.2% (極高)",
&#x20;   birthDate: "2005-07-10",
&#x20;   imageUrl: "[https://cdn.phototourl.com/member/2026-10-25-09d06678-a47a-49da-b2bb-c6197ce8a8ea.jpg](https://cdn.phototourl.com/member/2026-10-25-09d06678-a47a-49da-b2bb-c6197ce8a8ea.jpg)",
&#x20;   deviceImageUrl: "[https://cdn.phototourl.com/member/2026-11-12-7e79f8bf-8331-4a21-bd47-52142a9de442.webp](https://cdn.phototourl.com/member/2026-11-12-7e79f8bf-8331-4a21-bd47-52142a9de442.webp)",
&#x20;   signatureUrl: "[https://cdn.phototourl.com/member/2026-11-17-5d5e831c-b031-4cda-b5ec-45c043ea4136.jpg](https://cdn.phototourl.com/member/2026-11-17-5d5e831c-b031-4cda-b5ec-45c043ea4136.jpg)",
&#x20;   statement: "只要超過半天沒有打開繪圖軟體或思考進度，我就會產生一種即將被同齡人徹底淘汰的恐慌。明明身體已經累到極限，但大腦還是強迫性地尋找事情做。我需要這台機器幫我把『休息』當成一項公事來認真執行。"
&#x20; },
&#x20; {
&#x20;   id: "03",
&#x20;   name: "林Ｏ岑",
&#x20;   role: "代理呼吸員 03 號",
&#x20;   target: "林Ｏ岑 (本體)",
&#x20;   anxiety: "79.8% (高)",
&#x20;   birthDate: "2005-06-09",
&#x20;   imageUrl: "[https://cdn.phototourl.com/member/2026-10-25-b70cd799-0bbf-4737-a890-d4c0962fe741.jpg](https://cdn.phototourl.com/member/2026-10-25-b70cd799-0bbf-4737-a890-d4c0962fe741.jpg)",
&#x20;   deviceImageUrl: "[https://cdn.phototourl.com/member/2026-10-27-4e230bf1-0d8a-429f-b28d-9e4734ace26d.jpg](https://cdn.phototourl.com/member/2026-10-27-4e230bf1-0d8a-429f-b28d-9e4734ace26d.jpg)",
&#x20;   signatureUrl: "",
&#x20;   statement: "休假對我來說不是放鬆，而是一場審判。躺在床上滑手機的每一分鐘，都在盤算著自己少做了多少事。如果一定要有人在那裡放空，那就讓這顆塑膠薄膜包覆的人頭替我坐牢吧。"
&#x20; },
&#x20; {
&#x20;   id: "04",
&#x20;   name: "陳Ｏ騏",
&#x20;   role: "代理呼吸員 04 號",
&#x20;   target: "陳Ｏ騏 (本體)",
&#x20;   anxiety: "20.0%\~101.0% ",
&#x20;   birthDate: "2005-01-13",
&#x20;   imageUrl: "[https://cdn.phototourl.com/member/2026-11-12-7b3841fa-5cb3-4af4-a31d-fefcb0d22eb6.jpg](https://cdn.phototourl.com/member/2026-11-12-7b3841fa-5cb3-4af4-a31d-fefcb0d22eb6.jpg)",
&#x20;   deviceImageUrl: "[https://cdn.phototourl.com/member/2026-11-12-8b5daa41-c18f-4551-8ce9-0cd83800cb70.jpg](https://cdn.phototourl.com/member/2026-11-12-8b5daa41-c18f-4551-8ce9-0cd83800cb70.jpg)",
&#x20;   signatureUrl: "[https://cdn.phototourl.com/member/2026-11-12-138664ec-ffa6-434b-9a84-e752704cabef.png](https://cdn.phototourl.com/member/2026-11-12-138664ec-ffa6-434b-9a84-e752704cabef.png)",
&#x20;   statement: "檔案建立中，等待資料輸入..."
&#x20; }
];

const VISITOR_STORAGE_KEY = "IDEAL_STATE_VISITOR_CASES_2026";
const DEVICE_TOKEN_KEY = "IDEAL_STATE_DEVICE_TOKEN_2026";
const EDIT_WINDOW_MS = 3 \* 60 \* 1000;

function hasFirebaseConfigured() {
&#x20; return Boolean(
&#x20;   FIREBASE_DB_URL &&
&#x20;   FIREBASE_DB_URL.startsWith("https\://")
&#x20; );
}

function getDeviceToken() {
&#x20; try {
&#x20;   let token = localStorage.getItem(DEVICE_TOKEN_KEY);
&#x20;   if (!token) {
&#x20;     token = "DEV-" + Date.now().toString(36) + "-" + Math.random().toString(36).slice(2, 10);
&#x20;     localStorage.setItem(DEVICE_TOKEN_KEY, token);
&#x20;   }
&#x20;   return token;
&#x20; } catch (err) {
&#x20;   return "DEV-SESSION-TEMP";
&#x20; }
}

const CURRENT_DEVICE_TOKEN = getDeviceToken();

/\* 影像狀態變數 \*/
let rawCutoutBaseDataUrl = "";
let capturedPhotoDataUrl = "";
let isPrivacyBlurEnabled = false;
let editingCaseId = null;
let currentDetailCaseId = null;
let lastRenderedSignature = "";

let VISITOR_PARTICIPANTS = loadLocalVisitorCases();
let breathTimer = null;

function formatImageSrc(url) {
&#x20; if (!url || typeof url !== "string") return "";
&#x20; if (url.startsWith("data:image/")) {
&#x20;   if (url.length < 120 || !url.includes(",")) return "";
&#x20;   return url;
&#x20; }
&#x20; if (url.startsWith("http\://") || url.startsWith("https\://")) {
&#x20;   return \`${url}?v=${Date.now()}\`;
&#x20; }
&#x20; return "";
}

function resolveVisitorPhotoSrc(p) {
&#x20; if (!p) return "";
&#x20; return formatImageSrc(p.imageUrl || p.deviceImageUrl || p.rawImageUrl || "");
}

function loadLocalVisitorCases() {
&#x20; try {
&#x20;   const raw = localStorage.getItem(VISITOR_STORAGE_KEY);
&#x20;   if (!raw) return [];
&#x20;   const parsed = JSON.parse(raw);
&#x20;   return Array.isArray(parsed) ? parsed : [];
&#x20; } catch (err) {
&#x20;   return [];
&#x20; }
}

function saveLocalVisitorCases(cases) {
&#x20; try {
&#x20;   localStorage.setItem(VISITOR_STORAGE_KEY, JSON.stringify(cases));
&#x20; } catch (err) {
&#x20;   // 若容量不足則略過本機快取，以雲端為主
&#x20; }
}

/\* 從 Firebase 雲端讀取所有觀眾病例 \*/
async function fetchCloudVisitorCases() {
&#x20; if (!hasFirebaseConfigured()) {
&#x20;   return loadLocalVisitorCases();
&#x20; }

&#x20; try {
&#x20;   const res = await fetch(\`${FIREBASE_CASES_ENDPOINT}?t=${Date.now()}\`, {
&#x20;     cache: "no-store"
&#x20;   });
&#x20;   if (!res.ok) throw new Error("Firebase fetch failed");
&#x20;   const data = await res.json();

&#x20;   if (!data) return [];

&#x20;   let list = [];
&#x20;   if (Array.isArray(data)) {
&#x20;     list = data.filter(Boolean);
&#x20;   } else if (typeof data === "object") {
&#x20;     list = Object.values(data).filter(Boolean);
&#x20;   }

&#x20;   list.sort((a, b) => {
&#x20;     const na = parseInt(a.id, 10) || 0;
&#x20;     const nb = parseInt(b.id, 10) || 0;
&#x20;     return na - nb;
&#x20;   });

&#x20;   return list;
&#x20; } catch (err) {
&#x20;   return loadLocalVisitorCases();
&#x20; }
}

/\* 將所有觀眾病例同步寫入 Firebase 雲端 \*/
async function syncVisitorCasesToCloud(cases) {
&#x20; saveLocalVisitorCases(cases);

&#x20; if (!hasFirebaseConfigured()) {
&#x20;   return { ok: false, reason: "NOT_CONFIGURED" };
&#x20; }

&#x20; try {
&#x20;   const payload = cases.length === 0 ? null : cases;
&#x20;   const res = await fetch(FIREBASE_CASES_ENDPOINT, {
&#x20;     method: "PUT",
&#x20;     headers: { "Content-Type": "application/json" },
&#x20;     body: JSON.stringify(payload)
&#x20;   });
&#x20;   if (!res.ok) throw new Error("Firebase PUT failed");
&#x20;   return { ok: true };
&#x20; } catch (err) {
&#x20;   return { ok: false, reason: "NETWORK_ERROR" };
&#x20; }
}

function getAllParticipants() {
&#x20; return [...BASE_PARTICIPANTS, ...VISITOR_PARTICIPANTS];
}

function isVisitorCaseById(id) {
&#x20; return VISITOR_PARTICIPANTS.some(p => p.id === id);
}

function canEditOrDeleteCase(caseItem) {
&#x20; if (!caseItem || !caseItem.ownerToken || !caseItem.createdAt) return false;
&#x20; if (caseItem.ownerToken !== CURRENT_DEVICE_TOKEN) return false;
&#x20; const elapsed = Date.now() - Number(caseItem.createdAt);
&#x20; return elapsed >= 0 && elapsed < EDIT_WINDOW_MS;
}

function getRemainingEditSeconds(caseItem) {
&#x20; if (!canEditOrDeleteCase(caseItem)) return 0;
&#x20; const remainingMs = EDIT_WINDOW_MS - (Date.now() - Number(caseItem.createdAt));
&#x20; return Math.max(0, Math.ceil(remainingMs / 1000));
}

function formatSecondsMMSS(sec) {
&#x20; const m = String(Math.floor(sec / 60)).padStart(2, '0');
&#x20; const s = String(sec % 60).padStart(2, '0');
&#x20; return \`${m}:${s}\`;
}

function updateCaseCountersUI() {
&#x20; const visitorTotalEl = document.getElementById('visitor-total-label');
&#x20; const nextIdEl = document.getElementById('next-case-preview');
&#x20; const visitorCountStr = String(VISITOR_PARTICIPANTS.length).padStart(2, '0');

&#x20; if (visitorTotalEl) {
&#x20;   visitorTotalEl.innerText = \`VISITOR CASES: ${visitorCountStr}\`;
&#x20; }

&#x20; if (nextIdEl) {
&#x20;   if (editingCaseId) {
&#x20;     nextIdEl.innerText = \`EDITING: CASE #${editingCaseId}\`;
&#x20;   } else {
&#x20;     const maxNum = VISITOR_PARTICIPANTS.reduce((max, item) => {
&#x20;       const n = parseInt(item.id, 10);
&#x20;       return !isNaN(n) && n > max ? n : max;
&#x20;     }, BASE_PARTICIPANTS.length);
&#x20;     const nextIdStr = String(maxNum + 1).padStart(2, '0');
&#x20;     nextIdEl.innerText = \`NEXT ID: CASE #${nextIdStr}\`;
&#x20;   }
&#x20; }
}

/\* 作品理念 \*/
function openConceptModal() {
&#x20; const modal = document.getElementById('concept-modal');
&#x20; modal.classList.remove('hidden-modal');
&#x20; modal.classList.add('active-modal');
}

function closeConceptModal() {
&#x20; const modal = document.getElementById('concept-modal');
&#x20; modal.classList.remove('active-modal');
&#x20; modal.classList.add('hidden-modal');
}

document.getElementById('concept-modal').addEventListener('click', function(e) {
&#x20; if (e.target === this) {
&#x20;   closeConceptModal();
&#x20; }
});

/\* 產出數值 \*/
function startOutputFluctuationWithPause() {
&#x20; const headerOutputEl = document.getElementById('header-output-counter');
&#x20; const detailOutputEl = document.getElementById('detail-output-counter');

&#x20; let isPaused = false;

&#x20; function updateCounter() {
&#x20;   if (isPaused) return;

&#x20;   if (Math.random() < 0.15) {
&#x20;     isPaused = true;

&#x20;     if (headerOutputEl) {
&#x20;       headerOutputEl.innerText = "0.000 KG/H";
&#x20;     }

&#x20;     if (detailOutputEl) {
&#x20;       detailOutputEl.innerText = "0.000 KG/H (NULL)";
&#x20;     }

&#x20;     setTimeout(() => {
&#x20;       isPaused = false;
&#x20;     }, 500);

&#x20;   } else {
&#x20;     const d1 = Math.floor(Math.random() \* 10);
&#x20;     const d2 = Math.floor(Math.random() \* 10);
&#x20;     const d3 = Math.floor(Math.random() \* 10);
&#x20;     const d4 = Math.floor(Math.random() \* 10);

&#x20;     const randomVal = \`${d1}.${d2}${d3}${d4}\`;

&#x20;     if (headerOutputEl) {
&#x20;       headerOutputEl.innerText = \`${randomVal} KG/H\`;
&#x20;     }

&#x20;     if (detailOutputEl) {
&#x20;       detailOutputEl.innerText = \`${randomVal} KG/H (NULL)\`;
&#x20;     }
&#x20;   }
&#x20; }

&#x20; setInterval(updateCounter, 80);
}

/\* 呼吸計數 \*/
function calculateLifetimeBreaths(birthDateStr) {
&#x20; const birthDate = new Date(birthDateStr);
&#x20; const now = new Date();
&#x20; const diffInSeconds = (now - birthDate) / 1000;
&#x20; return Math.max(0, Math.floor(diffInSeconds \* 0.25));
}

function startBreathCounter(birthDateStr) {
&#x20; if (breathTimer) {
&#x20;   clearInterval(breathTimer);
&#x20; }

&#x20; let currentBreaths = calculateLifetimeBreaths(birthDateStr);
&#x20; const counterEl = document.getElementById('detail-breath-counter');
&#x20; counterEl.innerText = currentBreaths.toLocaleString();

&#x20; breathTimer = setInterval(() => {
&#x20;   currentBreaths += 1;
&#x20;   counterEl.innerText = currentBreaths.toLocaleString();
&#x20; }, 4000);
}

/\* 原始四位委託人圖片對比滑桿 \*/
function updateImgDimensions() {
&#x20; const sliderContainer = document.getElementById('detail-slider-container');
&#x20; const principalImg = document.getElementById('detail-principal-img');

&#x20; if (sliderContainer && principalImg) {
&#x20;   principalImg.style.width = \`${sliderContainer.clientWidth}px\`;
&#x20;   principalImg.style.height = \`${sliderContainer.clientHeight}px\`;
&#x20; }
}

function initComparisonSlider() {
&#x20; const slider = document.getElementById('comparison-slider');
&#x20; const clipWrapper = document.getElementById('slider-clip-wrapper');
&#x20; const sliderLine = document.getElementById('slider-line');
&#x20; const badgePrincipal = document.getElementById('badge-principal');
&#x20; const badgeDevice = document.getElementById('badge-device');

&#x20; const badgeBase =
&#x20;   "absolute bottom-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none";

&#x20; function updateSlider(val) {
&#x20;   clipWrapper.style.width = \`${val}%\`;
&#x20;   sliderLine.style.left = \`${val}%\`;

&#x20;   const principalActive = val > 55;
&#x20;   const deviceActive = val < 45;

&#x20;   badgePrincipal.className =
&#x20;     \`${badgeBase} left-2.5 \` +
&#x20;     (principalActive ? "badge-emerald-active" : "badge-inactive");

&#x20;   badgeDevice.className =
&#x20;     \`${badgeBase} right-2.5 \` +
&#x20;     (deviceActive ? "badge-emerald-active" : "badge-inactive");
&#x20; }

&#x20; slider.addEventListener('input', e => {
&#x20;   updateSlider(e.target.value);
&#x20; });

&#x20; window\.addEventListener('resize', updateImgDimensions);
&#x20; updateImgDimensions();
}

/\* 切換詳情頁左側顯示模式：觀眾病例隱藏滑桿並顯示獨立觀眾照片，原始四位還原滑桿對比 \*/
function applyDetailLeftMode(isVisitorCase) {
&#x20; const visitorImg = document.getElementById('detail-visitor-img');
&#x20; const clipWrapper = document.getElementById('slider-clip-wrapper');
&#x20; const principalImg = document.getElementById('detail-principal-img');
&#x20; const deviceImg = document.getElementById('detail-device-img');
&#x20; const sliderInput = document.getElementById('comparison-slider');
&#x20; const sliderLine = document.getElementById('slider-line');
&#x20; const badgePrincipal = document.getElementById('badge-principal');
&#x20; const badgeDevice = document.getElementById('badge-device');

&#x20; if (isVisitorCase) {
&#x20;   if (clipWrapper) clipWrapper.style.display = 'none';
&#x20;   if (deviceImg) deviceImg.style.display = 'none';
&#x20;   if (sliderInput) sliderInput.style.display = 'none';
&#x20;   if (sliderLine) sliderLine.style.display = 'none';
&#x20;   if (badgePrincipal) badgePrincipal.style.display = 'none';
&#x20;   if (badgeDevice) badgeDevice.style.display = 'none';
&#x20; } else {
&#x20;   if (visitorImg) {
&#x20;     visitorImg.src = '';
&#x20;     visitorImg.classList.add('hidden');
&#x20;   }
&#x20;   if (clipWrapper) {
&#x20;     clipWrapper.style.display = '';
&#x20;     clipWrapper.style.width = '50%';
&#x20;   }
&#x20;   if (deviceImg) {
&#x20;     deviceImg.style.display = '';
&#x20;   }
&#x20;   if (sliderInput) {
&#x20;     sliderInput.style.display = '';
&#x20;     sliderInput.value = 50;
&#x20;   }
&#x20;   if (sliderLine) {
&#x20;     sliderLine.style.display = '';
&#x20;     sliderLine.style.left = '50%';
&#x20;   }
&#x20;   if (badgePrincipal) {
&#x20;     badgePrincipal.style.display = '';
&#x20;     badgePrincipal.className =
&#x20;       "absolute bottom-2.5 left-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none badge-inactive";
&#x20;   }
&#x20;   if (badgeDevice) {
&#x20;     badgeDevice.style.display = '';
&#x20;     badgeDevice.className =
&#x20;       "absolute bottom-2.5 right-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none badge-inactive";
&#x20;   }
&#x20;   if (principalImg) {
&#x20;     principalImg.style.maxWidth = 'none';
&#x20;   }
&#x20;   updateImgDimensions();
&#x20; }
}

/\* 單張卡片模板 \*/
function buildCaseCardHTML(p, isVisitor = false) {
&#x20; const imgSrc = isVisitor ? resolveVisitorPhotoSrc(p) : formatImageSrc(p.imageUrl);
&#x20; const deviceImgSrc = formatImageSrc(p.deviceImageUrl || p.imageUrl);
&#x20; const editable = isVisitor && canEditOrDeleteCase(p);

&#x20; return \`
&#x20;   \<div
&#x20;     onclick="showDetail('${p.id}')"
&#x20;     class="glass-card rounded-xl overflow-hidden cursor-pointer flex flex-col justify-between relative group"
&#x20;   \>

&#x20;     \<div class="aspect-[3/4] w-full bg-gray-950 relative overflow-hidden flex items-center justify-center">

&#x20;       \<div class="absolute top-2.5 left-2.5 sm:top-3 sm:left-3 pointer-events-none z-30">
&#x20;         \<span class="text-[10px] sm:text-xs font-mono text-white tracking-wider font-bold bg-black/70 backdrop-blur-md px-2 sm:px-2.5 py-0.5 sm:py-1 rounded shadow-lg inline-block transition-all duration-300 group-hover:bg-transparent group-hover:backdrop-blur-none group-hover:shadow-none mobile-auto-bg-fade">
&#x20;           CASE #${p.id}
&#x20;         \</span>
&#x20;       \</div>

&#x20;       ${
&#x20;         imgSrc
&#x20;           ? \`
&#x20;             \<img
&#x20;               src="${imgSrc}"
&#x20;               alt="${p.name}"
&#x20;               onerror="this.style.display='none'"
&#x20;               class="w-full h-full object-cover group-hover:scale-105 transition-all duration-500 absolute inset-0 z-0"
&#x20;             \>

&#x20;             ${
&#x20;               !isVisitor && deviceImgSrc
&#x20;                 ? \`
&#x20;                   \<img
&#x20;                     src="${deviceImgSrc}"
&#x20;                     alt="Proxy Device"
&#x20;                     onerror="this.style.display='none'"
&#x20;                     class="w-full h-full object-cover opacity-0 group-hover:opacity-100 mobile-auto-fade group-hover:scale-105 transition-all duration-500 absolute inset-0 z-10"
&#x20;                   \>
&#x20;                 \`
&#x20;                 : ''
&#x20;             }
&#x20;           \`
&#x20;           : \`
&#x20;             \<div class="flex flex-col items-center space-y-2 text-gray-700">

&#x20;               \<svg
&#x20;                 class="w-10 h-10 sm:w-12 sm:h-12 stroke-gray-700"
&#x20;                 fill="none"
&#x20;                 viewBox="0 0 24 24"
&#x20;                 stroke="currentColor"
&#x20;               \>
&#x20;                 \<path
&#x20;                   stroke-linecap="round"
&#x20;                   stroke-linejoin="round"
&#x20;                   stroke-width="1.5"
&#x20;                   d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
&#x20;                 />
&#x20;               \</svg>

&#x20;               \<span class="text-[10px] font-mono text-gray-600">
&#x20;                 NO SCAN DATA
&#x20;               \</span>

&#x20;             \</div>
&#x20;           \`
&#x20;       }

&#x20;       \<div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/30 to-transparent pointer-events-none z-20">\</div>

&#x20;       \<div class="absolute ${editable ? 'bottom-[74px] sm:bottom-[82px]' : 'bottom-12 sm:bottom-14'} left-3 right-3 sm:left-4 sm:right-4 space-y-0.5 pointer-events-none z-30 transition-all duration-300">

&#x20;         \<h3 class="text-sm sm:text-lg font-bold text-white tracking-wide truncate">
&#x20;           ${p.name}
&#x20;         \</h3>

&#x20;         \<p class="text-[10px] sm:text-xs text-gray-300 font-mono truncate">
&#x20;           ${p.role}
&#x20;         \</p>

&#x20;       \</div>

&#x20;       \<div class="absolute bottom-2.5 left-2.5 right-2.5 sm:bottom-3 sm:left-3 sm:right-3 z-30 flex flex-col gap-1.5">

&#x20;         ${
&#x20;           editable
&#x20;             ? \`
&#x20;               \<div class="grid grid-cols-2 gap-1.5 w-full" onclick="event.stopPropagation()">
&#x20;                 \<button
&#x20;                   type="button"
&#x20;                   onclick="startEditVisitorCase('${p.id}')"
&#x20;                   class="w-full py-1 sm:py-1.5 rounded bg-white/95 text-black font-mono text-[10px] sm:text-xs font-bold hover:bg-gray-200 transition-colors shadow text-center"
&#x20;                 \>
&#x20;                   ✎ 修改
&#x20;                 \</button>
&#x20;                 \<button
&#x20;                   type="button"
&#x20;                   onclick="deleteVisitorCase('${p.id}')"
&#x20;                   class="w-full py-1 sm:py-1.5 rounded bg-red-950/90 text-red-400 border border-red-700/70 font-mono text-[10px] sm:text-xs font-bold hover:bg-red-800 hover:text-white transition-colors shadow text-center"
&#x20;                 \>
&#x20;                   ✕ 刪除
&#x20;                 \</button>
&#x20;               \</div>
&#x20;             \`
&#x20;             : ''
&#x20;         }

&#x20;         \<button class="btn-neon w-full py-1.5 sm:py-2 text-[10px] sm:text-xs rounded shadow-lg flex items-center justify-center space-x-1">
&#x20;           \<span>檢視個案診斷\</span>
&#x20;           \<span>→\</span>
&#x20;         \</button>

&#x20;       \</div>

&#x20;     \</div>

&#x20;   \</div>
&#x20; \`;
}

/\* 渲染原始四位卡片與現場觀眾獨立卡片 \*/
function renderPortalCards(force = false) {
&#x20; const sig = VISITOR_PARTICIPANTS.map(p => \`${p.id}|${p.name}|${p.statement}|${Boolean(resolveVisitorPhotoSrc(p))}\`).join("||");
&#x20; if (!force && sig === lastRenderedSignature) {
&#x20;   updateCaseCountersUI();
&#x20;   return;
&#x20; }
&#x20; lastRenderedSignature = sig;

&#x20; const baseContainer = document.getElementById('cards-container');
&#x20; const visitorContainer = document.getElementById('visitor-cards-container');

&#x20; if (baseContainer) {
&#x20;   baseContainer.innerHTML = BASE_PARTICIPANTS.map(p => buildCaseCardHTML(p, false)).join('');
&#x20; }

&#x20; if (visitorContainer) {
&#x20;   if (VISITOR_PARTICIPANTS.length === 0) {
&#x20;     visitorContainer.innerHTML = \`
&#x20;       \<div class="col-span-full bg-gray-950/60 border border-dashed border-gray-800 rounded-xl p-8 text-center font-mono space-y-1.5">
&#x20;         \<span class="text-xs text-gray-400 block">
&#x20;           [STANDBY] 尚未收錄現場觀眾病例檔案
&#x20;         \</span>
&#x20;         \<span class="text-[11px] text-gray-600 font-sans block">
&#x20;           請於下方「書寫你的病例檔案」登錄你的焦慮自白，建立後將跨裝置同步顯示於此
&#x20;         \</span>
&#x20;       \</div>
&#x20;     \`;
&#x20;   } else {
&#x20;     visitorContainer.innerHTML = VISITOR_PARTICIPANTS.map(p => buildCaseCardHTML(p, true)).join('');
&#x20;   }
&#x20; }

&#x20; updateCaseCountersUI();
}

/\* 人像去背 + 一般黑白灰階 + 適度曝光微調 \*/
let selfieSegmenterInstance = null;

function getSelfieSegmenter() {
&#x20; if (selfieSegmenterInstance) return selfieSegmenterInstance;
&#x20; if (typeof SelfieSegmentation === "undefined") return null;

&#x20; try {
&#x20;   const segmenter = new SelfieSegmentation({
&#x20;     locateFile: (file) => \`[https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation/${file](https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation/${file)}\`
&#x20;   });
&#x20;   segmenter.setOptions({
&#x20;     modelSelection: 1,
&#x20;     selfieMode: false
&#x20;   });
&#x20;   selfieSegmenterInstance = segmenter;
&#x20;   return segmenter;
&#x20; } catch (e) {
&#x20;   return null;
&#x20; }
}

function applyClinicalMonochromeTone(ctx, width, height, alphaMaskData = null) {
&#x20; const imgData = ctx.getImageData(0, 0, width, height);
&#x20; const data = imgData.data;

&#x20; const exposureGain = 1.18;
&#x20; const contrastFactor = 1.08;

&#x20; for (let i = 0; i < data.length; i += 4) {
&#x20;   const fgAlpha = alphaMaskData ? alphaMaskData[i + 3] / 255 : 1;

&#x20;   if (fgAlpha < 0.08) {
&#x20;     data[i] = 8;
&#x20;     data[i + 1] = 9;
&#x20;     data[i + 2] = 10;
&#x20;     continue;
&#x20;   }

&#x20;   const r = data[i];
&#x20;   const g = data[i + 1];
&#x20;   const b = data[i + 2];

&#x20;   let lum = (0.299 \* r + 0.587 \* g + 0.114 \* b) / 255;
&#x20;   lum = lum \* exposureGain;
&#x20;   lum = (lum - 0.5) \* contrastFactor + 0.5;
&#x20;   lum = Math.max(0, Math.min(1, lum));

&#x20;   const edgeBlend = Math.min(1, Math.max(0, (fgAlpha - 0.08) / 0.55));
&#x20;   const finalLum = lum \* edgeBlend + (8 / 255) \* (1 - edgeBlend);

&#x20;   const val = Math.round(finalLum \* 255);
&#x20;   data[i] = val;
&#x20;   data[i + 1] = val;
&#x20;   data[i + 2] = val;
&#x20; }

&#x20; ctx.putImageData(imgData, 0, 0);
}

function fallbackBackgroundCutout(img, width, height) {
&#x20; const canvas = document.createElement('canvas');
&#x20; canvas.width = width;
&#x20; canvas.height = height;
&#x20; const ctx = canvas.getContext('2d');
&#x20; ctx.drawImage(img, 0, 0, width, height);

&#x20; const imgData = ctx.getImageData(0, 0, width, height);
&#x20; const data = imgData.data;

&#x20; const maskCanvas = document.createElement('canvas');
&#x20; maskCanvas.width = width;
&#x20; maskCanvas.height = height;
&#x20; const maskCtx = maskCanvas.getContext('2d');
&#x20; const maskImgData = maskCtx.createImageData(width, height);
&#x20; const mData = maskImgData.data;

&#x20; const corners = [
&#x20;   0,
&#x20;   (width - 1) \* 4,
&#x20;   (Math.floor(height \* 0.15) \* width) \* 4,
&#x20;   (Math.floor(height \* 0.15) \* width + width - 1) \* 4
&#x20; ];

&#x20; let bgR = 0, bgG = 0, bgB = 0;
&#x20; corners.forEach(idx => {
&#x20;   bgR += data[idx];
&#x20;   bgG += data[idx + 1];
&#x20;   bgB += data[idx + 2];
&#x20; });
&#x20; bgR /= corners.length;
&#x20; bgG /= corners.length;
&#x20; bgB /= corners.length;

&#x20; const cx = width \* 0.5;
&#x20; const cy = height \* 0.48;

&#x20; for (let y = 0; y < height; y++) {
&#x20;   for (let x = 0; x < width; x++) {
&#x20;     const idx = (y \* width + x) \* 4;
&#x20;     const dr = data[idx] - bgR;
&#x20;     const dg = data[idx + 1] - bgG;
&#x20;     const db = data[idx + 2] - bgB;
&#x20;     const colorDist = Math.sqrt(dr \* dr + dg \* dg + db \* db);

&#x20;     const nx = (x - cx) / (width \* 0.42);
&#x20;     const ny = (y - cy) / (height \* 0.50);
&#x20;     const radialDist = Math.sqrt(nx \* nx + ny \* ny);

&#x20;     let alpha = 255;
&#x20;     if (colorDist < 42 && radialDist > 0.60) {
&#x20;       alpha = 0;
&#x20;     } else if (radialDist > 0.94) {
&#x20;       alpha = Math.max(0, Math.round(255 \* (1 - (radialDist - 0.94) \* 4)));
&#x20;     }

&#x20;     mData[idx] = 255;
&#x20;     mData[idx + 1] = 255;
&#x20;     mData[idx + 2] = 255;
&#x20;     mData[idx + 3] = alpha;
&#x20;   }
&#x20; }

&#x20; applyClinicalMonochromeTone(ctx, width, height, mData);
&#x20; return canvas.toDataURL('image/jpeg', 0.78);
}

function processPortraitCutoutAndTone(file) {
&#x20; return new Promise((resolve) => {
&#x20;   const reader = new FileReader();
&#x20;   reader.onload = function(event) {
&#x20;     const img = new Image();
&#x20;     img.onload = async function() {
&#x20;       const maxDim = 720;
&#x20;       let width = img.width;
&#x20;       let height = img.height;

&#x20;       if (width > maxDim || height > maxDim) {
&#x20;         if (width > height) {
&#x20;           height = Math.round((height \* maxDim) / width);
&#x20;           width = maxDim;
&#x20;         } else {
&#x20;           width = Math.round((width \* maxDim) / height);
&#x20;           height = maxDim;
&#x20;         }
&#x20;       }

&#x20;       const segmenter = getSelfieSegmenter();
&#x20;       if (!segmenter) {
&#x20;         resolve(fallbackBackgroundCutout(img, width, height));
&#x20;         return;
&#x20;       }

&#x20;       let resolved = false;
&#x20;       const timeoutId = setTimeout(() => {
&#x20;         if (!resolved) {
&#x20;           resolved = true;
&#x20;           resolve(fallbackBackgroundCutout(img, width, height));
&#x20;         }
&#x20;       }, 4500);

&#x20;       try {
&#x20;         segmenter.onResults((results) => {
&#x20;           if (resolved) return;
&#x20;           resolved = true;
&#x20;           clearTimeout(timeoutId);

&#x20;           const maskCanvas = document.createElement('canvas');
&#x20;           maskCanvas.width = width;
&#x20;           maskCanvas.height = height;
&#x20;           const maskCtx = maskCanvas.getContext('2d');
&#x20;           maskCtx.drawImage(results.segmentationMask, 0, 0, width, height);
&#x20;           const alphaMaskData = maskCtx.getImageData(0, 0, width, height).data;

&#x20;           const outCanvas = document.createElement('canvas');
&#x20;           outCanvas.width = width;
&#x20;           outCanvas.height = height;
&#x20;           const outCtx = outCanvas.getContext('2d');
&#x20;           outCtx.drawImage(results.image, 0, 0, width, height);

&#x20;           applyClinicalMonochromeTone(outCtx, width, height, alphaMaskData);

&#x20;           resolve(outCanvas.toDataURL('image/jpeg', 0.78));
&#x20;         });

&#x20;         const inputCanvas = document.createElement('canvas');
&#x20;         inputCanvas.width = width;
&#x20;         inputCanvas.height = height;
&#x20;         inputCanvas.getContext('2d').drawImage(img, 0, 0, width, height);
&#x20;         await segmenter.send({ image: inputCanvas });
&#x20;       } catch (err) {
&#x20;         if (!resolved) {
&#x20;           resolved = true;
&#x20;           clearTimeout(timeoutId);
&#x20;           resolve(fallbackBackgroundCutout(img, width, height));
&#x20;         }
&#x20;       }
&#x20;     };
&#x20;     img.src = event.target.result;
&#x20;   };
&#x20;   reader.readAsDataURL(file);
&#x20; });
}

function rebuildOutputPhotoWithPrivacyOption() {
&#x20; return new Promise((resolve) => {
&#x20;   if (!rawCutoutBaseDataUrl) {
&#x20;     capturedPhotoDataUrl = "";
&#x20;     resolve("");
&#x20;     return;
&#x20;   }

&#x20;   if (!isPrivacyBlurEnabled) {
&#x20;     capturedPhotoDataUrl = rawCutoutBaseDataUrl;
&#x20;     updatePhotoPreviewUI();
&#x20;     resolve(capturedPhotoDataUrl);
&#x20;     return;
&#x20;   }

&#x20;   const img = new Image();
&#x20;   img.onload = function() {
&#x20;     const canvas = document.createElement('canvas');
&#x20;     canvas.width = img.width;
&#x20;     canvas.height = img.height;
&#x20;     const ctx = canvas.getContext('2d');

&#x20;     ctx.fillStyle = '#08090a';
&#x20;     ctx.fillRect(0, 0, canvas.width, canvas.height);

&#x20;     if (typeof ctx.filter !== 'undefined') {
&#x20;       ctx.filter = 'blur(7px)';
&#x20;       ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
&#x20;       ctx.globalAlpha = 0.45;
&#x20;       ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
&#x20;       ctx.globalAlpha = 1;
&#x20;       ctx.filter = 'none';
&#x20;     } else {
&#x20;       const smallW = Math.max(64, Math.round(canvas.width \* 0.28));
&#x20;       const smallH = Math.max(80, Math.round(canvas.height \* 0.28));
&#x20;       const tmpCanvas = document.createElement('canvas');
&#x20;       tmpCanvas.width = smallW;
&#x20;       tmpCanvas.height = smallH;
&#x20;       const tmpCtx = tmpCanvas.getContext('2d');
&#x20;       tmpCtx.imageSmoothingEnabled = true;
&#x20;       tmpCtx.imageSmoothingQuality = 'high';
&#x20;       tmpCtx.drawImage(img, 0, 0, smallW, smallH);

&#x20;       ctx.imageSmoothingEnabled = true;
&#x20;       ctx.imageSmoothingQuality = 'high';
&#x20;       ctx.drawImage(tmpCanvas, 0, 0, smallW, smallH, 0, 0, canvas.width, canvas.height);
&#x20;     }

&#x20;     capturedPhotoDataUrl = canvas.toDataURL('image/jpeg', 0.78);
&#x20;     updatePhotoPreviewUI();
&#x20;     resolve(capturedPhotoDataUrl);
&#x20;   };
&#x20;   img.src = rawCutoutBaseDataUrl;
&#x20; });
}

function updatePhotoPreviewUI() {
&#x20; const previewBox = document.getElementById('photo-preview-box');
&#x20; const previewImg = document.getElementById('photo-preview-img');
&#x20; const previewBadge = document.getElementById('photo-preview-badge');
&#x20; const clearBtn = document.getElementById('clear-photo-btn');

&#x20; if (!capturedPhotoDataUrl) {
&#x20;   if (previewBox) previewBox.classList.add('hidden');
&#x20;   if (clearBtn) clearBtn.classList.add('hidden');
&#x20;   return;
&#x20; }

&#x20; if (previewImg) previewImg.src = capturedPhotoDataUrl;
&#x20; if (previewBox) previewBox.classList.remove('hidden');
&#x20; if (clearBtn) clearBtn.classList.remove('hidden');

&#x20; if (previewBadge) {
&#x20;   previewBadge.innerText = isPrivacyBlurEnabled
&#x20;     ? "✓ 影像已完成去背＋黑白曝光＋高解析柔焦模糊"
&#x20;     : "✓ 影像已完成去背與黑白曝光調整";
&#x20; }
}

async function togglePrivacyBlur() {
&#x20; isPrivacyBlurEnabled = !isPrivacyBlurEnabled;

&#x20; const btn = document.getElementById('privacy-blur-toggle');
&#x20; const indicator = document.getElementById('privacy-blur-indicator');
&#x20; const label = document.getElementById('privacy-blur-label');

&#x20; if (isPrivacyBlurEnabled) {
&#x20;   btn.className = "w-full sm:w-auto px-3.5 py-2 rounded text-xs font-mono inline-flex items-center justify-center gap-2 border transition-all cursor-pointer bg-emerald-950/50 text-emerald-400 border-emerald-500/60";
&#x20;   indicator.className = "w-2 h-2 rounded-full bg-emerald-400 animate-pulse";
&#x20;   label.innerText = "隱私模糊保護：已開啟 (臉部已模糊化)";
&#x20; } else {
&#x20;   btn.className = "w-full sm:w-auto px-3.5 py-2 rounded text-xs font-mono inline-flex items-center justify-center gap-2 border transition-all cursor-pointer bg-gray-900/90 text-gray-300 border-gray-700 hover:border-emerald-500/60";
&#x20;   indicator.className = "w-2 h-2 rounded-full bg-gray-500";
&#x20;   label.innerText = "隱私模糊保護：關閉 (點擊模糊化臉部)";
&#x20; }

&#x20; if (rawCutoutBaseDataUrl) {
&#x20;   await rebuildOutputPhotoWithPrivacyOption();
&#x20; }
}

function clearCapturedPhoto() {
&#x20; rawCutoutBaseDataUrl = "";
&#x20; capturedPhotoDataUrl = "";
&#x20; const photoInput = document.getElementById('visitor-photo-input');
&#x20; const previewBox = document.getElementById('photo-preview-box');
&#x20; const previewImg = document.getElementById('photo-preview-img');
&#x20; const clearBtn = document.getElementById('clear-photo-btn');

&#x20; if (photoInput) photoInput.value = "";
&#x20; if (previewImg) previewImg.src = "";
&#x20; if (previewBox) previewBox.classList.add('hidden');
&#x20; if (clearBtn) clearBtn.classList.add('hidden');
}

async function handleSelectedImageFile(file) {
&#x20; if (!file) return;
&#x20; const statusEl = document.getElementById('photo-processing-status');
&#x20; if (statusEl) statusEl.classList.remove('hidden');

&#x20; rawCutoutBaseDataUrl = await processPortraitCutoutAndTone(file);
&#x20; await rebuildOutputPhotoWithPrivacyOption();

&#x20; if (statusEl) statusEl.classList.add('hidden');
}

function startEditVisitorCase(id) {
&#x20; const target = VISITOR_PARTICIPANTS.find(p => p.id === id);
&#x20; if (!target || !canEditOrDeleteCase(target)) {
&#x20;   alert("此病例已超過 3 分鐘修改時限，或非原上傳裝置建立之檔案。");
&#x20;   renderPortalCards(true);
&#x20;   return;
&#x20; }

&#x20; editingCaseId = id;
&#x20; showPortal();

&#x20; document.getElementById('visitor-statement').value = target.statement || "";
&#x20; document.getElementById('visitor-name').value = target.name || "";
&#x20; document.getElementById('visitor-birth').value = target.birthDate || "";
&#x20; document.getElementById('visitor-anxiety').value = target.anxiety || "";

&#x20; if (target.rawImageUrl || target.imageUrl) {
&#x20;   rawCutoutBaseDataUrl = target.rawImageUrl || target.imageUrl;
&#x20;   capturedPhotoDataUrl = target.imageUrl || target.rawImageUrl;
&#x20;   updatePhotoPreviewUI();
&#x20; } else {
&#x20;   clearCapturedPhoto();
&#x20; }

&#x20; document.getElementById('form-mode-title').innerText = \`修改病例檔案：CASE #${id}\`;
&#x20; document.getElementById('submit-case-btn-text').innerText = "儲存病例修改";
&#x20; document.getElementById('cancel-edit-btn').classList.remove('hidden');
&#x20; document.getElementById('visitor-form-feedback').innerText = \`正在編輯 CASE #${id}（僅限建立後 3 分鐘內修改）\`;

&#x20; updateCaseCountersUI();

&#x20; const formSection = document.getElementById('visitor-case-section');
&#x20; if (formSection) {
&#x20;   formSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
&#x20; }
}

function cancelEditMode() {
&#x20; editingCaseId = null;
&#x20; const form = document.getElementById('visitor-case-form');
&#x20; if (form) form.reset();
&#x20; clearCapturedPhoto();

&#x20; document.getElementById('form-mode-title').innerText = "書寫你的病例檔案";
&#x20; document.getElementById('submit-case-btn-text').innerText = "建立並加入觀眾病例";
&#x20; document.getElementById('cancel-edit-btn').classList.add('hidden');
&#x20; document.getElementById('visitor-form-feedback').innerText = "";
&#x20; updateCaseCountersUI();
}

async function deleteVisitorCase(id) {
&#x20; const target = VISITOR_PARTICIPANTS.find(p => p.id === id);
&#x20; if (!target || !canEditOrDeleteCase(target)) {
&#x20;   alert("此病例已超過 3 分鐘刪除時限，或非原上傳裝置建立之檔案。");
&#x20;   renderPortalCards(true);
&#x20;   return;
&#x20; }

&#x20; if (!confirm(\`確定要刪除病例 CASE #${id}（${target.name}）嗎？\`)) {
&#x20;   return;
&#x20; }

&#x20; VISITOR_PARTICIPANTS = VISITOR_PARTICIPANTS.filter(p => p.id !== id);
&#x20; const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);

&#x20; if (editingCaseId === id) {
&#x20;   cancelEditMode();
&#x20; }

&#x20; renderPortalCards(true);

&#x20; if (currentDetailCaseId === id) {
&#x20;   showPortal();
&#x20; }

&#x20; const feedback = document.getElementById('visitor-form-feedback');
&#x20; if (feedback) {
&#x20;   feedback.innerText = syncResult.ok
&#x20;     ? \`已從雲端刪除病例 CASE #${id}（全裝置已同步）。\`
&#x20;     : \`已刪除病例 CASE #${id}（雲端連線異常，已於本機更新）。\`;
&#x20; }
}

/\* 現場觀眾病例登錄與更新處理 \*/
function initVisitorForm() {
&#x20; const form = document.getElementById('visitor-case-form');
&#x20; const feedback = document.getElementById('visitor-form-feedback');
&#x20; const photoInput = document.getElementById('visitor-photo-input');

&#x20; if (photoInput) {
&#x20;   photoInput.addEventListener('change', function(e) {
&#x20;     const file = e.target.files && e.target.files[0];
&#x20;     handleSelectedImageFile(file);
&#x20;   });
&#x20; }

&#x20; if (!form) return;

&#x20; form.addEventListener('submit', async function(e) {
&#x20;   e.preventDefault();

&#x20;   const statementInput = document.getElementById('visitor-statement').value.trim();
&#x20;   const nameInput = document.getElementById('visitor-name').value.trim();
&#x20;   const birthInput = document.getElementById('visitor-birth').value;
&#x20;   const anxietyInput = document.getElementById('visitor-anxiety').value.trim();

&#x20;   if (!statementInput) {
&#x20;     feedback.textContent = "請填寫本體焦慮自白紀錄。";
&#x20;     return;
&#x20;   }

&#x20;   feedback.textContent = "正在同步至雲端資料庫...";

&#x20;   if (editingCaseId) {
&#x20;     const idx = VISITOR_PARTICIPANTS.findIndex(p => p.id === editingCaseId);
&#x20;     if (idx === -1 || !canEditOrDeleteCase(VISITOR_PARTICIPANTS[idx])) {
&#x20;       feedback.textContent = "已超過 3 分鐘可修改時限，無法儲存變更。";
&#x20;       cancelEditMode();
&#x20;       renderPortalCards(true);
&#x20;       return;
&#x20;     }

&#x20;     const existing = VISITOR_PARTICIPANTS[idx];
&#x20;     const resolvedName = nameInput || existing.name;
&#x20;     const resolvedBirth = birthInput || existing.birthDate || "2005-01-01";

&#x20;     VISITOR_PARTICIPANTS[idx] = {
&#x20;       ...existing,
&#x20;       name: resolvedName,
&#x20;       target: \`${resolvedName} (本體)\`,
&#x20;       anxiety: anxietyInput || existing.anxiety,
&#x20;       birthDate: resolvedBirth,
&#x20;       rawImageUrl: rawCutoutBaseDataUrl,
&#x20;       imageUrl: capturedPhotoDataUrl,
&#x20;       deviceImageUrl: capturedPhotoDataUrl,
&#x20;       statement: statementInput
&#x20;     };

&#x20;     const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);
&#x20;     const updatedId = existing.id;
&#x20;     cancelEditMode();
&#x20;     renderPortalCards(true);
&#x20;     feedback.textContent = syncResult.ok
&#x20;       ? \`已更新病例 CASE #${updatedId}（${resolvedName}）並同步至所有裝置。\`
&#x20;       : \`已更新病例 CASE #${updatedId}（雲端連線異常，已暫存於本機）。\`;
&#x20;     return;
&#x20;   }

&#x20;   const latestCloud = await fetchCloudVisitorCases();
&#x20;   if (latestCloud.length > 0) {
&#x20;     VISITOR_PARTICIPANTS = latestCloud;
&#x20;   }

&#x20;   const maxNum = VISITOR_PARTICIPANTS.reduce((max, item) => {
&#x20;     const n = parseInt(item.id, 10);
&#x20;     return !isNaN(n) && n > max ? n : max;
&#x20;   }, BASE_PARTICIPANTS.length);

&#x20;   const nextNum = maxNum + 1;
&#x20;   const nextId = String(nextNum).padStart(2, '0');
&#x20;   const resolvedName = nameInput || \`現場委託人 #${nextId}\`;
&#x20;   const resolvedBirth = birthInput || "2005-01-01";

&#x20;   const newCase = {
&#x20;     id: nextId,
&#x20;     name: resolvedName,
&#x20;     role: \`現場延伸病例 ${nextId} 號\`,
&#x20;     target: \`${resolvedName} (本體)\`,
&#x20;     anxiety: anxietyInput || "85.0% (現場登錄)",
&#x20;     birthDate: resolvedBirth,
&#x20;     rawImageUrl: rawCutoutBaseDataUrl,
&#x20;     imageUrl: capturedPhotoDataUrl,
&#x20;     deviceImageUrl: capturedPhotoDataUrl,
&#x20;     signatureUrl: "",
&#x20;     statement: statementInput,
&#x20;     ownerToken: CURRENT_DEVICE_TOKEN,
&#x20;     createdAt: Date.now()
&#x20;   };

&#x20;   VISITOR_PARTICIPANTS = [...VISITOR_PARTICIPANTS, newCase];
&#x20;   const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);

&#x20;   renderPortalCards(true);
&#x20;   form.reset();
&#x20;   clearCapturedPhoto();

&#x20;   if (syncResult.ok) {
&#x20;     feedback.textContent = \`已建立病例 CASE #${nextId}（${resolvedName}）並同步至雲端，3 分鐘內可於本裝置修改或刪除。\`;
&#x20;   } else {
&#x20;     feedback.textContent = \`已建立病例 CASE #${nextId}（${resolvedName}），雲端連線異常，請確認 Firebase 測試規則權限。\`;
&#x20;   }
&#x20; });
}

/\* 定期從雲端同步最新病例（每 3 秒一次） \*/
function startCloudSyncPolling() {
&#x20; if (!hasFirebaseConfigured()) return;

&#x20; setInterval(async () => {
&#x20;   if (editingCaseId) return;
&#x20;   const cloudCases = await fetchCloudVisitorCases();
&#x20;   const nextSig = cloudCases.map(p => \`${p.id}|${p.name}|${p.statement}|${Boolean(resolveVisitorPhotoSrc(p))}\`).join("||");
&#x20;   if (nextSig !== lastRenderedSignature) {
&#x20;     VISITOR_PARTICIPANTS = cloudCases;
&#x20;     saveLocalVisitorCases(cloudCases);
&#x20;     renderPortalCards(true);
&#x20;   }
&#x20; }, 3000);
}

/\* 定期檢查 3 分鐘編輯時限到期狀態並重新渲染 \*/
function startEditWindowTicker() {
&#x20; setInterval(() => {
&#x20;   const expiredIds = VISITOR_PARTICIPANTS
&#x20;     .filter(p => p.ownerToken === CURRENT_DEVICE_TOKEN && p.createdAt)
&#x20;     .filter(p => (Date.now() - Number(p.createdAt)) >= EDIT_WINDOW_MS)
&#x20;     .map(p => p.id);

&#x20;   if (expiredIds.length > 0) {
&#x20;     if (editingCaseId && expiredIds.includes(editingCaseId)) {
&#x20;       cancelEditMode();
&#x20;     }
&#x20;     renderPortalCards(true);
&#x20;   }

&#x20;   if (currentDetailCaseId) {
&#x20;     updateDetailOwnerControls(currentDetailCaseId);
&#x20;   }
&#x20; }, 1000);
}

/\* 頁面切換 \*/
function showPortal() {
&#x20; currentDetailCaseId = null;
&#x20; document.getElementById('portal-view').classList.remove('hidden');
&#x20; document.getElementById('detail-view').classList.add('hidden');
&#x20; document.getElementById('header-output-box').style.display = '';

&#x20; if (breathTimer) {
&#x20;   clearInterval(breathTimer);
&#x20; }
}

function updateDetailOwnerControls(id) {
&#x20; const controlsEl = document.getElementById('detail-owner-controls');
&#x20; const countdownEl = document.getElementById('detail-edit-countdown');
&#x20; const editBtn = document.getElementById('detail-edit-btn');
&#x20; const deleteBtn = document.getElementById('detail-delete-btn');

&#x20; const visitorItem = VISITOR_PARTICIPANTS.find(p => p.id === id);
&#x20; if (visitorItem && canEditOrDeleteCase(visitorItem)) {
&#x20;   const remain = getRemainingEditSeconds(visitorItem);
&#x20;   controlsEl.classList.remove('hidden');
&#x20;   countdownEl.innerText = \`可編輯剩餘 ${formatSecondsMMSS(remain)}\`;
&#x20;   editBtn.onclick = () => startEditVisitorCase(id);
&#x20;   deleteBtn.onclick = () => deleteVisitorCase(id);
&#x20; } else {
&#x20;   controlsEl.classList.add('hidden');
&#x20; }
}

function showDetail(id) {
&#x20; const allCases = getAllParticipants();
&#x20; const data = allCases.find(p => p.id === id) || BASE_PARTICIPANTS[0];
&#x20; currentDetailCaseId = data.id;
&#x20; const isVisitorCase = isVisitorCaseById(data.id);

&#x20; document.getElementById('header-output-box').style.display = 'none';
&#x20; updateDetailOwnerControls(data.id);

&#x20; document.getElementById('detail-id').innerText = \`CASE #${data.id}\`;
&#x20; document.getElementById('detail-name').innerText = data.name;
&#x20; document.getElementById('detail-role').innerText = data.role;
&#x20; document.getElementById('detail-target').innerText = data.target;
&#x20; document.getElementById('detail-anxiety').innerText = data.anxiety;
&#x20; document.getElementById('detail-statement').innerText = \`"${data.statement}"\`;

&#x20; document.getElementById('detail-birth-info').innerText =
&#x20;   \`自出生日期 ${data.birthDate} 起算，依成人平靜狀態每分鐘 15 次呼吸即時動態估算\`;

&#x20; const visitorImg = document.getElementById('detail-visitor-img');
&#x20; const principalImg = document.getElementById('detail-principal-img');
&#x20; const deviceImg = document.getElementById('detail-device-img');
&#x20; const noImgFallback = document.getElementById('detail-no-image-fallback');

&#x20; applyDetailLeftMode(isVisitorCase);

&#x20; if (isVisitorCase) {
&#x20;   const vUrl = resolveVisitorPhotoSrc(data);
&#x20;   visitorImg.onerror = function() {
&#x20;     visitorImg.classList.add('hidden');
&#x20;     if (noImgFallback) noImgFallback.classList.remove('hidden');
&#x20;   };

&#x20;   if (vUrl) {
&#x20;     visitorImg.src = vUrl;
&#x20;     visitorImg.classList.remove('hidden');
&#x20;     if (noImgFallback) noImgFallback.classList.add('hidden');
&#x20;   } else {
&#x20;     visitorImg.src = '';
&#x20;     visitorImg.classList.add('hidden');
&#x20;     if (noImgFallback) noImgFallback.classList.remove('hidden');
&#x20;   }
&#x20; } else {
&#x20;   const pUrl = formatImageSrc(data.imageUrl);
&#x20;   const dUrl = formatImageSrc(data.deviceImageUrl || data.imageUrl);

&#x20;   principalImg.onerror = function() {
&#x20;     principalImg.classList.add('hidden');
&#x20;     deviceImg.classList.add('hidden');
&#x20;     if (noImgFallback) noImgFallback.classList.remove('hidden');
&#x20;   };

&#x20;   if (pUrl) {
&#x20;     principalImg.src = pUrl;
&#x20;     deviceImg.src = dUrl;
&#x20;     principalImg.classList.remove('hidden');
&#x20;     deviceImg.classList.remove('hidden');
&#x20;     if (noImgFallback) noImgFallback.classList.add('hidden');
&#x20;   } else {
&#x20;     principalImg.src = '';
&#x20;     deviceImg.src = '';
&#x20;     principalImg.classList.add('hidden');
&#x20;     deviceImg.classList.add('hidden');
&#x20;     if (noImgFallback) noImgFallback.classList.remove('hidden');
&#x20;   }
&#x20; }

&#x20; const sigImg = document.getElementById('detail-signature-img');
&#x20; const sigPlaceholder = document.getElementById('detail-signature-placeholder');
&#x20; if (data.signatureUrl) {
&#x20;   sigImg.src = formatImageSrc(data.signatureUrl);
&#x20;   sigImg.classList.remove('hidden');
&#x20;   sigPlaceholder.classList.add('hidden');
&#x20; } else {
&#x20;   sigImg.src = '';
&#x20;   sigImg.classList.add('hidden');
&#x20;   sigPlaceholder.classList.remove('hidden');
&#x20; }

&#x20; startBreathCounter(data.birthDate);

&#x20; document.getElementById('portal-view').classList.add('hidden');
&#x20; document.getElementById('detail-view').classList.remove('hidden');

&#x20; window\.scrollTo({
&#x20;   top: 0,
&#x20;   behavior: 'smooth'
&#x20; });

&#x20; if (!isVisitorCase) {
&#x20;   setTimeout(updateImgDimensions, 60);
&#x20; }
}

/\* 初始化 \*/
window\.addEventListener('DOMContentLoaded', async () => {
&#x20; renderPortalCards(true);
&#x20; initComparisonSlider();
&#x20; startOutputFluctuationWithPause();
&#x20; initVisitorForm();
&#x20; startEditWindowTicker();

&#x20; if (hasFirebaseConfigured()) {
&#x20;   const cloudCases = await fetchCloudVisitorCases();
&#x20;   VISITOR_PARTICIPANTS = cloudCases;
&#x20;   saveLocalVisitorCases(cloudCases);
&#x20;   renderPortalCards(true);
&#x20;   startCloudSyncPolling();
&#x20; }

&#x20; const urlParams = new URLSearchParams(window\.location.search);
&#x20; const targetId = urlParams.get('id');

&#x20; if (targetId) {
&#x20;   showDetail(targetId);
&#x20; }
});
\</script>

\</body>
\</html>
