<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>理想狀態 - Archive Portal</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation/selfie_segmentation.js" crossorigin="anonymous"></script>

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

@media (hover: none) {
  .mobile-auto-fade {
    animation: mobileFade 6s infinite ease-in-out;
  }

  .mobile-auto-bg-fade {
    animation: mobileBgFade 6s infinite ease-in-out;
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
}
</style>
</head>

<body class="bg-grid-dark min-h-screen text-gray-300 relative selection:bg-emerald-500 selection:text-black">

<div class="fixed top-0 left-1/2 -translate-x-1/2 w-full max-w-6xl h-80 bg-emerald-950/20 blur-[130px] pointer-events-none rounded-full"></div>

<div class="max-w-5xl mx-auto px-4 sm:px-6 py-8 md:py-14 relative z-10">

  <!-- HEADER -->

  <header class="border-b border-gray-700/70 pb-8 mb-12">

    <div class="flex flex-wrap items-center justify-between gap-3 mb-8 font-mono text-[10px] sm:text-xs tracking-[0.14em]">

      <div class="flex items-center gap-2 text-emerald-400">
        <span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse"></span>
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

      <div class="archive-header-actions">

        <button
          id="header-concept-button"
          onclick="openConceptModal()"
          class="btn-white inline-flex items-center justify-center gap-2 shadow-sm font-mono"
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
              只需填寫焦慮自白即可建立病例（其餘皆可選填）；上傳後 3 分鐘內僅限原上傳裝置可編輯或刪除
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
                委託人姓名 / 代號 (選填)
              </label>
              <input
                type="text"
                id="visitor-name"
                placeholder="例如：王Ｏ明（留空自動編號）"
                class="clinical-input"
              >
            </div>

            <div class="w-full min-w-0">
              <label for="visitor-birth" class="block text-[10px] text-gray-400 mb-1.5">
                出生日期 (選填，計算呼吸數)
              </label>
              <input
                type="date"
                id="visitor-birth"
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
                點擊下方按鈕可拍照或從相簿選取照片，系統將自動去背並套用黑白灰階與微調曝光
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
                class="hidden w-full sm:w-auto px-3.5 py-2 rounded border border-red-900/60 bg-red-950/20 text-red-400 hover:bg-red-950/40 text-xs text-center"
              >
                清除已選照片
              </button>

            </div>

            <div id="photo-processing-status" class="hidden text-[11px] text-emerald-400 flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></span>
              <span>正在進行人像去背、黑白灰階與曝光微調處理...</span>
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
                  ✓ 影像已完成去背與黑白曝光調整
                </span>
                <span class="text-gray-500 block font-sans leading-relaxed">
                  已套用純黑底色與自然黑白灰階；可隨時切換上方「隱私模糊保護」確認遮蔽效果
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
                class="hidden w-full sm:w-auto px-4 py-2.5 rounded border border-gray-700 text-gray-300 hover:text-white transition-colors text-xs text-center"
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
        class="inline-flex items-center space-x-2 text-xs text-gray-300 hover:text-white transition-colors bg-gray-900/90 hover:bg-gray-800 px-4 py-2 rounded border border-gray-700"
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
          [SYSTEM NOTE] 個人特徵（髮型/皮肉）已於代理化過程中剝離。
        </p>

      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6 items-stretch">

        <div
          id="detail-slider-container"
          class="w-full h-full min-h-[360px] md:min-h-full bg-gray-950 rounded-lg overflow-hidden border border-gray-800 relative select-none group"
        >

          <div
            id="detail-no-image-fallback"
            class="hidden absolute inset-0 flex flex-col items-center justify-center space-y-2 text-gray-700 z-0"
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
    statement: "我發現自己根本無法忍受純粹的放空。只要一停下來，那種『時間正被浪費』的罪惡感就會把我吞噬。我做這個替身，不是為了讓自己舒服地去休息，而是因為我停不下來——我需要一個實體掛在那裡，替我承擔什麼都不做、只剩下呼吸的死寂。"
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
    statement: "休假對我來說不是放鬆，而是一場審判。躺在床上滑手機的每一分鐘，都在盤算著自己少做了多少事。如果一定要有人在那裡放空，那就讓這顆塑膠薄膜包覆的人頭替我坐牢吧。"
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
const EDIT_WINDOW_MS = 3 * 60 * 1000; // 3 分鐘可編輯/刪除視窗

function hasFirebaseConfigured() {
  return Boolean(
    FIREBASE_DB_URL &&
    FIREBASE_DB_URL.startsWith("https://")
  );
}

/* 取得本機裝置唯一識別碼（確保只有原上傳裝置能在 3 分鐘內修改或刪除） */
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
let rawCutoutBaseDataUrl = "";
let capturedPhotoDataUrl = "";
let isPrivacyBlurEnabled = false;
let editingCaseId = null;
let currentDetailCaseId = null;
let lastRenderedSignature = "";

let VISITOR_PARTICIPANTS = loadLocalVisitorCases();
let breathTimer = null;

function formatImageSrc(url) {
  if (!url) return "";
  if (url.startsWith("data:")) return url;
  return `${url}?v=${Date.now()}`;
}

function loadLocalVisitorCases() {
  try {
    const raw = localStorage.getItem(VISITOR_STORAGE_KEY);
    if (!raw) return [];
    const parsed = JSON.parse(raw);
    return Array.isArray(parsed) ? parsed : [];
  } catch (err) {
    return [];
  }
}

function saveLocalVisitorCases(cases) {
  try {
    localStorage.setItem(VISITOR_STORAGE_KEY, JSON.stringify(cases));
  } catch (err) {
    // 忽略本機快取空間限制
  }
}

/* 從 Firebase 雲端讀取所有觀眾病例 */
async function fetchCloudVisitorCases() {
  if (!hasFirebaseConfigured()) {
    return loadLocalVisitorCases();
  }

  try {
    const res = await fetch(`${FIREBASE_CASES_ENDPOINT}?t=${Date.now()}`, {
      cache: "no-store"
    });
    if (!res.ok) throw new Error("Firebase fetch failed");
    const data = await res.json();

    if (!data) return [];

    let list = [];
    if (Array.isArray(data)) {
      list = data.filter(Boolean);
    } else if (typeof data === "object") {
      list = Object.values(data).filter(Boolean);
    }

    list.sort((a, b) => {
      const na = parseInt(a.id, 10) || 0;
      const nb = parseInt(b.id, 10) || 0;
      return na - nb;
    });

    return list;
  } catch (err) {
    return loadLocalVisitorCases();
  }
}

/* 將所有觀眾病例同步寫入 Firebase 雲端 */
async function syncVisitorCasesToCloud(cases) {
  saveLocalVisitorCases(cases);

  if (!hasFirebaseConfigured()) {
    return { ok: false, reason: "NOT_CONFIGURED" };
  }

  try {
    const payload = cases.length === 0 ? null : cases;
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

function getAllParticipants() {
  return [...BASE_PARTICIPANTS, ...VISITOR_PARTICIPANTS];
}

function isVisitorCaseById(id) {
  return VISITOR_PARTICIPANTS.some(p => p.id === id);
}

function canEditOrDeleteCase(caseItem) {
  if (!caseItem || !caseItem.ownerToken || !caseItem.createdAt) return false;
  if (caseItem.ownerToken !== CURRENT_DEVICE_TOKEN) return false;
  const elapsed = Date.now() - Number(caseItem.createdAt);
  return elapsed >= 0 && elapsed < EDIT_WINDOW_MS;
}

function getRemainingEditSeconds(caseItem) {
  if (!canEditOrDeleteCase(caseItem)) return 0;
  const remainingMs = EDIT_WINDOW_MS - (Date.now() - Number(caseItem.createdAt));
  return Math.max(0, Math.ceil(remainingMs / 1000));
}

function formatSecondsMMSS(sec) {
  const m = String(Math.floor(sec / 60)).padStart(2, '0');
  const s = String(sec % 60).padStart(2, '0');
  return `${m}:${s}`;
}

function updateCaseCountersUI() {
  const visitorTotalEl = document.getElementById('visitor-total-label');
  const nextIdEl = document.getElementById('next-case-preview');
  const visitorCountStr = String(VISITOR_PARTICIPANTS.length).padStart(2, '0');

  if (visitorTotalEl) {
    visitorTotalEl.innerText = `VISITOR CASES: ${visitorCountStr}`;
  }

  if (nextIdEl) {
    if (editingCaseId) {
      nextIdEl.innerText = `EDITING: CASE #${editingCaseId}`;
    } else {
      const maxNum = VISITOR_PARTICIPANTS.reduce((max, item) => {
        const n = parseInt(item.id, 10);
        return !isNaN(n) && n > max ? n : max;
      }, BASE_PARTICIPANTS.length);
      const nextIdStr = String(maxNum + 1).padStart(2, '0');
      nextIdEl.innerText = `NEXT ID: CASE #${nextIdStr}`;
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

/* 圖片對比 */
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

/* 設定詳情頁是否顯示左右對比滑桿（觀眾病例不顯示滑桿） */
function setDetailSliderVisibility(isVisitorCase) {
  const clipWrapper = document.getElementById('slider-clip-wrapper');
  const sliderInput = document.getElementById('comparison-slider');
  const sliderLine = document.getElementById('slider-line');
  const badgePrincipal = document.getElementById('badge-principal');
  const badgeDevice = document.getElementById('badge-device');
  const principalImg = document.getElementById('detail-principal-img');

  if (isVisitorCase) {
    if (clipWrapper) clipWrapper.style.display = 'none';
    if (sliderInput) sliderInput.style.display = 'none';
    if (sliderLine) sliderLine.style.display = 'none';
    if (badgePrincipal) badgePrincipal.style.display = 'none';
    if (badgeDevice) badgeDevice.style.display = 'none';
    if (principalImg) {
      principalImg.style.width = '100%';
      principalImg.style.height = '100%';
      principalImg.style.maxWidth = '100%';
    }
  } else {
    if (clipWrapper) {
      clipWrapper.style.display = '';
      clipWrapper.style.width = '50%';
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

/* 單張卡片模板（左上僅留 CASE 編號；修改/刪除置於檢視按鈕上方、左右對齊、各佔一半） */
function buildCaseCardHTML(p, isVisitor = false) {
  const imgSrc = formatImageSrc(p.imageUrl);
  const deviceImgSrc = formatImageSrc(p.deviceImageUrl || p.imageUrl);
  const editable = isVisitor && canEditOrDeleteCase(p);

  return `
    <div
      onclick="showDetail('${p.id}')"
      class="glass-card rounded-xl overflow-hidden cursor-pointer flex flex-col justify-between relative group"
    >

      <div class="aspect-[3/4] w-full bg-gray-950 relative overflow-hidden flex items-center justify-center">

        <div class="absolute top-2.5 left-2.5 sm:top-3 sm:left-3 pointer-events-none z-30">
          <span class="text-[10px] sm:text-xs font-mono text-white tracking-wider font-bold bg-black/70 backdrop-blur-md px-2 sm:px-2.5 py-0.5 sm:py-1 rounded shadow-lg inline-block transition-all duration-300 group-hover:bg-transparent group-hover:backdrop-blur-none group-hover:shadow-none mobile-auto-bg-fade">
            CASE #${p.id}
          </span>
        </div>

        ${
          p.imageUrl
            ? `
              <img
                src="${imgSrc}"
                alt="${p.name}"
                class="w-full h-full object-cover group-hover:scale-105 transition-all duration-500 absolute inset-0 z-0"
              >

              <img
                src="${deviceImgSrc}"
                alt="Proxy Device"
                class="w-full h-full object-cover opacity-0 group-hover:opacity-100 mobile-auto-fade group-hover:scale-105 transition-all duration-500 absolute inset-0 z-10"
              >
            `
            : `
              <div class="flex flex-col items-center space-y-2 text-gray-700">

                <svg
                  class="w-10 h-10 sm:w-12 sm:h-12 stroke-gray-700"
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
            `
        }

        <div class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/30 to-transparent pointer-events-none z-20"></div>

        <div class="absolute ${editable ? 'bottom-[74px] sm:bottom-[82px]' : 'bottom-12 sm:bottom-14'} left-3 right-3 sm:left-4 sm:right-4 space-y-0.5 pointer-events-none z-30 transition-all duration-300">

          <h3 class="text-sm sm:text-lg font-bold text-white tracking-wide truncate">
            ${p.name}
          </h3>

          <p class="text-[10px] sm:text-xs text-gray-300 font-mono truncate">
            ${p.role}
          </p>

        </div>

        <div class="absolute bottom-2.5 left-2.5 right-2.5 sm:bottom-3 sm:left-3 sm:right-3 z-30 flex flex-col gap-1.5">

          ${
            editable
              ? `
                <div class="grid grid-cols-2 gap-1.5 w-full" onclick="event.stopPropagation()">
                  <button
                    type="button"
                    onclick="startEditVisitorCase('${p.id}')"
                    class="w-full py-1 sm:py-1.5 rounded bg-white/95 text-black font-mono text-[10px] sm:text-xs font-bold hover:bg-gray-200 transition-colors shadow text-center"
                  >
                    ✎ 修改
                  </button>
                  <button
                    type="button"
                    onclick="deleteVisitorCase('${p.id}')"
                    class="w-full py-1 sm:py-1.5 rounded bg-red-950/90 text-red-400 border border-red-700/70 font-mono text-[10px] sm:text-xs font-bold hover:bg-red-800 hover:text-white transition-colors shadow text-center"
                  >
                    ✕ 刪除
                  </button>
                </div>
              `
              : ''
          }

          <button class="btn-neon w-full py-1.5 sm:py-2 text-[10px] sm:text-xs rounded shadow-lg flex items-center justify-center space-x-1">
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
  const sig = VISITOR_PARTICIPANTS.map(p => `${p.id}|${p.name}|${p.statement}|${Boolean(p.imageUrl)}`).join("||");
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
      visitorContainer.innerHTML = VISITOR_PARTICIPANTS.map(p => buildCaseCardHTML(p, true)).join('');
    }
  }

  updateCaseCountersUI();
}

/* 人像去背 + 一般黑白灰階 + 適度曝光微調 */
let selfieSegmenterInstance = null;

function getSelfieSegmenter() {
  if (selfieSegmenterInstance) return selfieSegmenterInstance;
  if (typeof SelfieSegmentation === "undefined") return null;

  try {
    const segmenter = new SelfieSegmentation({
      locateFile: (file) => `https://cdn.jsdelivr.net/npm/@mediapipe/selfie_segmentation/${file}`
    });
    segmenter.setOptions({
      modelSelection: 1,
      selfieMode: false
    });
    selfieSegmenterInstance = segmenter;
    return segmenter;
  } catch (e) {
    return null;
  }
}

function applyClinicalMonochromeTone(ctx, width, height, alphaMaskData = null) {
  const imgData = ctx.getImageData(0, 0, width, height);
  const data = imgData.data;

  const exposureGain = 1.18;
  const contrastFactor = 1.08;

  for (let i = 0; i < data.length; i += 4) {
    const fgAlpha = alphaMaskData ? alphaMaskData[i + 3] / 255 : 1;

    if (fgAlpha < 0.08) {
      data[i] = 8;
      data[i + 1] = 9;
      data[i + 2] = 10;
      continue;
    }

    const r = data[i];
    const g = data[i + 1];
    const b = data[i + 2];

    let lum = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
    lum = lum * exposureGain;
    lum = (lum - 0.5) * contrastFactor + 0.5;
    lum = Math.max(0, Math.min(1, lum));

    const edgeBlend = Math.min(1, Math.max(0, (fgAlpha - 0.08) / 0.55));
    const finalLum = lum * edgeBlend + (8 / 255) * (1 - edgeBlend);

    const val = Math.round(finalLum * 255);
    data[i] = val;
    data[i + 1] = val;
    data[i + 2] = val;
  }

  ctx.putImageData(imgData, 0, 0);
}

function fallbackBackgroundCutout(img, width, height) {
  const canvas = document.createElement('canvas');
  canvas.width = width;
  canvas.height = height;
  const ctx = canvas.getContext('2d');
  ctx.drawImage(img, 0, 0, width, height);

  const imgData = ctx.getImageData(0, 0, width, height);
  const data = imgData.data;

  const maskCanvas = document.createElement('canvas');
  maskCanvas.width = width;
  maskCanvas.height = height;
  const maskCtx = maskCanvas.getContext('2d');
  const maskImgData = maskCtx.createImageData(width, height);
  const mData = maskImgData.data;

  const corners = [
    0,
    (width - 1) * 4,
    (Math.floor(height * 0.15) * width) * 4,
    (Math.floor(height * 0.15) * width + width - 1) * 4
  ];

  let bgR = 0, bgG = 0, bgB = 0;
  corners.forEach(idx => {
    bgR += data[idx];
    bgG += data[idx + 1];
    bgB += data[idx + 2];
  });
  bgR /= corners.length;
  bgG /= corners.length;
  bgB /= corners.length;

  const cx = width * 0.5;
  const cy = height * 0.48;

  for (let y = 0; y < height; y++) {
    for (let x = 0; x < width; x++) {
      const idx = (y * width + x) * 4;
      const dr = data[idx] - bgR;
      const dg = data[idx + 1] - bgG;
      const db = data[idx + 2] - bgB;
      const colorDist = Math.sqrt(dr * dr + dg * dg + db * db);

      const nx = (x - cx) / (width * 0.42);
      const ny = (y - cy) / (height * 0.50);
      const radialDist = Math.sqrt(nx * nx + ny * ny);

      let alpha = 255;
      if (colorDist < 42 && radialDist > 0.60) {
        alpha = 0;
      } else if (radialDist > 0.94) {
        alpha = Math.max(0, Math.round(255 * (1 - (radialDist - 0.94) * 4)));
      }

      mData[idx] = 255;
      mData[idx + 1] = 255;
      mData[idx + 2] = 255;
      mData[idx + 3] = alpha;
    }
  }

  applyClinicalMonochromeTone(ctx, width, height, mData);
  return canvas.toDataURL('image/jpeg', 0.82);
}

function processPortraitCutoutAndTone(file) {
  return new Promise((resolve) => {
    const reader = new FileReader();
    reader.onload = function(event) {
      const img = new Image();
      img.onload = async function() {
        const maxDim = 640;
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

        const segmenter = getSelfieSegmenter();
        if (!segmenter) {
          resolve(fallbackBackgroundCutout(img, width, height));
          return;
        }

        let resolved = false;
        const timeoutId = setTimeout(() => {
          if (!resolved) {
            resolved = true;
            resolve(fallbackBackgroundCutout(img, width, height));
          }
        }, 4500);

        try {
          segmenter.onResults((results) => {
            if (resolved) return;
            resolved = true;
            clearTimeout(timeoutId);

            const maskCanvas = document.createElement('canvas');
            maskCanvas.width = width;
            maskCanvas.height = height;
            const maskCtx = maskCanvas.getContext('2d');
            maskCtx.drawImage(results.segmentationMask, 0, 0, width, height);
            const alphaMaskData = maskCtx.getImageData(0, 0, width, height).data;

            const outCanvas = document.createElement('canvas');
            outCanvas.width = width;
            outCanvas.height = height;
            const outCtx = outCanvas.getContext('2d');
            outCtx.drawImage(results.image, 0, 0, width, height);

            applyClinicalMonochromeTone(outCtx, width, height, alphaMaskData);

            resolve(outCanvas.toDataURL('image/jpeg', 0.82));
          });

          const inputCanvas = document.createElement('canvas');
          inputCanvas.width = width;
          inputCanvas.height = height;
          inputCanvas.getContext('2d').drawImage(img, 0, 0, width, height);
          await segmenter.send({ image: inputCanvas });
        } catch (err) {
          if (!resolved) {
            resolved = true;
            clearTimeout(timeoutId);
            resolve(fallbackBackgroundCutout(img, width, height));
          }
        }
      };
      img.src = event.target.result;
    };
    reader.readAsDataURL(file);
  });
}

function rebuildOutputPhotoWithPrivacyOption() {
  return new Promise((resolve) => {
    if (!rawCutoutBaseDataUrl) {
      capturedPhotoDataUrl = "";
      resolve("");
      return;
    }

    if (!isPrivacyBlurEnabled) {
      capturedPhotoDataUrl = rawCutoutBaseDataUrl;
      updatePhotoPreviewUI();
      resolve(capturedPhotoDataUrl);
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

      if (typeof ctx.filter !== 'undefined') {
        ctx.filter = 'blur(14px)';
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        ctx.filter = 'none';
      } else {
        const smallW = Math.max(18, Math.round(canvas.width * 0.06));
        const smallH = Math.max(24, Math.round(canvas.height * 0.06));
        const tmpCanvas = document.createElement('canvas');
        tmpCanvas.width = smallW;
        tmpCanvas.height = smallH;
        const tmpCtx = tmpCanvas.getContext('2d');
        tmpCtx.drawImage(img, 0, 0, smallW, smallH);

        ctx.imageSmoothingEnabled = true;
        ctx.drawImage(tmpCanvas, 0, 0, smallW, smallH, 0, 0, canvas.width, canvas.height);
      }

      capturedPhotoDataUrl = canvas.toDataURL('image/jpeg', 0.80);
      updatePhotoPreviewUI();
      resolve(capturedPhotoDataUrl);
    };
    img.src = rawCutoutBaseDataUrl;
  });
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
      ? "✓ 影像已完成去背＋黑白曝光＋隱私模糊加密"
      : "✓ 影像已完成去背與黑白曝光調整";
  }
}

async function togglePrivacyBlur() {
  isPrivacyBlurEnabled = !isPrivacyBlurEnabled;

  const btn = document.getElementById('privacy-blur-toggle');
  const indicator = document.getElementById('privacy-blur-indicator');
  const label = document.getElementById('privacy-blur-label');

  if (isPrivacyBlurEnabled) {
    btn.className = "w-full sm:w-auto px-3.5 py-2 rounded text-xs font-mono inline-flex items-center justify-center gap-2 border transition-all cursor-pointer bg-emerald-950/50 text-emerald-400 border-emerald-500/60";
    indicator.className = "w-2 h-2 rounded-full bg-emerald-400 animate-pulse";
    label.innerText = "隱私模糊保護：已開啟 (臉部已模糊化)";
  } else {
    btn.className = "w-full sm:w-auto px-3.5 py-2 rounded text-xs font-mono inline-flex items-center justify-center gap-2 border transition-all cursor-pointer bg-gray-900/90 text-gray-300 border-gray-700 hover:border-emerald-500/60";
    indicator.className = "w-2 h-2 rounded-full bg-gray-500";
    label.innerText = "隱私模糊保護：關閉 (點擊模糊化臉部)";
  }

  if (rawCutoutBaseDataUrl) {
    await rebuildOutputPhotoWithPrivacyOption();
  }
}

function clearCapturedPhoto() {
  rawCutoutBaseDataUrl = "";
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

  rawCutoutBaseDataUrl = await processPortraitCutoutAndTone(file);
  await rebuildOutputPhotoWithPrivacyOption();

  if (statusEl) statusEl.classList.add('hidden');
}

function startEditVisitorCase(id) {
  const target = VISITOR_PARTICIPANTS.find(p => p.id === id);
  if (!target || !canEditOrDeleteCase(target)) {
    alert("此病例已超過 3 分鐘修改時限，或非原上傳裝置建立之檔案。");
    renderPortalCards(true);
    return;
  }

  editingCaseId = id;
  showPortal();

  document.getElementById('visitor-statement').value = target.statement || "";
  document.getElementById('visitor-name').value = target.name || "";
  document.getElementById('visitor-birth').value = target.birthDate || "";
  document.getElementById('visitor-anxiety').value = target.anxiety || "";

  if (target.rawImageUrl || target.imageUrl) {
    rawCutoutBaseDataUrl = target.rawImageUrl || target.imageUrl;
    capturedPhotoDataUrl = target.imageUrl;
    updatePhotoPreviewUI();
  } else {
    clearCapturedPhoto();
  }

  document.getElementById('form-mode-title').innerText = `修改病例檔案：CASE #${id}`;
  document.getElementById('submit-case-btn-text').innerText = "儲存病例修改";
  document.getElementById('cancel-edit-btn').classList.remove('hidden');
  document.getElementById('visitor-form-feedback').innerText = `正在編輯 CASE #${id}（僅限建立後 3 分鐘內修改）`;

  updateCaseCountersUI();

  const formSection = document.getElementById('visitor-case-section');
  if (formSection) {
    formSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
}

function cancelEditMode() {
  editingCaseId = null;
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
  const target = VISITOR_PARTICIPANTS.find(p => p.id === id);
  if (!target || !canEditOrDeleteCase(target)) {
    alert("此病例已超過 3 分鐘刪除時限，或非原上傳裝置建立之檔案。");
    renderPortalCards(true);
    return;
  }

  if (!confirm(`確定要刪除病例 CASE #${id}（${target.name}）嗎？`)) {
    return;
  }

  VISITOR_PARTICIPANTS = VISITOR_PARTICIPANTS.filter(p => p.id !== id);
  const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);

  if (editingCaseId === id) {
    cancelEditMode();
  }

  renderPortalCards(true);

  if (currentDetailCaseId === id) {
    showPortal();
  }

  const feedback = document.getElementById('visitor-form-feedback');
  if (feedback) {
    feedback.innerText = syncResult.ok
      ? `已從雲端刪除病例 CASE #${id}（全裝置已同步）。`
      : `已刪除病例 CASE #${id}（雲端連線異常，已於本機更新）。`;
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

    feedback.textContent = "正在同步至雲端資料庫...";

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
      const resolvedBirth = birthInput || existing.birthDate || "2005-01-01";

      VISITOR_PARTICIPANTS[idx] = {
        ...existing,
        name: resolvedName,
        target: `${resolvedName} (本體)`,
        anxiety: anxietyInput || existing.anxiety,
        birthDate: resolvedBirth,
        rawImageUrl: rawCutoutBaseDataUrl,
        imageUrl: capturedPhotoDataUrl,
        deviceImageUrl: capturedPhotoDataUrl,
        statement: statementInput
      };

      const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);
      const updatedId = existing.id;
      cancelEditMode();
      renderPortalCards(true);
      feedback.textContent = syncResult.ok
        ? `已更新病例 CASE #${updatedId}（${resolvedName}）並同步至所有裝置。`
        : `已更新病例 CASE #${updatedId}（雲端連線異常，已暫存於本機）。`;
      return;
    }

    const latestCloud = await fetchCloudVisitorCases();
    if (latestCloud.length > 0) {
      VISITOR_PARTICIPANTS = latestCloud;
    }

    const maxNum = VISITOR_PARTICIPANTS.reduce((max, item) => {
      const n = parseInt(item.id, 10);
      return !isNaN(n) && n > max ? n : max;
    }, BASE_PARTICIPANTS.length);

    const nextNum = maxNum + 1;
    const nextId = String(nextNum).padStart(2, '0');
    const resolvedName = nameInput || `現場委託人 #${nextId}`;
    const resolvedBirth = birthInput || "2005-01-01";

    const newCase = {
      id: nextId,
      name: resolvedName,
      role: `現場延伸病例 ${nextId} 號`,
      target: `${resolvedName} (本體)`,
      anxiety: anxietyInput || "85.0% (現場登錄)",
      birthDate: resolvedBirth,
      rawImageUrl: rawCutoutBaseDataUrl,
      imageUrl: capturedPhotoDataUrl,
      deviceImageUrl: capturedPhotoDataUrl,
      signatureUrl: "",
      statement: statementInput,
      ownerToken: CURRENT_DEVICE_TOKEN,
      createdAt: Date.now()
    };

    VISITOR_PARTICIPANTS = [...VISITOR_PARTICIPANTS, newCase];
    const syncResult = await syncVisitorCasesToCloud(VISITOR_PARTICIPANTS);

    renderPortalCards(true);
    form.reset();
    clearCapturedPhoto();

    if (syncResult.ok) {
      feedback.textContent = `已建立病例 CASE #${nextId}（${resolvedName}）並同步至雲端，3 分鐘內可於本裝置修改或刪除。`;
    } else {
      feedback.textContent = `已建立病例 CASE #${nextId}（${resolvedName}），雲端連線異常，請確認 Firebase 測試規則權限。`;
    }
  });
}

/* 定期從雲端同步最新病例（每 3 秒一次） */
function startCloudSyncPolling() {
  if (!hasFirebaseConfigured()) return;

  setInterval(async () => {
    if (editingCaseId) return;
    const cloudCases = await fetchCloudVisitorCases();
    const nextSig = cloudCases.map(p => `${p.id}|${p.name}|${p.statement}|${Boolean(p.imageUrl)}`).join("||");
    if (nextSig !== lastRenderedSignature) {
      VISITOR_PARTICIPANTS = cloudCases;
      saveLocalVisitorCases(cloudCases);
      renderPortalCards(true);
    }
  }, 3000);
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

  const visitorItem = VISITOR_PARTICIPANTS.find(p => p.id === id);
  if (visitorItem && canEditOrDeleteCase(visitorItem)) {
    const remain = getRemainingEditSeconds(visitorItem);
    controlsEl.classList.remove('hidden');
    countdownEl.innerText = `可編輯剩餘 ${formatSecondsMMSS(remain)}`;
    editBtn.onclick = () => startEditVisitorCase(id);
    deleteBtn.onclick = () => deleteVisitorCase(id);
  } else {
    controlsEl.classList.add('hidden');
  }
}

function showDetail(id) {
  const allCases = getAllParticipants();
  const data = allCases.find(p => p.id === id) || BASE_PARTICIPANTS[0];
  currentDetailCaseId = data.id;
  const isVisitorCase = isVisitorCaseById(data.id);

  document.getElementById('header-output-box').style.display = 'none';
  updateDetailOwnerControls(data.id);

  document.getElementById('detail-id').innerText = `CASE #${data.id}`;
  document.getElementById('detail-name').innerText = data.name;
  document.getElementById('detail-role').innerText = data.role;
  document.getElementById('detail-target').innerText = data.target;
  document.getElementById('detail-anxiety').innerText = data.anxiety;
  document.getElementById('detail-statement').innerText = `"${data.statement}"`;

  document.getElementById('detail-birth-info').innerText =
    `自出生日期 ${data.birthDate} 起算，依成人平靜狀態每分鐘 15 次呼吸即時動態估算`;

  const principalImg = document.getElementById('detail-principal-img');
  const deviceImg = document.getElementById('detail-device-img');
  const noImgFallback = document.getElementById('detail-no-image-fallback');

  const pUrl = formatImageSrc(data.imageUrl);
  const dUrl = formatImageSrc(data.deviceImageUrl || data.imageUrl);

  if (pUrl) {
    principalImg.src = pUrl;
    deviceImg.src = isVisitorCase ? '' : dUrl;
    principalImg.classList.remove('hidden');
    if (isVisitorCase) {
      deviceImg.classList.add('hidden');
    } else {
      deviceImg.classList.remove('hidden');
    }
    if (noImgFallback) noImgFallback.classList.add('hidden');
  } else {
    principalImg.src = '';
    deviceImg.src = '';
    principalImg.classList.add('hidden');
    deviceImg.classList.add('hidden');
    if (noImgFallback) noImgFallback.classList.remove('hidden');
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

  setDetailSliderVisibility(isVisitorCase);
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
  renderPortalCards(true);
  initComparisonSlider();
  startOutputFluctuationWithPause();
  initVisitorForm();
  startEditWindowTicker();

  if (hasFirebaseConfigured()) {
    const cloudCases = await fetchCloudVisitorCases();
    VISITOR_PARTICIPANTS = cloudCases;
    saveLocalVisitorCases(cloudCases);
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
