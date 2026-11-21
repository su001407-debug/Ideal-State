<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>理想狀態 - Archive Portal</title>
<script src="https://cdn.tailwindcss.com"></script>

<style>
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;600;800&family=Noto+Sans+TC:wght@300;400;500;700;900&display=swap');

* {
  box-sizing: border-box;
}

body {
  font-family: 'JetBrains Mono', 'Noto Sans TC', monospace;
  background-color: #08090a;
  color: #d1d5db;
  overflow-x: hidden;
}

.bg-grid-dark {
  background-size: 32px 32px;
  background-image:
    linear-gradient(to right, rgba(255,255,255,.012) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(255,255,255,.012) 1px, transparent 1px);
}

.glass-card {
  background: rgba(15,18,22,.85);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(255,255,255,.08);
  transition: all .35s cubic-bezier(.16,1,.3,1);
}

@media (max-width: 767px) {
  .glass-card {
    background: rgba(15,18,22,.96);
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }
}

.glass-card:hover {
  border-color: rgba(16,185,129,.4);
  transform: translateY(-4px);
  box-shadow:
    0 16px 36px -10px rgba(0,0,0,.9),
    0 0 24px rgba(16,185,129,.08);
}

.btn-white {
  background: #fff;
  color: #000;
  font-weight: 700;
  border: none;
  transition: all .25s ease;
}

.btn-white:hover {
  background: #e5e7eb;
  box-shadow: 0 0 16px rgba(255,255,255,.3);
}

.btn-neon {
  background: #39ff14;
  color: #000;
  font-weight: 800;
  border: none;
  transition: all .2s ease;
}

.btn-neon:hover {
  background: #55ff33;
  box-shadow: 0 0 20px rgba(57,255,20,.6);
}

.badge-emerald-active {
  background: rgba(16,185,129,.2) !important;
  color: #10b981 !important;
  font-weight: 700 !important;
  border: 1px solid rgba(16,185,129,.5) !important;
  box-shadow: 0 0 12px rgba(16,185,129,.25);
}

.badge-inactive {
  background: rgba(15,18,22,.85) !important;
  color: #6b7280 !important;
  font-weight: 500 !important;
  border: 1px solid rgba(255,255,255,.1) !important;
  box-shadow: none !important;
}

.stamp-red {
  border: 2px solid #ef4444;
  color: #ef4444;
  transform: rotate(-6deg);
  letter-spacing: .15em;
  text-shadow: 0 0 10px rgba(239,68,68,.3);
  box-shadow:
    inset 0 0 12px rgba(239,68,68,.12),
    0 0 8px rgba(239,68,68,.2);
}

@keyframes pulse-emerald {
  0%,100% {
    color: #10b981;
    opacity: 1;
  }
  50% {
    color: #6ee7b7;
    opacity: .7;
  }
}

.breath-counter-active {
  animation: pulse-emerald 2s infinite;
}

@keyframes mobileFade {
  0%,45% { opacity: 0; }
  55%,95% { opacity: 1; }
  100% { opacity: 0; }
}

@keyframes mobileBgFade {
  0%,45% {
    background-color: rgba(0,0,0,.7);
    backdrop-filter: blur(6px);
  }
  55%,95% {
    background-color: transparent;
    backdrop-filter: none;
  }
  100% {
    background-color: rgba(0,0,0,.7);
    backdrop-filter: blur(6px);
  }
}

@media (hover: none), (max-width: 767px) {
  .mobile-auto-fade {
    animation: mobileFade 6s infinite ease-in-out !important;
    will-change: opacity;
  }

  .mobile-auto-bg-fade {
    animation: mobileBgFade 6s infinite ease-in-out !important;
  }
}

#comparison-slider {
  touch-action: none;
}

.modal-backdrop {
  transition: opacity .3s cubic-bezier(.16,1,.3,1);
}

.modal-backdrop.hidden-modal {
  opacity: 0;
  pointer-events: none;
}

.modal-backdrop.active-modal {
  opacity: 1;
  pointer-events: auto;
}

.modal-content {
  transition:
    transform .3s cubic-bezier(.16,1,.3,1),
    opacity .3s cubic-bezier(.16,1,.3,1);
}

.hidden-modal .modal-content {
  transform: scale(.94) translateY(12px);
  opacity: 0;
}

.active-modal .modal-content {
  transform: scale(1) translateY(0);
  opacity: 1;
}

.clinical-input {
  display: block;
  width: 100%;
  max-width: 100%;
  min-width: 0;
  min-height: 44px;
  background-color: rgba(3, 7, 18, 0.92);
  border: 1px solid #1f2937;
  border-radius: 6px;
  padding: 10px 14px;
  font-size: 13px;
  line-height: 1.4;
  color: #ffffff;
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  transition: border-color .2s ease, box-shadow .2s ease;
}

.clinical-input:focus {
  border-color: #10b981;
  box-shadow: 0 0 0 1px rgba(16, 185, 129, 0.25);
}

input[type="date"].clinical-input {
  color-scheme: dark;
  height: 44px;
}

input[type="date"].clinical-input::-webkit-date-and-time-value {
  text-align: left;
}

.archive-header-main {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.archive-title-group {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  min-width: 0;
  flex: 1;
}

.archive-eyebrow {
  font-size: 10px;
  letter-spacing: .25em;
  color: #6b7280;
  margin: 0 0 16px;
  padding: 0;
}

.archive-title {
  font-family: 'Noto Sans TC', sans-serif;
  font-size: 64px !important;
  line-height: 1.1;
  letter-spacing: -.04em;
  font-weight: 700;
  color: #f3f4f6;
  margin: 0 !important;
  padding: 0 !important;
  border: 0 !important;
  outline: 0 !important;
  box-shadow: none !important;
  text-decoration: none !important;
}

.archive-subtitle {
  font-size: clamp(13px,1.3vw,18px);
  line-height: 1.5;
  letter-spacing: .03em;
  color: #6b7280;
  margin: 8px 0 0 !important;
  padding: 0 !important;
  border: 0 !important;
  outline: 0 !important;
  box-shadow: none !important;
}

.archive-title-group hr,
.archive-title-divider,
.archive-title::before,
.archive-title::after,
.archive-subtitle::before,
.archive-subtitle::after {
  display: none !important;
  content: none !important;
}

.archive-right-column {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  justify-content: flex-end;
  gap: 20px;
  flex-shrink: 0;
}

.archive-header-actions {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 12px;
  flex-shrink: 0;
}

#header-concept-button,
#header-output-box {
  border-radius: 4px !important;
  font-size: 12px;
  line-height: 20px;
  min-height: 36px;
  padding: 8px 16px;
  white-space: nowrap;
}

.concept-modal-footer {
  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
  width: 100% !important;
  margin-left: auto !important;
  margin-right: auto !important;
  text-align: center !important;
}

#concept-close-button {
  display: inline-flex !important;
  align-items: center !important;
  justify-content: center !important;
  flex: 0 0 auto;
  margin-left: auto !important;
  margin-right: auto !important;
  padding: 8px 20px;
  background: #ffffff !important;
  background-color: #ffffff !important;
  color: #000000 !important;
  -webkit-text-fill-color: #000000 !important;
  border: 1px solid #ffffff !important;
  border-radius: 4px;
  font-weight: 700 !important;
  text-align: center !important;
  opacity: 1 !important;
  appearance: none;
  -webkit-appearance: none;
  cursor: pointer;
  transition: background-color .2s ease;
}

#concept-close-button:hover,
#concept-close-button:focus,
#concept-close-button:active {
  background: #e5e7eb !important;
  background-color: #e5e7eb !important;
  color: #000000 !important;
  -webkit-text-fill-color: #000000 !important;
  border-color: #e5e7eb !important;
}

@media (min-width: 768px) {
  .archive-header-main {
    flex-direction: row;
    align-items: flex-end;
    justify-content: space-between;
  }

  .archive-right-column {
    align-self: stretch;
  }

  .archive-header-actions {
    align-self: flex-end;
  }
}

@media (max-width: 767px) {
  .archive-title {
    font-size: 56px !important;
  }

  .archive-subtitle {
    font-size: clamp(12px,3.5vw,16px);
  }

  .archive-right-column {
    align-items: flex-start;
    width: 100%;
  }
}
</style>
</head>

<body class="bg-grid-dark min-h-screen text-gray-300 relative selection:bg-emerald-500 selection:text-black">

<div class="hidden md:block fixed top-0 left-1/2 -translate-x-1/2 w-full max-w-6xl h-80 bg-emerald-950/20 blur-[130px] pointer-events-none rounded-full"></div>

<!-- 系統訊息提示條 -->
<div
  id="system-toast"
  class="hidden fixed bottom-5 right-5 z-50 max-w-sm bg-[#0d1117]/95 border border-emerald-500/50 text-gray-200 px-4 py-3 rounded-lg shadow-2xl font-mono text-xs flex items-center justify-between gap-3 backdrop-blur-md"
>
  <span id="system-toast-text" class="leading-relaxed">---</span>
  <button
    type="button"
    onclick="hideToast()"
    class="text-gray-400 hover:text-white font-bold px-1 shrink-0 cursor-pointer"
  >
    ✕
  </button>
</div>

<div class="max-w-5xl mx-auto px-4 sm:px-6 py-8 md:py-14 relative z-10">

  <!-- HEADER -->
  <header class="border-b border-gray-700/70 pb-8 mb-12">

    <div class="flex flex-wrap items-center justify-between gap-3 mb-6 font-mono text-[10px] sm:text-xs tracking-[0.14em]">

      <div class="flex items-center gap-2 text-emerald-400">
        <span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse shrink-0"></span>
        <span>
          SYSTEM RUNNING
          <span class="text-gray-600 mx-1">/</span>
          CLINICAL ARCHIVE PORTAL
        </span>
      </div>

      <span class="text-gray-500 tracking-[0.18em]">
        ARCHIVE / 2026 / 001
      </span>

    </div>

    <div class="archive-header-main">

      <div class="archive-title-group">

        <div class="archive-eyebrow">
          IDEAL STATE — ARCHIVE PORTAL
        </div>

        <h1 class="archive-title">理想狀態</h1>

        <p class="archive-subtitle">
          個人焦慮檔案與系統診斷報告
          <span class="text-gray-600">/</span>
          Archive Portal
        </p>

      </div>

      <div class="archive-right-column">

        <div class="archive-header-actions">

          <button
            id="header-concept-button"
            onclick="openConceptModal()"
            class="btn-white inline-flex items-center justify-center gap-2 shadow-sm font-mono cursor-pointer"
          >
            <span>ⓘ</span>
            <span>作品理念 (CONCEPT)</span>
          </button>

          <div
            id="header-output-box"
            class="inline-flex items-center gap-3 font-mono text-gray-400 bg-gray-950/80 border border-gray-700/80"
          >
            <span class="text-gray-500">VALUABLE OUTPUT:</span>
            <span id="header-output-counter" class="text-emerald-400 font-bold">
              0.000 KG/H
            </span>
          </div>

        </div>

      </div>

    </div>

  </header>

  <!-- 首頁 -->
  <main id="portal-view" class="space-y-14">

    <!-- 原始四位委託人檔案目錄 -->
    <section class="space-y-8">

      <div class="flex justify-between items-end border-b border-gray-800/60 pb-3 gap-4">

        <div>
          <h2 class="text-lg font-bold text-white tracking-wide">
            委託人檔案目錄
          </h2>

          <p class="text-xs text-gray-500">
            點擊個案卡片，檢視完整「臨床診斷報告與生命累積呼吸數據」
          </p>
        </div>

        <span class="text-xs text-gray-600 font-mono shrink-0">
          TOTAL: 04 CASES
        </span>

      </div>

      <div
        id="cards-container"
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6"
      ></div>

    </section>

    <!-- 現場觀眾病例獨立區塊 -->
    <section class="border-t border-gray-800/80 pt-12 space-y-8">

      <div class="flex flex-wrap justify-between items-end border-b border-gray-800/60 pb-3 gap-3">

        <div>
          <span class="text-[10px] font-mono text-emerald-400 tracking-widest block mb-1">
            PUBLIC ARCHIVE EXTENSION / 現場觀眾延伸檔案
          </span>

          <h2 class="text-lg font-bold text-white tracking-wide">
            現場觀眾病例檔案目錄
          </h2>

          <p class="text-xs text-gray-500">
            跨裝置同步收錄觀眾提交之個人焦慮病例（上傳後 3 分鐘內可於原裝置修改或刪除）
          </p>
        </div>

        <span id="visitor-total-label" class="text-xs text-gray-600 font-mono shrink-0">
          VISITOR CASES: 00
        </span>

      </div>

      <div
        id="visitor-cards-container"
        class="grid grid-cols-2 md:grid-cols-2 lg:grid-cols-4 gap-3.5 sm:gap-6"
      ></div>

      <!-- 現場觀眾病例登錄表單 -->
      <div id="visitor-case-section" class="glass-card rounded-xl p-5 sm:p-6 md:p-8 space-y-6 w-full overflow-hidden">

        <div class="flex flex-col sm:flex-row sm:items-end justify-between border-b border-gray-800/80 pb-4 gap-2">

          <div class="min-w-0">
            <span class="text-[10px] font-mono text-emerald-400 tracking-widest block mb-1">
              VISITOR INTAKE TERMINAL / 現場病例登錄
            </span>

            <h3 id="form-mode-title" class="text-base sm:text-lg font-bold text-white tracking-wide">
              書寫你的病例檔案
            </h3>

            <p class="text-xs text-gray-500 mt-1 leading-relaxed">
              請填寫焦慮自白與出生日期即可建立病例（姓名、焦慮指數與照片皆可選填）；上傳後 3 分鐘內僅限原上傳裝置可編輯或刪除
            </p>
          </div>

          <span id="next-case-preview" class="text-xs font-mono text-emerald-400 shrink-0">
            NEXT ID: CASE #05
          </span>

        </div>

        <form id="visitor-case-form" class="space-y-5 font-mono text-xs w-full">

          <div class="w-full">
            <label for="visitor-statement" class="block text-[10px] text-emerald-400 font-bold mb-1.5">
              本體焦慮自白紀錄 (ANXIETY STATEMENT) * 必填
            </label>
            <textarea
              id="visitor-statement"
              rows="4"
              required
              placeholder="寫下你面對空閒、休息或沒有產出時的真實感受與焦慮自白..."
              class="clinical-input font-sans leading-relaxed"
            ></textarea>
          </div>

          <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 w-full">

            <div class="w-full min-w-0">
              <label for="visitor-name" class="block text-[10px] text-gray-400 mb-1.5">
                姓名 / 代號 (選填，未填自動匿名)
              </label>
              <input
                type="text"
                id="visitor-name"
                placeholder="例如：王Ｏ明（選填）"
                class="clinical-input"
              >
            </div>

            <div class="w-full min-w-0">
              <label for="visitor-birth" class="block text-[10px] text-emerald-400 font-bold mb-1.5">
                出生日期 (必填，計算呼吸數) *
              </label>
              <input
                type="date"
                id="visitor-birth"
                required
                class="clinical-input"
              >
            </div>

            <div class="w-full min-w-0">
              <label for="visitor-anxiety" class="block text-[10px] text-gray-400 mb-1.5">
                焦慮指數 (選填)
              </label>
              <input
                type="text"
                id="visitor-anxiety"
                placeholder="例如：89.5% (極高)"
                class="clinical-input"
              >
            </div>

          </div>

          <!-- 拍照與選取照片建檔區（選填） -->
          <div class="bg-gray-950/75 border border-gray-800/90 rounded-lg p-4 sm:p-5 space-y-4 w-full">

            <div class="space-y-1">
              <span class="block text-[11px] text-gray-200 font-bold">
                現場本體影像拍攝 / 選取照片 (選填 CAMERA / PHOTO)
              </span>
              <span class="block text-[10px] text-gray-500 font-sans leading-relaxed">
                點擊下方按鈕可拍照或從相簿選取照片，系統將自動套用黑白灰階與微調曝光，並可切換隱私模糊保護
              </span>
            </div>

            <div class="w-full">

              <label
                for="visitor-photo-input"
                class="btn-white w-full py-2.5 px-4 rounded text-xs inline-flex items-center justify-center gap-2 cursor-pointer text-center"
              >
                <span>📷</span>
                <span>拍照 / 選取照片</span>
              </label>

              <input
                type="file"
                id="visitor-photo-input"
                accept="image/*"
                class="hidden"
              >

            </div>

            <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 pt-2 border-t border-gray-800/70">

              <button
                type="button"
                id="privacy-blur-toggle"
                onclick="togglePrivacyBlur()"
                class="w-full sm:w-auto px-3.5 py-2 rounded text-xs font-mono inline-flex items-center justify-center gap-2 border transition-all cursor-pointer bg-gray-900/90 text-gray-300 border-gray-700 hover:border-emerald-500/60"
              >
                <span id="privacy-blur-indicator" class="w-2 h-2 rounded-full bg-gray-500"></span>
                <span id="privacy-blur-label">隱私模糊保護：關閉 (點擊模糊化臉部)</span>
              </button>

              <button
                type="button"
                id="clear-photo-btn"
                onclick="clearCapturedPhoto()"
                class="hidden w-full sm:w-auto px-3.5 py-2 rounded border border-red-900/60 bg-red-950/20 text-red-400 hover:bg-red-950/40 text-xs text-center cursor-pointer"
              >
                清除已選照片
              </button>

            </div>

            <div id="photo-processing-status" class="hidden text-[11px] text-emerald-400 flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></span>
              <span>正在進行黑白灰階與曝光微調處理...</span>
            </div>

            <div id="photo-preview-box" class="hidden pt-3 border-t border-gray-800/80 flex items-center gap-4">

              <img
                id="photo-preview-img"
                src=""
                alt="Captured Preview"
                class="w-16 h-20 object-cover rounded border border-emerald-500/40 bg-black shrink-0"
              >

              <div class="text-[10px] space-y-1 min-w-0">
                <span id="photo-preview-badge" class="text-emerald-400 font-bold block">
                  ✓ 影像已完成黑白曝光調整
                </span>
              </div>

            </div>

          </div>

          <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 pt-1 w-full">

            <span id="visitor-form-feedback" class="text-[11px] text-emerald-400 min-h-[16px] leading-relaxed"></span>

            <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-2.5 w-full sm:w-auto shrink-0">

              <button
                type="button"
                id="cancel-edit-btn"
                onclick="cancelEditMode()"
                class="hidden w-full sm:w-auto px-4 py-2.5 rounded border border-gray-700 text-gray-300 hover:text-white transition-colors text-xs text-center cursor-pointer"
              >
                取消修改
              </button>

              <button
                type="submit"
                id="submit-case-btn"
                class="btn-neon w-full sm:w-auto px-5 py-2.5 rounded text-xs inline-flex items-center justify-center gap-2 cursor-pointer text-center"
              >
                <span id="submit-case-btn-text">建立並加入觀眾病例</span>
                <span>→</span>
              </button>

            </div>

          </div>

        </form>

      </div>

    </section>

  </main>

  <!-- 個案詳情 -->
  <main id="detail-view" class="hidden space-y-6">

    <div class="flex flex-wrap items-center justify-between gap-3">

      <button
        onclick="showPortal()"
        class="inline-flex items-center space-x-2 text-xs text-gray-300 hover:text-white transition-colors bg-gray-900/90 hover:bg-gray-800 px-4 py-2 rounded border border-gray-700 cursor-pointer"
      >
        <span>←</span>
        <span>返回檔案目錄 (RETURN TO DIRECTORY)</span>
      </button>

      <div id="detail-owner-controls" class="hidden flex flex-wrap items-center gap-2 font-mono text-xs">

        <span id="detail-edit-countdown" class="text-[11px] text-emerald-400 bg-emerald-950/30 border border-emerald-500/30 px-2.5 py-1.5 rounded">
          可編輯剩餘 03:00
        </span>

        <button
          type="button"
          id="detail-edit-btn"
          class="px-3.5 py-1.5 rounded bg-white text-black font-bold hover:bg-gray-200 transition-colors cursor-pointer"
        >
          ✎ 修改病例
        </button>

        <button
          type="button"
          id="detail-delete-btn"
          class="px-3.5 py-1.5 rounded bg-red-950/60 text-red-400 border border-red-800/70 hover:bg-red-900/60 font-bold transition-colors cursor-pointer"
        >
          ✕ 刪除病例
        </button>

      </div>

    </div>

    <article class="glass-card rounded-xl p-5 sm:p-6 md:p-10 relative space-y-6">

      <div class="border-b border-gray-800 pb-2 space-y-1">

        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-2">

          <div>
            <span id="detail-id" class="inline-block text-white text-xs font-mono mb-2">
              CASE #000
            </span>

            <h2 id="detail-name" class="text-2xl md:text-3xl font-bold text-white">
              ---
            </h2>

            <p id="detail-role" class="text-xs font-mono text-gray-400 mt-1">
              ---
            </p>
          </div>

          <div class="stamp-red px-4 py-1.5 rounded text-xs font-bold font-mono tracking-widest uppercase shrink-0">
            AUTHORIZED IDLE
          </div>

        </div>

        <p class="text-[10px] font-mono text-gray-400 tracking-wide pb-1">
         
        </p>

      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6 items-stretch">

        <div
          id="detail-slider-container"
          class="w-full h-full min-h-[360px] md:min-h-full bg-gray-950 rounded-lg overflow-hidden border border-gray-800 relative select-none group"
        >

          <div
            id="detail-no-image-fallback"
            class="hidden absolute inset-0 flex flex-col items-center justify-center space-y-2 text-gray-700 z-20 bg-gray-950"
          >
            <svg
              class="w-12 h-12 stroke-gray-700"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="1.5"
                d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
              />
            </svg>
            <span class="text-[10px] font-mono text-gray-600">
              NO SCAN DATA
            </span>
          </div>

          <!-- 觀眾病例專屬照片 -->
          <img
            id="detail-visitor-img"
            src=""
            alt="Visitor Portrait"
            class="hidden w-full h-full object-cover absolute inset-0 z-10 pointer-events-none"
          >

          <!-- 原始四位委託人專用結構 -->
          <img
            id="detail-device-img"
            src=""
            alt="Proxy Device"
            class="w-full h-full object-cover absolute inset-0 pointer-events-none z-0"
          >

          <div
            id="slider-clip-wrapper"
            class="absolute inset-0 overflow-hidden w-1/2 border-r-2 border-emerald-500 z-10"
          >
            <img
              id="detail-principal-img"
              src=""
              alt="Principal"
              class="w-full h-full object-cover pointer-events-none"
              style="max-width:none;"
            >
          </div>

          <input
            type="range"
            min="0"
            max="100"
            value="50"
            id="comparison-slider"
            class="absolute inset-0 w-full h-full opacity-0 cursor-ew-resize z-30"
          >

          <div
            id="slider-line"
            class="absolute top-0 bottom-0 w-0.5 bg-emerald-500 pointer-events-none z-20 left-1/2 -translate-x-1/2 shadow-[0_0_8px_rgba(16,185,129,0.5)]"
          >
            <div class="w-6 h-6 rounded-full bg-emerald-500 text-black font-extrabold text-[10px] flex items-center justify-center absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 shadow-lg">
              ◄►
            </div>
          </div>

          <span
            id="badge-principal"
            class="absolute bottom-2.5 left-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none badge-inactive"
          >
            本體 PRINCIPAL
          </span>

          <span
            id="badge-device"
            class="absolute bottom-2.5 right-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none badge-inactive"
          >
            裝置 PROXY DEVICE
          </span>

        </div>

        <div class="md:col-span-2 bg-gray-950/80 rounded-lg p-5 border border-gray-800/80 flex flex-col justify-between space-y-4 font-mono text-xs overflow-hidden">

          <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 border-b border-gray-800/80 pb-4">

            <div>
              <span class="text-gray-500 text-[10px] block">
                SERVICE TARGET (代理對象)
              </span>
              <span id="detail-target" class="font-bold text-gray-200">
                ---
              </span>
            </div>

            <div>
              <span class="text-gray-500 text-[10px] block">
                ANXIETY INDEX (焦慮指數)
              </span>
              <span id="detail-anxiety" class="font-bold text-red-400">
                ---
              </span>
            </div>

            <div class="sm:col-span-2 bg-emerald-950/20 p-3.5 rounded border border-emerald-500/20 w-full overflow-hidden">

              <div class="flex justify-between items-center mb-1.5 gap-2">

                <span class="text-emerald-400 text-[10px] font-bold tracking-wider break-words leading-tight">
                  LIFETIME CUMULATIVE RESPIRATION (出生至今生命累積總呼吸數)
                </span>

                <span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-ping shrink-0"></span>

              </div>

              <div class="flex flex-wrap items-baseline gap-x-2 gap-y-1 my-1">

                <span
                  id="detail-breath-counter"
                  class="text-lg sm:text-xl md:text-2xl font-black text-emerald-400 breath-counter-active tracking-wider break-all"
                >
                  0
                </span>

                <span class="text-[11px] text-gray-400 shrink-0">
                  CYCLES (次)
                </span>

              </div>

              <p
                id="detail-birth-info"
                class="text-[10px] text-gray-500 mt-1 leading-normal break-words"
              >
                自出生日期起算，依成人平靜狀態每分鐘 15 次呼吸即時動態估算
              </p>

            </div>

            <div>
              <span class="text-gray-500 text-[10px] block">
                ECONOMIC OUTPUT (產出價值)
              </span>
              <span id="detail-output-counter" class="font-bold text-emerald-400">
                0.000 KG/H (NULL)
              </span>
            </div>

          </div>

          <div>
            <span class="text-gray-500 text-[10px] block mb-1">
              SYSTEM CLAUSE (授權條款)
            </span>

            <p class="text-gray-400 leading-relaxed text-[11px] font-sans">
              本體因無法忍受空閒狀態產生之罪惡感，現已將「維持最低限度生物性呼吸」之權限，完全移交予代理個案執行。
            </p>
          </div>

        </div>

      </div>

      <div class="bg-gray-950/90 rounded-lg p-6 border border-gray-800/80 relative">

        <span class="text-[10px] font-mono text-gray-500 tracking-widest absolute top-3 right-4">
          STATEMENT ARCHIVE
        </span>

        <h3 class="text-xs font-mono text-gray-400 tracking-wider mb-3">
          【本體焦慮自白紀錄】
        </h3>

        <blockquote
          id="detail-statement"
          class="text-sm md:text-base text-gray-200 leading-relaxed font-sans italic border-l-2 border-emerald-500/60 pl-4 py-1"
        >
          "---"
        </blockquote>

      </div>

      <div class="bg-gray-950/90 rounded-lg p-6 border border-gray-800/80 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 font-mono">
        <div>
          <span class="text-[10px] text-emerald-400 tracking-widest block mb-1">
            OFFICIAL SIGNATURE / 官方授權簽署紀錄
          </span>
          <p class="text-[11px] text-gray-500 font-sans">
            茲證明本檔案已完成代理化程序，本體與裝置之生物權限正式移交。
          </p>
        </div>

        <div class="flex items-center justify-center min-w-[160px] h-[120px] relative overflow-hidden shrink-0">
          <img id="detail-signature-img" src="" alt="Principal Signature" class="max-h-28 max-w-[180px] object-contain opacity-95 mix-blend-screen hidden">
          <span id="detail-signature-placeholder" class="text-[10px] text-gray-600 tracking-widest">
            PENDING SEAL
          </span>
        </div>
      </div>

    </article>

  </main>

  <!-- 作品理念視窗 -->
  <div
    id="concept-modal"
    class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/85 backdrop-blur-md modal-backdrop hidden-modal"
  >

    <div class="bg-[#0b0d10] max-w-2xl w-full max-h-[85vh] rounded-xl border-0 p-6 md:p-8 overflow-y-auto space-y-6 text-sm relative shadow-2xl modal-content">

      <div class="border-b border-gray-800/60 pb-4">

        <span class="text-[10px] font-mono text-emerald-500 tracking-widest block">
          CURATORIAL STATEMENT
        </span>

        <h2 class="text-xl font-bold text-white font-mono">
          《理想狀態》作品理念
        </h2>

      </div>

      <div class="space-y-5 text-gray-300 font-sans leading-relaxed text-xs md:text-sm">

        <section class="space-y-1.5">

          <h3 class="text-emerald-500 font-mono font-bold text-xs">
            01. 假期的罪惡感
          </h3>

          <p class="text-gray-400">
            在這個高度追求效率與生產力的當代社會中，「空閒」逐漸轉化為一種罪惡感的來源。這件作品源自於我在假期中的一段空閒時間，當時我意識到，在這段毫無自我價值提升的時光裡，我的內心竟不斷萌生出一種「時間正被浪費」的焦慮與罪惡感。這樣的狀態迫使我開始思考：「我們是不是習慣用產出與結果，來證明一個人的價值？」「如果一個人做某件事卻沒有產出任何價值，它的存在是否仍值得被承認？」
          </p>

        </section>

        <section class="space-y-1.5">

          <h3 class="text-emerald-500 font-mono font-bold text-xs">
            02. 生產一件代替我停止生產的作品
          </h3>

          <p class="text-gray-400">
            對我而言，創作原本是一種創造價值與實現自我的方式。然而在過往的經驗中，當我沒有持續創作或完成任何事情時，便極易陷入生產力恐慌。為了進一步探討這種焦慮，我邀請了另外三位與我有類似經驗的人共同參與這件作品：既然創作對我來說是一種創造價值的行為，那麼，我們是否能「生產一件代替我們休息、只是在呼吸而沒有實際產出的作品」？我試圖透過這樣的矛盾，讓作品暫時代替我們完成一次真正的「休息」。它不需要完成任何事情，也不以產出任何結果為目的，只是不斷地呼吸，將原本被我視為缺乏價值的空白時間，轉化為作品本身的一部分。
          </p>

        </section>

        <section class="space-y-1.5">

          <h3 class="text-emerald-500 font-mono font-bold text-xs">
            03. 邀請與裝置代理
          </h3>

          <p class="text-gray-400">
            我邀請另外三位與我有相同經驗感受的人進入本次創作，作品由四件臉部雕塑組成，並將我與參與者的個人特徵（頭髮）剝離，僅留存五官結構。為了將「呼吸」視覺化，我採用氣動裝置搭配塑膠薄膜包裹雕塑，利用裝置抽氣與放氣的特性，模擬人頭套著塑膠袋呼吸時的膨脹與收縮，進而製造雕塑正在呼吸的錯覺。藉由這次的邀請參與，作品進一步延伸出四組系列檔案。這些檔案具體記錄了：本體與代理裝置的視覺對比、個案焦慮指數、自出生起算之生命累積總呼吸數估算，以及各自的「本體焦慮自白紀錄」，構成一份份完整的個人焦慮檔案與系統診斷報告。
          </p>

        </section>

        <section class="space-y-1.5">

          <h3 class="text-emerald-500 font-mono font-bold text-xs">
            04. 現場觀眾的參與與延伸檔案
          </h3>

          <p class="text-gray-400">
            除了既有的四組委託人檔案之外，本檔案系統也向現場觀眾開放即時參與，讓觀者得以自行登錄並建立屬於自己的延伸病例檔案。觀眾可透過手中的裝置寫下面對空閒與無產出狀態時的「本體焦慮自白紀錄」。這些由現場觀眾自主提交的病例，會即時同步匯入《理想狀態》的公共檔案目錄之中，使這份關於生產力焦慮與呼吸存在證明的臨床紀錄，隨著每一位觀者的加入而不斷擴張。
          </p>

        </section>

      </div>

      <div
        class="concept-modal-footer border-t border-gray-800/60 pt-4"
        style="display:flex !important;justify-content:center !important;align-items:center !important;width:100% !important;"
      >

        <button
          id="concept-close-button"
          type="button"
          onclick="closeConceptModal()"
          class="text-xs font-mono shadow-sm transition-all"
          style="background-color:#ffffff !important;color:#000000 !important;-webkit-text-fill-color:#000000 !important;border:1px solid #ffffff !important;margin-left:auto !important;margin-right:auto !important;"
        >
          關閉視窗 (CLOSE)
        </button>

      </div>

    </div>

  </div>

  <footer class="border-t border-gray-800/60 mt-16 pt-6 text-center md:flex justify-between items-center text-xs text-gray-600 font-mono">

    <p>© SHAOQI SU. ALL RIGHTS RESERVED.</p>

    <p class="mt-2 md:mt-0">
      ARTWORK BY SHAOQI SU
    </p>

  </footer>

</div>

<script>
/* Firebase Realtime Database 跨裝置即時同步設定 */
const FIREBASE_DB_URL = "https://ideal-state-default-rtdb.asia-southeast1.firebasedatabase.app";
const FIREBASE_CASES_ENDPOINT = `${FIREBASE_DB_URL}/visitor_cases.json`;

/* 原始四位個案資料 */
const BASE_PARTICIPANTS = [
  {
    id: "01",
    name: "蘇Ｏ齊",
    role: "代理呼吸員 01 號",
    target: "蘇Ｏ齊 (本體)",
    anxiety: "87.4% (極高)",
    birthDate: "2005-02-26",
    imageUrl: "https://cdn.phototourl.com/member/2026-10-25-96d3b302-64c7-4d0a-92c3-dae869f734ea.jpg",
    deviceImageUrl: "https://cdn.phototourl.com/member/2026-10-27-116824ad-2295-48ab-bfc1-92ff1656d47d.jpg",
    signatureUrl: "https://cdn.phototourl.com/member/2026-11-12-7dcaeab3-e243-42a6-b786-45621df6d49b.png",
    statement: "我發現自己根本無法忍受純粹的放空。只要一停下來，那種『時間正被浪費』的罪惡感就會把我吞噬。我停不下來——我需要讓腦袋不停的思考（詳情請至作品理念）。"
  },
  {
    id: "02",
    name: "張Ｏ禎",
    role: "代理呼吸員 02 號",
    target: "張Ｏ禎 (本體)",
    anxiety: "91.2% (極高)",
    birthDate: "2005-07-10",
    imageUrl: "https://cdn.phototourl.com/member/2026-10-25-09d06678-a47a-49da-b2bb-c6197ce8a8ea.jpg",
    deviceImageUrl: "https://cdn.phototourl.com/member/2026-11-12-7e79f8bf-8331-4a21-bd47-52142a9de442.webp",
    signatureUrl: "https://cdn.phototourl.com/member/2026-11-17-5d5e831c-b031-4cda-b5ec-45c043ea4136.jpg",
    statement: "只要超過半天沒有打開繪圖軟體或思考進度，我就會產生一種即將被同齡人徹底淘汰的恐慌。明明身體已經累到極限，但大腦還是強迫性地尋找事情做。我需要這台機器幫我把『休息』當成一項公事來認真執行。"
  },
  {
    id: "03",
    name: "林Ｏ岑",
    role: "代理呼吸員 03 號",
    target: "林Ｏ岑 (本體)",
    anxiety: "79.8% (高)",
    birthDate: "2005-06-09",
    imageUrl: "https://cdn.phototourl.com/member/2026-10-25-b70cd799-0bbf-4737-a890-d4c0962fe741.jpg",
    deviceImageUrl: "https://cdn.phototourl.com/member/2026-10-27-4e230bf1-0d8a-429f-b28d-9e4734ace26d.jpg",
    signatureUrl: "",
    statement: "壓力來時，既要照常過日子，又要安撫腦中的聲音，悶得說不出口，像梅雨天關著窗的房間，連呼吸都潮濕沉重。"
  },
  {
    id: "04",
    name: "陳Ｏ騏",
    role: "代理呼吸員 04 號",
    target: "陳Ｏ騏 (本體)",
    anxiety: "20.0%~101.0% ",
    birthDate: "2005-01-13",
    imageUrl: "https://cdn.phototourl.com/member/2026-11-12-7b3841fa-5cb3-4af4-a31d-fefcb0d22eb6.jpg",
    deviceImageUrl: "https://cdn.phototourl.com/member/2026-11-12-8b5daa41-c18f-4551-8ce9-0cd83800cb70.jpg",
    signatureUrl: "https://cdn.phototourl.com/member/2026-11-12-138664ec-ffa6-434b-9a84-e752704cabef.png",
    statement: "檔案建立中，等待資料輸入..."
  }
];

const VISITOR_STORAGE_KEY = "IDEAL_STATE_VISITOR_CASES_2026";
const DEVICE_TOKEN_KEY = "IDEAL_STATE_DEVICE_TOKEN_2026";
const LEGACY_VISITOR_KEYS = [
  "IDEAL_STATE_VISITOR_CASES_2026",
  "IDEAL_STATE_VISITOR_PHOTOS_2026",
  "VISITOR_CASES_BACKUP"
];
const EDIT_WINDOW_MS = 3 * 60 * 1000;

let toastTimeout = null;

function showToast(message) {
  const toastEl = document.getElementById('system-toast');
  const textEl = document.getElementById('system-toast-text');
  if (!toastEl || !textEl) return;
  textEl.innerText = message;
  toastEl.classList.remove('hidden');
  if (toastTimeout) clearTimeout(toastTimeout);
  toastTimeout = setTimeout(() => {
    hideToast();
  }, 4500);
}

function hideToast() {
  const toastEl = document.getElementById('system-toast');
  if (toastEl) toastEl.classList.add('hidden');
}

function hasFirebaseConfigured() {
  return Boolean(
    FIREBASE_DB_URL &&
    FIREBASE_DB_URL.startsWith("https://")
  );
}

function getDeviceToken() {
  try {
    let token = localStorage.getItem(DEVICE_TOKEN_KEY);
    if (!token) {
      token = "DEV-" + Date.now().toString(36) + "-" + Math.random().toString(36).slice(2, 10);
      localStorage.setItem(DEVICE_TOKEN_KEY, token);
    }
    return token;
  } catch (err) {
    return "DEV-SESSION-TEMP";
  }
}

const CURRENT_DEVICE_TOKEN = getDeviceToken();

/* 影像狀態變數 */
let rawMonochromeBaseDataUrl = "";
let capturedPhotoDataUrl = "";
let isPrivacyBlurEnabled = false;
let editingCaseId = null;
let currentDetailCaseId = null;
let lastRenderedSignature = "";

let VISITOR_PARTICIPANTS = loadLocalVisitorCases();
let breathTimer = null;

function formatImageSrc(url) {
  if (!url || typeof url !== "string") return "";
  const trimmed = url.trim();
  if (!trimmed) return "";
  if (trimmed.startsWith("data:image/")) {
    return trimmed;
  }
  if (trimmed.startsWith("http://") || trimmed.startsWith("https://")) {
    return trimmed;
  }
  return "";
}

function handleCardImageError(imgEl, fallbackUrl) {
  if (!imgEl) return;
  if (!imgEl.dataset.retried && fallbackUrl) {
    imgEl.dataset.retried = "1";
    const cleanUrl = fallbackUrl.split("?")[0];
    imgEl.src = `${cleanUrl}?retry=${Date.now()}`;
    return;
  }
  imgEl.style.opacity = "0";
}

function recoverVisitorCaseRecord(item) {
  if (!item || typeof item !== "object") return null;
  const recoveredImg =
    formatImageSrc(item.imageUrl) ||
    formatImageSrc(item.deviceImageUrl) ||
    formatImageSrc(item.rawImageUrl) ||
    formatImageSrc(item.photoUrl) ||
    formatImageSrc(item.portraitUrl) ||
    formatImageSrc(item.avatarUrl) ||
    "";

  return {
    ...item,
    imageUrl: recoveredImg,
    rawImageUrl: recoveredImg,
    deviceImageUrl: recoveredImg
  };
}

function collectLegacyVisitorPhotosMap() {
  const map = new Map();
  LEGACY_VISITOR_KEYS.forEach(key => {
    try {
      const raw = localStorage.getItem(key);
      if (!raw) return;
      const parsed = JSON.parse(raw);
      if (Array.isArray(parsed)) {
        parsed.forEach(entry => {
          if (!entry) return;
          const idKey = String(entry.id || "");
          const photo =
            formatImageSrc(entry.imageUrl) ||
            formatImageSrc(entry.deviceImageUrl) ||
            formatImageSrc(entry.rawImageUrl) ||
            formatImageSrc(entry.photoUrl) ||
            "";
          if (idKey && photo && !map.has(idKey)) {
            map.set(idKey, photo);
          }
        });
      } else if (parsed && typeof parsed === "object") {
        Object.entries(parsed).forEach(([k, v]) => {
          const photo = typeof v === "string" ? formatImageSrc(v) : resolveVisitorPhotoSrc(v);
          if (k && photo && !map.has(String(k))) {
            map.set(String(k), photo);
          }
        });
      }
    } catch (e) {
      // 忽略單一舊鍵解析錯誤
    }
  });
  return map;
}

function resolveVisitorPhotoSrc(p) {
  if (!p) return "";
  return (
    formatImageSrc(p.imageUrl) ||
    formatImageSrc(p.deviceImageUrl) ||
    formatImageSrc(p.rawImageUrl) ||
    formatImageSrc(p.photoUrl) ||
    formatImageSrc(p.portraitUrl) ||
    formatImageSrc(p.avatarUrl) ||
    formatImageSrc(p.originalImageUrl) ||
    formatImageSrc(p.backupImageUrl) ||
    ""
  );
}

function loadLocalVisitorCases() {
  const photoMap = collectLegacyVisitorPhotosMap();
  const allKeys = Array.from(new Set([...LEGACY_VISITOR_KEYS, ...Object.keys(localStorage)]));
  allKeys.forEach(key => {
    try {
      const raw = localStorage.getItem(key);
      if (!raw) return;
      const parsed = JSON.parse(raw);
      if (Array.isArray(parsed)) {
        parsed.forEach(entry => {
          if (!entry) return;
          const idKey = String(entry.id || "");
          const photo = resolveVisitorPhotoSrc(entry);
          if (idKey && photo && !photoMap.has(idKey)) {
            photoMap.set(idKey, photo);
          }
        });
      }
    } catch (e) {
      // 略過非 JSON 項目
    }
  });

  try {
    const raw = localStorage.getItem(VISITOR_STORAGE_KEY);
    if (!raw) return [];
    const parsed = JSON.parse(raw);
    if (!Array.isArray(parsed)) return [];
    return parsed.map(rawEntry => {
      const recovered = recoverVisitorCaseRecord(rawEntry);
      if (!recovered) return null;
      const fallbackPhoto = photoMap.get(String(recovered.id));
      if (!resolveVisitorPhotoSrc(recovered) && fallbackPhoto) {
        recovered.imageUrl = fallbackPhoto;
        recovered.rawImageUrl = fallbackPhoto;
        recovered.deviceImageUrl = fallbackPhoto;
      }
      return recovered;
    }).filter(Boolean);
  } catch (err) {
    return [];
  }
}

function saveLocalVisitorCases(cases) {
  try {
    const normalized = Array.isArray(cases) ? cases.map(recoverVisitorCaseRecord).filter(Boolean) : [];
    localStorage.setItem(VISITOR_STORAGE_KEY, JSON.stringify(normalized));
  } catch (err) {
    // 若容量不足則略過本機快取，以雲端為主
  }
}

/* 從 Firebase 雲端讀取所有觀眾病例並復原既有照片欄位 */
async function fetchCloudVisitorCases() {
  const localFallback = loadLocalVisitorCases();
  const legacyPhotoMap = collectLegacyVisitorPhotosMap();

  if (!hasFirebaseConfigured()) {
    return localFallback;
  }

  try {
    const res = await fetch(`${FIREBASE_CASES_ENDPOINT}?t=${Date.now()}`, {
      cache: "no-store"
    });
    if (!res.ok) throw new Error("Firebase fetch failed");
    const data = await res.json();

    if (!data) return localFallback;

    let list = [];
    if (Array.isArray(data)) {
      list = data.filter(Boolean);
    } else if (typeof data === "object") {
      list = Object.values(data).filter(Boolean);
    }

    const localById = new Map(localFallback.map(item => [String(item.id), item]));

    list = list.map(rawItem => {
      const recovered = recoverVisitorCaseRecord(rawItem);
      if (!recovered) return null;
      if (!resolveVisitorPhotoSrc(recovered)) {
        const localMatch = localById.get(String(recovered.id));
        const localPhoto = resolveVisitorPhotoSrc(localMatch) || legacyPhotoMap.get(String(recovered.id)) || "";
        if (localPhoto) {
          recovered.imageUrl = localPhoto;
          recovered.rawImageUrl = localPhoto;
          recovered.deviceImageUrl = localPhoto;
        }
      }
      return recovered;
    }).filter(Boolean);

    list.sort((a, b) => {
      const na = parseInt(a.id, 10) || 0;
      const nb = parseInt(b.id, 10) || 0;
      return na - nb;
    });

    return list;
  } catch (err) {
    return localFallback;
  }
}

/* 將所有觀眾病例同步寫入 Firebase 雲端 */
async function syncVisitorCasesToCloud(cases) {
  const normalized = Array.isArray(cases) ? cases.map(recoverVisitorCaseRecord).filter(Boolean) : [];
  saveLocalVisitorCases(normalized);

  if (!hasFirebaseConfigured()) {
    return { ok: false, reason: "NOT_CONFIGURED" };
  }

  try {
    const payload = normalized.length === 0 ? null : normalized;
    const res = await fetch(FIREBASE_CASES_ENDPOINT, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(payload)
    });
    if (!res.ok) throw new Error("Firebase PUT failed");
    return { ok: true };
  } catch (err) {
    return { ok: false, reason: "NETWORK_ERROR" };
  }
}

/* 取得所有個案（原始四位 + 現場觀眾） */
function getAllParticipants() {
  return [...BASE_PARTICIPANTS, ...VISITOR_PARTICIPANTS];
}

function getVisitorDisplayId(caseItem, index = -1) {
  if (!caseItem) return "01";
  const resolvedIndex = index >= 0
    ? index
    : VISITOR_PARTICIPANTS.findIndex(p => p.id === caseItem.id);
  if (resolvedIndex >= 0) {
    return String(resolvedIndex + 1).padStart(2, '0');
  }
  const n = parseInt(caseItem.id, 10);
  if (!isNaN(n) && n > BASE_PARTICIPANTS.length) {
    return String(n - BASE_PARTICIPANTS.length).padStart(2, '0');
  }
  return String(caseItem.id || "01").padStart(2, '0');
}

function getVisitorCaseNumber(caseItem, index = -1) {
  if (!caseItem) return String(BASE_PARTICIPANTS.length + 1).padStart(2, '0');
  const resolvedIndex = index >= 0
    ? index
    : VISITOR_PARTICIPANTS.findIndex(p => p.id === caseItem.id);
  if (resolvedIndex >= 0) {
    return String(BASE_PARTICIPANTS.length + resolvedIndex + 1).padStart(2, '0');
  }
  const n = parseInt(caseItem.id, 10);
  if (!isNaN(n)) {
    return String(n).padStart(2, '0');
  }
  return String(BASE_PARTICIPANTS.length + 1).padStart(2, '0');
}

function getVisitorDisplayRole(caseItem, index = -1) {
  const displayId = getVisitorDisplayId(caseItem, index);
  return `現場延伸病例 ${displayId} 號`;
}

function isVisitorCaseById(id) {
  return VISITOR_PARTICIPANTS.some(p => p.id === id);
}

/* 判斷目前裝置是否在 3 分鐘內建立該觀眾病例 */
function canEditOrDeleteCase(caseItem) {
  if (!caseItem || !caseItem.ownerToken || !caseItem.createdAt) return false;
  if (caseItem.ownerToken !== CURRENT_DEVICE_TOKEN) return false;
  return (Date.now() - Number(caseItem.createdAt)) < EDIT_WINDOW_MS;
}

function getRemainingEditSeconds(caseItem) {
  if (!canEditOrDeleteCase(caseItem)) return 0;
  const remainMs = EDIT_WINDOW_MS - (Date.now() - Number(caseItem.createdAt));
  return Math.max(0, Math.ceil(remainMs / 1000));
}

function formatSecondsMMSS(totalSeconds) {
  const mins = Math.floor(totalSeconds / 60);
  const secs = totalSeconds % 60;
  return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
}

/* 更新觀眾病例數量與下一個編號預覽 */
function updateCaseCountersUI() {
  const countLabel = document.getElementById('visitor-total-label');
  const nextPreview = document.getElementById('next-case-preview');

  if (countLabel) {
    const countStr = String(VISITOR_PARTICIPANTS.length).padStart(2, '0');
    countLabel.innerText = `VISITOR CASES: ${countStr}`;
  }

  if (nextPreview) {
    if (editingCaseId) {
      const editIdx = VISITOR_PARTICIPANTS.findIndex(p => p.id === editingCaseId);
      const editCaseNum = editIdx >= 0 ? getVisitorCaseNumber(VISITOR_PARTICIPANTS[editIdx], editIdx) : editingCaseId;
      nextPreview.innerText = `EDITING ID: CASE #${editCaseNum}`;
    } else {
      const nextNum = String(BASE_PARTICIPANTS.length + VISITOR_PARTICIPANTS.length + 1).padStart(2, '0');
      nextPreview.innerText = `NEXT ID: CASE #${nextNum}`;
    }
  }
}

/* 作品理念 */
function openConceptModal() {
  const modal = document.getElementById('concept-modal');
  modal.classList.remove('hidden-modal');
  modal.classList.add('active-modal');
}

function closeConceptModal() {
  const modal = document.getElementById('concept-modal');
  modal.classList.remove('active-modal');
  modal.classList.add('hidden-modal');
}

document.getElementById('concept-modal').addEventListener('click', function(e) {
  if (e.target === this) {
    closeConceptModal();
  }
});

/* 產出數值 */
function startOutputFluctuationWithPause() {
  const headerOutputEl = document.getElementById('header-output-counter');
  const detailOutputEl = document.getElementById('detail-output-counter');

  let isPaused = false;

  function updateCounter() {
    if (isPaused) return;

    if (Math.random() < 0.15) {
      isPaused = true;

      if (headerOutputEl) {
        headerOutputEl.innerText = "0.000 KG/H";
      }

      if (detailOutputEl) {
        detailOutputEl.innerText = "0.000 KG/H (NULL)";
      }

      setTimeout(() => {
        isPaused = false;
      }, 500);

    } else {
      const d1 = Math.floor(Math.random() * 10);
      const d2 = Math.floor(Math.random() * 10);
      const d3 = Math.floor(Math.random() * 10);
      const d4 = Math.floor(Math.random() * 10);

      const randomVal = `${d1}.${d2}${d3}${d4}`;

      if (headerOutputEl) {
        headerOutputEl.innerText = `${randomVal} KG/H`;
      }

      if (detailOutputEl) {
        detailOutputEl.innerText = `${randomVal} KG/H (NULL)`;
      }
    }
  }

  setInterval(updateCounter, 80);
}

/* 呼吸計數 */
function calculateLifetimeBreaths(birthDateStr) {
  const birthDate = new Date(birthDateStr);
  const now = new Date();
  const diffInSeconds = (now - birthDate) / 1000;
  return Math.max(0, Math.floor(diffInSeconds * 0.25));
}

function startBreathCounter(birthDateStr) {
  if (breathTimer) {
    clearInterval(breathTimer);
  }

  let currentBreaths = calculateLifetimeBreaths(birthDateStr);
  const counterEl = document.getElementById('detail-breath-counter');
  counterEl.innerText = currentBreaths.toLocaleString();

  breathTimer = setInterval(() => {
    currentBreaths += 1;
    counterEl.innerText = currentBreaths.toLocaleString();
  }, 4000);
}

/* 原始四位委託人圖片對比滑桿 */
function updateImgDimensions() {
  const sliderContainer = document.getElementById('detail-slider-container');
  const principalImg = document.getElementById('detail-principal-img');

  if (sliderContainer && principalImg) {
    principalImg.style.width = `${sliderContainer.clientWidth}px`;
    principalImg.style.height = `${sliderContainer.clientHeight}px`;
  }
}

function initComparisonSlider() {
  const slider = document.getElementById('comparison-slider');
  const clipWrapper = document.getElementById('slider-clip-wrapper');
  const sliderLine = document.getElementById('slider-line');
  const badgePrincipal = document.getElementById('badge-principal');
  const badgeDevice = document.getElementById('badge-device');

  const badgeBase =
    "absolute bottom-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none";

  function updateSlider(val) {
    clipWrapper.style.width = `${val}%`;
    sliderLine.style.left = `${val}%`;

    const principalActive = val > 55;
    const deviceActive = val < 45;

    badgePrincipal.className =
      `${badgeBase} left-2.5 ` +
      (principalActive ? "badge-emerald-active" : "badge-inactive");

    badgeDevice.className =
      `${badgeBase} right-2.5 ` +
      (deviceActive ? "badge-emerald-active" : "badge-inactive");
  }

  slider.addEventListener('input', e => {
    updateSlider(e.target.value);
  });

  window.addEventListener('resize', updateImgDimensions);
  updateImgDimensions();
}

/* 切換詳情頁左側顯示模式：觀眾病例隱藏滑桿並顯示獨立觀眾照片，原始四位還原滑桿對比 */
function applyDetailLeftMode(isVisitorCase) {
  const visitorImg = document.getElementById('detail-visitor-img');
  const clipWrapper = document.getElementById('slider-clip-wrapper');
  const principalImg = document.getElementById('detail-principal-img');
  const deviceImg = document.getElementById('detail-device-img');
  const sliderInput = document.getElementById('comparison-slider');
  const sliderLine = document.getElementById('slider-line');
  const badgePrincipal = document.getElementById('badge-principal');
  const badgeDevice = document.getElementById('badge-device');

  if (isVisitorCase) {
    if (clipWrapper) clipWrapper.style.display = 'none';
    if (deviceImg) deviceImg.style.display = 'none';
    if (sliderInput) sliderInput.style.display = 'none';
    if (sliderLine) sliderLine.style.display = 'none';
    if (badgePrincipal) badgePrincipal.style.display = 'none';
    if (badgeDevice) badgeDevice.style.display = 'none';
  } else {
    if (visitorImg) {
      visitorImg.src = '';
      visitorImg.classList.add('hidden');
    }
    if (clipWrapper) {
      clipWrapper.style.display = '';
      clipWrapper.style.width = '50%';
    }
    if (deviceImg) {
      deviceImg.style.display = '';
    }
    if (sliderInput) {
      sliderInput.style.display = '';
      sliderInput.value = 50;
    }
    if (sliderLine) {
      sliderLine.style.display = '';
      sliderLine.style.left = '50%';
    }
    if (badgePrincipal) {
      badgePrincipal.style.display = '';
      badgePrincipal.className =
        "absolute bottom-2.5 left-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none badge-inactive";
    }
    if (badgeDevice) {
      badgeDevice.style.display = '';
      badgeDevice.className =
        "absolute bottom-2.5 right-2.5 text-[9px] font-mono backdrop-blur px-2.5 py-1 rounded transition-all duration-300 z-20 pointer-events-none badge-inactive";
    }
    if (principalImg) {
      principalImg.style.maxWidth = 'none';
    }
    updateImgDimensions();
  }
}

/* 單張卡片模板 */
function buildCaseCardHTML(p, isVisitor = false, visitorIndex = -1) {
  const imgSrc = isVisitor ? resolveVisitorPhotoSrc(p) : formatImageSrc(p.imageUrl);
  const deviceImgSrc = formatImageSrc(p.deviceImageUrl || p.imageUrl);
  const editable = isVisitor && canEditOrDeleteCase(p);
  const displayCaseId = isVisitor ? getVisitorCaseNumber(p, visitorIndex) : p.id;
  const displayRole = isVisitor ? getVisitorDisplayRole(p, visitorIndex) : p.role;
  const detailTargetKey = isVisitor ? `visitor:${p.id}` : p.id;

  const deviceImgHTML = (!isVisitor && deviceImgSrc)
    ? `<img src="${deviceImgSrc}" alt="Proxy Device" loading="eager" decoding="async" onerror="handleCardImageError(this, '${deviceImgSrc}')" class="w-full h-full object-cover bg-gray-950 opacity-0 group-hover:opacity-100 mobile-auto-fade group-hover:scale-105 transition-all duration-500 absolute inset-0 z-10">`
    : '';

  const mediaHTML = imgSrc
    ? `<img src="${imgSrc}" alt="${p.name}" loading="eager" decoding="async" onerror="handleCardImageError(this, '${imgSrc}')" class="w-full h-full object-cover bg-gray-950 group-hover:scale-105 transition-all duration-500 absolute inset-0 z-0">${deviceImgHTML}`
    : `<div class="flex flex-col items-center space-y-2 text-gray-700">
        <svg class="w-10 h-10 sm:w-12 sm:h-12 stroke-gray-700" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
        </svg>
        <span class="text-[10px] font-mono text-gray-600">NO SCAN DATA</span>
      </div>`;

  const ownerControlsHTML = editable
    ? `<div class="grid grid-cols-2 gap-1.5 w-full" onclick="event.stopPropagation()">
        <button type="button" onclick="startEditVisitorCase('${p.id}')" class="w-full py-1 sm:py-1.5 rounded bg-white/95 text-black font-mono text-[10px] sm:text-xs font-bold hover:bg-gray-200 transition-colors shadow text-center cursor-pointer">✎ 修改</button>
        <button type="button" onclick="deleteVisitorCase('${p.id}')" class="w-full py-1 sm:py-1.5 rounded bg-red-950/90 text-red-400 border border-red-700/70 font-mono text-[10px] sm:text-xs font-bold hover:bg-red-800 hover:text-white transition-colors shadow text-center cursor-pointer">✕ 刪除</button>
      </div>`
    : '';

  return `
    <div
      onclick="showDetail('${detailTargetKey}')"
      class="glass-card rounded-xl overflow-hidden cursor-pointer flex flex-col justify-between relative group"
    >
      <div class="aspect-[3/4] w-full bg-gray-950 relative overflow-hidden flex items-center justify-center">
        <div class="absolute top-2.5 left-2.5 sm:top-3 sm:left-3 pointer-events-none z-30">
          <span class="text-[10px] sm:text-xs font-mono text-white tracking-wider font-bold bg-black/70 backdrop-blur-md px-2 sm:px-2.5 py-0.5 sm:py-1 rounded shadow-lg inline-block transition-all duration-300 group-hover:bg-transparent group-hover:backdrop-blur-none group-hover:shadow-none mobile-auto-bg-fade">
            CASE #${displayCaseId}
          </span>
        </div>

        ${mediaHTML}

        <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/30 to-transparent pointer-events-none z-20"></div>

        <div class="absolute ${editable ? 'bottom-[74px] sm:bottom-[82px]' : 'bottom-12 sm:bottom-14'} left-3 right-3 sm:left-4 sm:right-4 space-y-0.5 pointer-events-none z-30 transition-all duration-300">
          <h3 class="text-sm sm:text-lg font-bold text-white tracking-wide truncate">
            ${p.name}
          </h3>
          <p class="text-[10px] sm:text-xs text-gray-300 font-mono truncate">
            ${displayRole}
          </p>
        </div>

        <div class="absolute bottom-2.5 left-2.5 right-2.5 sm:bottom-3 sm:left-3 sm:right-3 z-30 flex flex-col gap-1.5">
          ${ownerControlsHTML}
          <button class="btn-neon w-full py-1.5 sm:py-2 text-[10px] sm:text-xs rounded shadow-lg flex items-center justify-center space-x-1 cursor-pointer">
            <span>檢視個案診斷</span>
            <span>→</span>
          </button>
        </div>
      </div>
    </div>
  `;
}

/* 渲染原始四位卡片與現場觀眾獨立卡片 */
function renderPortalCards(force = false) {
  const sig = VISITOR_PARTICIPANTS.map(p => `${p.id}|${p.name}|${p.statement}|${Boolean(resolveVisitorPhotoSrc(p))}`).join("||");
  if (!force && sig === lastRenderedSignature) {
    updateCaseCountersUI();
    return;
  }
  lastRenderedSignature = sig;

  const baseContainer = document.getElementById('cards-container');
  const visitorContainer = document.getElementById('visitor-cards-container');

  if (baseContainer) {
    baseContainer.innerHTML = BASE_PARTICIPANTS.map(p => buildCaseCardHTML(p, false)).join('');
  }

  if (visitorContainer) {
    if (VISITOR_PARTICIPANTS.length === 0) {
      visitorContainer.innerHTML = `
        <div class="col-span-full bg-gray-950/60 border border-dashed border-gray-800 rounded-xl p-8 text-center font-mono space-y-1.5">
          <span class="text-xs text-gray-400 block">
            [STANDBY] 尚未收錄現場觀眾病例檔案
          </span>
          <span class="text-[11px] text-gray-600 font-sans block">
            請於下方「書寫你的病例檔案」登錄你的焦慮自白，建立後將跨裝置同步顯示於此
          </span>
        </div>
      `;
    } else {
      visitorContainer.innerHTML = VISITOR_PARTICIPANTS.map((p, idx) => buildCaseCardHTML(p, true, idx)).join('');
    }
  }

  updateCaseCountersUI();
}

/* 觀眾影像處理：黑白灰階 + 曝光微調 + 可切換隱私模糊保護 */
function applyOriginalVisitorMonochromeTone(ctx, width, height) {
  const imgData = ctx.getImageData(0, 0, width, height);
  const data = imgData.data;

  const exposureGain = 1.18;
  const contrastFactor = 1.08;

  for (let i = 0; i < data.length; i += 4) {
    const r = data[i];
    const g = data[i + 1];
    const b = data[i + 2];

    let lum = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
    lum = lum * exposureGain;
    lum = (lum - 0.5) * contrastFactor + 0.5;
    lum = Math.max(0, Math.min(1, lum));

    const val = Math.round(lum * 255);
    data[i] = val;
    data[i + 1] = val;
    data[i + 2] = val;
  }

  ctx.putImageData(imgData, 0, 0);
}

function processVisitorPhotoTone(file) {
  return new Promise((resolve) => {
    const reader = new FileReader();
    reader.onload = function(event) {
      const img = new Image();
      img.onload = function() {
        const maxDim = 720;
        let width = img.width;
        let height = img.height;

        if (width > maxDim || height > maxDim) {
          if (width > height) {
            height = Math.round((height * maxDim) / width);
            width = maxDim;
          } else {
            width = Math.round((width * maxDim) / height);
            height = maxDim;
          }
        }

        const outCanvas = document.createElement('canvas');
        outCanvas.width = width;
        outCanvas.height = height;
        const outCtx = outCanvas.getContext('2d');
        outCtx.drawImage(img, 0, 0, width, height);
        applyOriginalVisitorMonochromeTone(outCtx, width, height);

        resolve(outCanvas.toDataURL('image/jpeg', 0.82));
      };
      img.src = event.target.result;
    };
    reader.readAsDataURL(file);
  });
}

function applyPrivacyBlurToDataUrl(sourceDataUrl) {
  return new Promise((resolve) => {
    if (!sourceDataUrl) {
      resolve("");
      return;
    }

    const img = new Image();
    img.onload = function() {
      const canvas = document.createElement('canvas');
      canvas.width = img.width;
      canvas.height = img.height;
      const ctx = canvas.getContext('2d');

      ctx.fillStyle = '#08090a';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      // 依影像短邊計算馬賽克方塊大小，確保高解析度照片也能產生足夠粗大的方塊
      const shortSide = Math.min(canvas.width, canvas.height);
      const blockSize = Math.max(14, Math.round(shortSide / 24));
      const cols = Math.max(1, Math.ceil(canvas.width / blockSize));
      const rows = Math.max(1, Math.ceil(canvas.height / blockSize));

      const mosaicCanvas = document.createElement('canvas');
      mosaicCanvas.width = cols;
      mosaicCanvas.height = rows;
      const mosaicCtx = mosaicCanvas.getContext('2d');
      mosaicCtx.imageSmoothingEnabled = true;
      mosaicCtx.imageSmoothingQuality = 'medium';
      mosaicCtx.drawImage(img, 0, 0, cols, rows);

      // 關閉平滑插值，將縮小的方塊像素放大成硬邊馬賽克
      ctx.save();
      ctx.imageSmoothingEnabled = false;
      ctx.mozImageSmoothingEnabled = false;
      ctx.webkitImageSmoothingEnabled = false;
      ctx.msImageSmoothingEnabled = false;
      ctx.drawImage(mosaicCanvas, 0, 0, cols, rows, 0, 0, canvas.width, canvas.height);
      ctx.restore();

      resolve(canvas.toDataURL('image/jpeg', 0.85));
    };
    img.onerror = function() {
      resolve(sourceDataUrl);
    };
    img.src = sourceDataUrl;
  });
}

async function rebuildOutputPhotoWithPrivacyOption() {
  if (!rawMonochromeBaseDataUrl) {
    capturedPhotoDataUrl = "";
    updatePhotoPreviewUI();
    return "";
  }

  if (!isPrivacyBlurEnabled) {
    capturedPhotoDataUrl = rawMonochromeBaseDataUrl;
    updatePhotoPreviewUI();
    return capturedPhotoDataUrl;
  }

  capturedPhotoDataUrl = await applyPrivacyBlurToDataUrl(rawMonochromeBaseDataUrl);
  updatePhotoPreviewUI();
  return capturedPhotoDataUrl;
}

function updatePhotoPreviewUI() {
  const previewBox = document.getElementById('photo-preview-box');
  const previewImg = document.getElementById('photo-preview-img');
  const previewBadge = document.getElementById('photo-preview-badge');
  const clearBtn = document.getElementById('clear-photo-btn');

  if (!capturedPhotoDataUrl) {
    if (previewBox) previewBox.classList.add('hidden');
    if (clearBtn) clearBtn.classList.add('hidden');
    return;
  }

  if (previewImg) previewImg.src = capturedPhotoDataUrl;
  if (previewBox) previewBox.classList.remove('hidden');
  if (clearBtn) clearBtn.classList.remove('hidden');

  if (previewBadge) {
    previewBadge.innerText = isPrivacyBlurEnabled
      ? "✓ 隱私保護已開啟"
      : "✓ 影像已完成黑白曝光調整";
  }
}

function syncPrivacyBlurButtonUI() {
  const btn = document.getElementById('privacy-blur-toggle');
  const indicator = document.getElementById('privacy-blur-indicator');
  const label = document.getElementById('privacy-blur-label');
  if (!btn || !indicator || !label) return;

  if (isPrivacyBlurEnabled) {
    btn.className = "w-full sm:w-auto px-3.5 py-2 rounded text-xs font-mono inline-flex items-center justify-center gap-2 border transition-all cursor-pointer bg-emerald-950/50 text-emerald-400 border-emerald-500/60";
    indicator.className = "w-2 h-2 rounded-full bg-emerald-400 animate-pulse";
    label.innerText = "隱私模糊保護：已開啟 (臉部已模糊化)";
  } else {
    btn.className = "w-full sm:w-auto px-3.5 py-2 rounded text-xs font-mono inline-flex items-center justify-center gap-2 border transition-all cursor-pointer bg-gray-900/90 text-gray-300 border-gray-700 hover:border-emerald-500/60";
    indicator.className = "w-2 h-2 rounded-full bg-gray-500";
    label.innerText = "隱私模糊保護：關閉 (點擊模糊化臉部)";
  }
}

async function togglePrivacyBlur() {
  isPrivacyBlurEnabled = !isPrivacyBlurEnabled;
  syncPrivacyBlurButtonUI();

  if (rawMonochromeBaseDataUrl) {
    await rebuildOutputPhotoWithPrivacyOption();
  }
}

function clearCapturedPhoto() {
  rawMonochromeBaseDataUrl = "";
  capturedPhotoDataUrl = "";
  const photoInput = document.getElementById('visitor-photo-input');
  const previewBox = document.getElementById('photo-preview-box');
  const previewImg = document.getElementById('photo-preview-img');
  const clearBtn = document.getElementById('clear-photo-btn');

  if (photoInput) photoInput.value = "";
  if (previewImg) previewImg.src = "";
  if (previewBox) previewBox.classList.add('hidden');
  if (clearBtn) clearBtn.classList.add('hidden');
}

async function handleSelectedImageFile(file) {
  if (!file) return;
  const statusEl = document.getElementById('photo-processing-status');
  if (statusEl) statusEl.classList.remove('hidden');

  rawMonochromeBaseDataUrl = await processVisitorPhotoTone(file);
  await rebuildOutputPhotoWithPrivacyOption();

  if (statusEl) statusEl.classList.add('hidden');
}

function startEditVisitorCase(id) {
  const target = VISITOR_PARTICIPANTS.find(p => p.id === id);
  if (!target || !canEditOrDeleteCase(target)) {
    showToast("此病例已超過 3 分鐘修改時限，或非原上傳裝置建立之檔案。");
    renderPortalCards(true);
    return;
  }

  editingCaseId = id;
  showPortal();

  const editIdx = VISITOR_PARTICIPANTS.findIndex(p => p.id === id);
  const editCaseNum = getVisitorCaseNumber(target, editIdx);

  document.getElementById('visitor-statement').value = target.statement || "";
  document.getElementById('visitor-name').value = target.name || "";
  document.getElementById('visitor-birth').value = target.birthDate || "";
  document.getElementById('visitor-anxiety').value = target.anxiety || "";

  isPrivacyBlurEnabled = Boolean(target.privacyBlurEnabled);
  syncPrivacyBlurButtonUI();

  const existingPhoto = resolveVisitorPhotoSrc(target);
  if (target.rawImageUrl || existingPhoto) {
    rawMonochromeBaseDataUrl = target.rawImageUrl || existingPhoto;
    capturedPhotoDataUrl = existingPhoto || target.rawImageUrl;
    updatePhotoPreviewUI();
  } else {
    clearCapturedPhoto();
  }

  document.getElementById('form-mode-title').innerText = `修改病例檔案：CASE #${editCaseNum}`;
  document.getElementById('submit-case-btn-text').innerText = "儲存病例修改";
  document.getElementById('cancel-edit-btn').classList.remove('hidden');
  document.getElementById('visitor-form-feedback').innerText = `正在編輯 CASE #${editCaseNum}（僅限建立後 3 分鐘內修改）`;

  updateCaseCountersUI();

  const formSection = document.getElementById('visitor-case-section');
  if (formSection) {
    formSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
}

function cancelEditMode() {
  editingCaseId = null;
  isPrivacyBlurEnabled = false;
  syncPrivacyBlurButtonUI();
  const form = document.getElementById('visitor-case-form');
  if (form) form.reset();
  clearCapturedPhoto();

  document.getElementById('form-mode-title').innerText = "書寫你的病例檔案";
  document.getElementById('submit-case-btn-text').innerText = "建立並加入觀眾病例";
  document.getElementById('cancel-edit-btn').classList.add('hidden');
  document.getElementById('visitor-form-feedback').innerText = "";
  updateCaseCountersUI();
}

async function deleteVisitorCase(id) {
  const targetIdx = VISITOR_PARTICIPANTS.findIndex(p => p.id === id);
  const target = targetIdx >= 0 ? VISITOR_PARTICIPANTS[targetIdx] : null;
  if (!target || !canEditOrDeleteCase(target)) {
    showToast("此病例已超過 3 分鐘刪除時限，或非原上傳裝置建立之檔案。");
    renderPortalCards(true);
    return;
  }

  const deletedCaseNum = getVisitorCaseNumber(target, targetIdx);
  VISITOR_PARTICIPANTS = VISITOR_PARTICIPANTS.filter(p => p.id !== id);
  const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);

  if (editingCaseId === id) {
    cancelEditMode();
  }

  renderPortalCards(true);

  if (currentDetailCaseId === id || currentDetailCaseId === `visitor:${id}`) {
    showPortal();
  }

  const msg = syncResult.ok
    ? `已從雲端刪除病例 CASE #${deletedCaseNum}（全裝置已同步）。`
    : `已刪除病例 CASE #${deletedCaseNum}（雲端連線異常，已於本機更新）。`;

  showToast(msg);
  const feedback = document.getElementById('visitor-form-feedback');
  if (feedback) {
    feedback.innerText = msg;
  }
}

/* 現場觀眾病例登錄與更新處理 */
function initVisitorForm() {
  const form = document.getElementById('visitor-case-form');
  const feedback = document.getElementById('visitor-form-feedback');
  const photoInput = document.getElementById('visitor-photo-input');

  if (photoInput) {
    photoInput.addEventListener('change', function(e) {
      const file = e.target.files && e.target.files[0];
      handleSelectedImageFile(file);
    });
  }

  if (!form) return;

  form.addEventListener('submit', async function(e) {
    e.preventDefault();

    const statementInput = document.getElementById('visitor-statement').value.trim();
    const nameInput = document.getElementById('visitor-name').value.trim();
    const birthInput = document.getElementById('visitor-birth').value;
    const anxietyInput = document.getElementById('visitor-anxiety').value.trim();

    if (!statementInput) {
      feedback.textContent = "請填寫本體焦慮自白紀錄。";
      return;
    }

    if (!birthInput) {
      feedback.textContent = "請填寫出生日期。";
      return;
    }

    feedback.textContent = "正在同步至雲端資料庫...";

    let finalPhotoDataUrl = capturedPhotoDataUrl;
    if (rawMonochromeBaseDataUrl) {
      finalPhotoDataUrl = await rebuildOutputPhotoWithPrivacyOption();
    }

    if (editingCaseId) {
      const idx = VISITOR_PARTICIPANTS.findIndex(p => p.id === editingCaseId);
      if (idx === -1 || !canEditOrDeleteCase(VISITOR_PARTICIPANTS[idx])) {
        feedback.textContent = "已超過 3 分鐘可修改時限，無法儲存變更。";
        cancelEditMode();
        renderPortalCards(true);
        return;
      }

      const existing = VISITOR_PARTICIPANTS[idx];
      const resolvedName = nameInput || existing.name;
      const resolvedBirth = birthInput;

      VISITOR_PARTICIPANTS[idx] = {
        ...existing,
        name: resolvedName,
        target: `${resolvedName} (本體)`,
        anxiety: anxietyInput || existing.anxiety,
        birthDate: resolvedBirth,
        rawImageUrl: rawMonochromeBaseDataUrl,
        imageUrl: finalPhotoDataUrl,
        deviceImageUrl: finalPhotoDataUrl,
        privacyBlurEnabled: isPrivacyBlurEnabled,
        statement: statementInput
      };

      const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);
      const updatedCaseNum = getVisitorCaseNumber(VISITOR_PARTICIPANTS[idx], idx);
      cancelEditMode();
      renderPortalCards(true);
      feedback.textContent = syncResult.ok
        ? `已更新病例 CASE #${updatedCaseNum}（${resolvedName}）並同步至所有裝置。`
        : `已更新病例 CASE #${updatedCaseNum}（雲端連線異常，已暫存於本機）。`;
      return;
    }

    const latestCloud = await fetchCloudVisitorCases();
    if (latestCloud.length > 0) {
      VISITOR_PARTICIPANTS = latestCloud;
    }

    const nextVisitorSeq = VISITOR_PARTICIPANTS.length + 1;
    const nextVisitorDisplayId = String(nextVisitorSeq).padStart(2, '0');
    const nextOverallCaseNum = String(BASE_PARTICIPANTS.length + nextVisitorSeq).padStart(2, '0');
    const storageId = `V-${Date.now().toString(36)}-${nextVisitorDisplayId}`;
    const resolvedName = nameInput || `現場委託人 #${nextVisitorDisplayId}`;
    const resolvedBirth = birthInput;

    const newCase = {
      id: storageId,
      name: resolvedName,
      role: `現場延伸病例 ${nextVisitorDisplayId} 號`,
      target: `${resolvedName} (本體)`,
      anxiety: anxietyInput || "85.0% (現場登錄)",
      birthDate: resolvedBirth,
      rawImageUrl: rawMonochromeBaseDataUrl,
      imageUrl: finalPhotoDataUrl,
      deviceImageUrl: finalPhotoDataUrl,
      privacyBlurEnabled: isPrivacyBlurEnabled,
      signatureUrl: "",
      statement: statementInput,
      ownerToken: CURRENT_DEVICE_TOKEN,
      createdAt: Date.now()
    };

    VISITOR_PARTICIPANTS = [...VISITOR_PARTICIPANTS, newCase];
    const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);

    isPrivacyBlurEnabled = false;
    syncPrivacyBlurButtonUI();
    renderPortalCards(true);
    form.reset();
    clearCapturedPhoto();

    if (syncResult.ok) {
      feedback.textContent = `已建立病例 CASE #${nextOverallCaseNum}（${resolvedName}）並同步至雲端，3 分鐘內可於本裝置修改或刪除。`;
    } else {
      feedback.textContent = `已建立病例 CASE #${nextOverallCaseNum}（${resolvedName}），雲端連線異常，請確認 Firebase 測試規則權限。`;
    }
  });
}

/* 定期從雲端同步最新病例（每 6 秒一次，且僅在頁面可見時執行以降低手機耗電與卡頓） */
function startCloudSyncPolling() {
  if (!hasFirebaseConfigured()) return;

  setInterval(async () => {
    if (document.hidden || editingCaseId) return;
    const cloudCases = await fetchCloudVisitorCases();
    const nextSig = cloudCases.map(p => `${p.id}|${p.name}|${p.statement}|${Boolean(resolveVisitorPhotoSrc(p))}`).join("||");
    if (nextSig !== lastRenderedSignature) {
      VISITOR_PARTICIPANTS = cloudCases;
      saveLocalVisitorCases(cloudCases);
      renderPortalCards(true);
    }
  }, 6000);
}

/* 定期檢查 3 分鐘編輯時限到期狀態並重新渲染 */
function startEditWindowTicker() {
  setInterval(() => {
    const expiredIds = VISITOR_PARTICIPANTS
      .filter(p => p.ownerToken === CURRENT_DEVICE_TOKEN && p.createdAt)
      .filter(p => (Date.now() - Number(p.createdAt)) >= EDIT_WINDOW_MS)
      .map(p => p.id);

    if (expiredIds.length > 0) {
      if (editingCaseId && expiredIds.includes(editingCaseId)) {
        cancelEditMode();
      }
      renderPortalCards(true);
    }

    if (currentDetailCaseId) {
      updateDetailOwnerControls(currentDetailCaseId);
    }
  }, 1000);
}

/* 頁面切換 */
function showPortal() {
  currentDetailCaseId = null;
  document.getElementById('portal-view').classList.remove('hidden');
  document.getElementById('detail-view').classList.add('hidden');
  document.getElementById('header-output-box').style.display = '';

  if (breathTimer) {
    clearInterval(breathTimer);
  }
}

function updateDetailOwnerControls(id) {
  const controlsEl = document.getElementById('detail-owner-controls');
  const countdownEl = document.getElementById('detail-edit-countdown');
  const editBtn = document.getElementById('detail-edit-btn');
  const deleteBtn = document.getElementById('detail-delete-btn');

  const rawId = typeof id === 'string' && id.startsWith('visitor:') ? id.slice(8) : id;
  const visitorItem = VISITOR_PARTICIPANTS.find(p => p.id === rawId);
  if (visitorItem && canEditOrDeleteCase(visitorItem)) {
    const remain = getRemainingEditSeconds(visitorItem);
    controlsEl.classList.remove('hidden');
    countdownEl.innerText = `可編輯剩餘 ${formatSecondsMMSS(remain)}`;
    editBtn.onclick = () => startEditVisitorCase(rawId);
    deleteBtn.onclick = () => deleteVisitorCase(rawId);
  } else {
    controlsEl.classList.add('hidden');
  }
}

function showDetail(id) {
  const isExplicitVisitor = typeof id === 'string' && id.startsWith('visitor:');
  const lookupId = isExplicitVisitor ? id.slice(8) : id;
  const visitorIdx = VISITOR_PARTICIPANTS.findIndex(p => p.id === lookupId);
  const isVisitorCase = isExplicitVisitor || (!BASE_PARTICIPANTS.some(p => p.id === lookupId) && visitorIdx >= 0);

  const data = isVisitorCase
    ? (VISITOR_PARTICIPANTS[visitorIdx] || VISITOR_PARTICIPANTS[0] || BASE_PARTICIPANTS[0])
    : (BASE_PARTICIPANTS.find(p => p.id === lookupId) || BASE_PARTICIPANTS[0]);

  currentDetailCaseId = isVisitorCase ? `visitor:${data.id}` : data.id;
  const displayCaseId = isVisitorCase ? getVisitorCaseNumber(data, visitorIdx) : data.id;
  const displayRole = isVisitorCase ? getVisitorDisplayRole(data, visitorIdx) : data.role;

  document.getElementById('header-output-box').style.display = 'none';
  updateDetailOwnerControls(currentDetailCaseId);

  document.getElementById('detail-id').innerText = `CASE #${displayCaseId}`;
  document.getElementById('detail-name').innerText = data.name;
  document.getElementById('detail-role').innerText = displayRole;
  document.getElementById('detail-target').innerText = data.target;
  document.getElementById('detail-anxiety').innerText = data.anxiety;
  document.getElementById('detail-statement').innerText = `"${data.statement}"`;

  document.getElementById('detail-birth-info').innerText =
    `自出生日期 ${data.birthDate} 起算，依成人平靜狀態每分鐘 15 次呼吸即時動態估算`;

  const visitorImg = document.getElementById('detail-visitor-img');
  const principalImg = document.getElementById('detail-principal-img');
  const deviceImg = document.getElementById('detail-device-img');
  const noImgFallback = document.getElementById('detail-no-image-fallback');

  applyDetailLeftMode(isVisitorCase);

  if (isVisitorCase) {
    const vUrl = resolveVisitorPhotoSrc(data);
    visitorImg.onerror = function() {
      visitorImg.classList.add('hidden');
      if (noImgFallback) noImgFallback.classList.remove('hidden');
    };

    if (vUrl) {
      visitorImg.src = vUrl;
      visitorImg.classList.remove('hidden');
      if (noImgFallback) noImgFallback.classList.add('hidden');
    } else {
      visitorImg.src = '';
      visitorImg.classList.add('hidden');
      if (noImgFallback) noImgFallback.classList.remove('hidden');
    }
  } else {
    const pUrl = formatImageSrc(data.imageUrl);
    const dUrl = formatImageSrc(data.deviceImageUrl || data.imageUrl);

    principalImg.onerror = function() {
      principalImg.classList.add('hidden');
      deviceImg.classList.add('hidden');
      if (noImgFallback) noImgFallback.classList.remove('hidden');
    };

    if (pUrl) {
      principalImg.src = pUrl;
      deviceImg.src = dUrl;
      principalImg.classList.remove('hidden');
      deviceImg.classList.remove('hidden');
      if (noImgFallback) noImgFallback.classList.add('hidden');
    } else {
      principalImg.src = '';
      deviceImg.src = '';
      principalImg.classList.add('hidden');
      deviceImg.classList.add('hidden');
      if (noImgFallback) noImgFallback.classList.remove('hidden');
    }
  }

  const sigImg = document.getElementById('detail-signature-img');
  const sigPlaceholder = document.getElementById('detail-signature-placeholder');
  if (data.signatureUrl) {
    sigImg.src = formatImageSrc(data.signatureUrl);
    sigImg.classList.remove('hidden');
    sigPlaceholder.classList.add('hidden');
  } else {
    sigImg.src = '';
    sigImg.classList.add('hidden');
    sigPlaceholder.classList.remove('hidden');
  }

  startBreathCounter(data.birthDate);

  document.getElementById('portal-view').classList.add('hidden');
  document.getElementById('detail-view').classList.remove('hidden');

  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  });

  if (!isVisitorCase) {
    setTimeout(updateImgDimensions, 60);
  }
}

/* 初始化 */
window.addEventListener('DOMContentLoaded', async () => {
  syncPrivacyBlurButtonUI();
  renderPortalCards(true);
  initComparisonSlider();
  startOutputFluctuationWithPause();
  initVisitorForm();
  startEditWindowTicker();

  if (hasFirebaseConfigured()) {
    const cloudCases = await fetchCloudVisitorCases();
    const hadMissingCloudPhoto = cloudCases.some(c => !resolveVisitorPhotoSrc(c)) &&
      VISITOR_PARTICIPANTS.some(c => Boolean(resolveVisitorPhotoSrc(c)));
    VISITOR_PARTICIPANTS = cloudCases.length > 0 ? cloudCases : VISITOR_PARTICIPANTS;
    saveLocalVisitorCases(VISITOR_PARTICIPANTS);
    if (hadMissingCloudPhoto) {
      await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);
    }
    renderPortalCards(true);
    startCloudSyncPolling();
  }

  const urlParams = new URLSearchParams(window.location.search);
  const targetId = urlParams.get('id');

  if (targetId) {
    showDetail(targetId);
  }
});
</script>

</body>
</html>
