<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>신규 개발 크림 3일 사용 품평회 | R&D Formulation Lab</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Fonts: Noto Sans KR & Inter -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Noto+Sans+KR:wght@300;400;500;600;700&display=swap" rel="stylesheet" />
  <!-- Chart.js for Live Data Visualization -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>

  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['"Noto Sans KR"', 'Inter', '-apple-system', 'BlinkMacSystemFont', 'sans-serif'],
            display: ['Inter', '"Noto Sans KR"', 'sans-serif'],
          },
          colors: {
            ivory: {
              50: '#FDFCFA',
              100: '#FAF8F4',
              200: '#F3EFE8',
              300: '#E6E0D5',
            },
            sage: {
              50: '#F2F6F4',
              100: '#E1ECE7',
              200: '#C3D9D0',
              500: '#4A7C6F',
              600: '#3B665B',
              700: '#2E5148',
            },
            terracotta: {
              50: '#FDF5F2',
              100: '#FAE6DF',
              200: '#F3C8B9',
              500: '#C86D51',
              600: '#B0583D',
            },
          },
        },
      },
    };
  </script>

  <style>
    body {
      background-color: #FAF8F4;
      color: #1E293B;
      -webkit-tap-highlight-color: transparent;
    }

    /* Custom smooth scrollbar for tables */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #F3EFE8;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb {
      background: #CBD5E1;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #94A3B8;
    }

    /* Subtle pulse for live indicator */
    @keyframes pulse-dot {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.45; transform: scale(1.2); }
    }
    .live-dot {
      animation: pulse-dot 2s infinite ease-in-out;
    }

    /* Rating button active elevation */
    .rating-btn {
      transition: all 0.16s cubic-bezier(0.4, 0, 0.2, 1);
    }
    .rating-btn:active {
      transform: scale(0.95);
    }
  </style>
</head>
<body class="min-h-screen flex flex-col antialiased selection:bg-sage-200 selection:text-sage-700">

  <!-- Top Lab Header -->
  <header class="bg-white/90 backdrop-blur-md border-b border-ivory-300 sticky top-0 z-30">
    <div class="max-w-5xl mx-auto px-4 sm:px-6 pt-4 pb-3">
      <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2">
        <div>
          <div class="inline-flex items-center gap-2 px-2.5 py-0.5 rounded-full bg-sage-50 border border-sage-200 text-sage-700 text-xs font-medium tracking-wide mb-1.5">
            <span class="w-1.5 h-1.5 rounded-full bg-sage-500 live-dot"></span>
            R&amp;D FORMULATION LAB · 3-DAY SAMPLE TEST
          </div>
          <h1 class="text-xl sm:text-2xl font-bold text-slate-800 tracking-tight">
            신규 개발 크림 3일 사용 품평회
          </h1>
        </div>
        <div class="flex items-center gap-2 self-start sm:self-center">
          <span id="connectionStatusBadge" class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-medium bg-ivory-200 text-slate-600">
            <span id="connectionDot" class="w-2 h-2 rounded-full bg-amber-400"></span>
            <span id="connectionText">서버 연결 중...</span>
          </span>
        </div>
      </div>

      <!-- Segmented Navigation Tabs -->
      <nav class="mt-4 grid grid-cols-2 gap-1.5 p-1 bg-ivory-200/80 rounded-2xl border border-ivory-300/70" aria-label="품평회 탭 메뉴">
        <button
          type="button"
          id="tabBtnSurvey"
          onclick="switchTab('survey')"
          class="flex items-center justify-center gap-2 py-2.5 px-3 rounded-xl text-sm font-semibold transition-all duration-200 bg-white text-sage-700 shadow-sm"
        >
          <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
          </svg>
          <span>1. 설문 작성하기</span>
        </button>
        <button
          type="button"
          id="tabBtnReport"
          onclick="switchTab('report')"
          class="flex items-center justify-center gap-2 py-2.5 px-3 rounded-xl text-sm font-semibold transition-all duration-200 text-slate-600 hover:text-slate-800"
        >
          <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z" />
          </svg>
          <span>2. 실시간 결과 리포트</span>
          <span id="tabParticipantCount" class="inline-flex items-center justify-center px-2 py-0.5 text-xs font-bold rounded-full bg-sage-100 text-sage-700">
            0명 참여
          </span>
        </button>
      </nav>
    </div>
  </header>

  <!-- Main Content Container -->
  <main class="flex-1 max-w-5xl w-full mx-auto px-4 sm:px-6 py-6 sm:py-8">

    <!-- =================================================================== -->
    <!-- TAB 1: SURVEY FORM (설문 작성하기)                                    -->
    <!-- =================================================================== -->
    <section id="surveyView" class="space-y-6">
      <!-- Intro Banner Card -->
      <div class="bg-white rounded-2xl p-5 sm:p-6 border border-ivory-300 shadow-sm">
        <div class="flex items-start gap-3.5">
          <div class="w-10 h-10 rounded-xl bg-sage-50 border border-sage-200 flex items-center justify-center text-sage-600 shrink-0 mt-0.5">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" stroke-width="1.8" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z" />
            </svg>
          </div>
          <div>
            <h2 class="text-base sm:text-lg font-bold text-slate-800">3일간 샘플을 사용해보신 솔직한 후기를 들려주세요</h2>
            <p class="text-sm text-slate-600 mt-1 leading-relaxed">
              임직원 여러분의 세심한 피드백은 정식 출시 처방(제형·보습·사용감) 고도화에 직접 반영됩니다. 소요 시간은 약 <span class="font-semibold text-sage-700">2분</span>이며, 제출 즉시 상단 <span class="font-semibold text-slate-700">[실시간 결과 리포트]</span>에 통합 집계됩니다.
            </p>
          </div>
        </div>
      </div>

      <!-- Validation Alert Banner (Hidden by default) -->
      <div id="formValidationAlert" class="hidden bg-terracotta-50 border border-terracotta-200 text-terracotta-600 rounded-2xl p-4 flex items-center gap-3">
        <svg class="w-5 h-5 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
        </svg>
        <p id="formValidationMessage" class="text-sm font-medium">필수 항목을 모두 선택해 주세요.</p>
      </div>

      <form id="creamSurveyForm" novalidate class="space-y-6">
        <!-- SECTION 1: 기본 정보 -->
        <div class="bg-white rounded-2xl p-5 sm:p-7 border border-ivory-300 shadow-sm space-y-6">
          <div class="border-b border-ivory-200 pb-3.5 flex items-center justify-between">
            <div>
              <span class="text-xs font-bold uppercase tracking-wider text-sage-600">SECTION 01</span>
              <h3 class="text-lg font-bold text-slate-800 mt-0.5">기본 정보</h3>
            </div>
            <span class="text-xs text-slate-400">* 필수 선택</span>
          </div>

          <!-- 1-1. 참여자 이름 또는 닉네임 -->
          <div>
            <label for="participantName" class="block text-sm font-semibold text-slate-700 mb-1.5">
              1. 참여자 이름 또는 닉네임 <span class="text-xs font-normal text-slate-400">(선택 입력 · 미입력 시 '익명'으로 등록)</span>
            </label>
            <input
              type="text"
              id="participantName"
              maxlength="24"
              placeholder="예: 마케팅팀 김지은 또는 익명"
              class="w-full sm:max-w-md px-4 py-2.5 rounded-xl bg-ivory-50 border border-ivory-300 text-sm text-slate-800 placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-sage-500/30 focus:border-sage-500 transition"
            />
          </div>

          <!-- 1-2. 본인 피부 타입 (단일 선택) -->
          <div id="field-skinType">
            <label class="block text-sm font-semibold text-slate-700 mb-2">
              2. 본인 피부 타입 <span class="text-terracotta-500">*</span>
            </label>
            <div class="grid grid-cols-2 sm:grid-cols-5 gap-2.5" id="skinTypeOptions">
              <button type="button" data-value="건성" onclick="selectSingleOption('skinType', this)" class="single-opt-btn py-2.5 px-3 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition text-center">
                건성
              </button>
              <button type="button" data-value="지성" onclick="selectSingleOption('skinType', this)" class="single-opt-btn py-2.5 px-3 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition text-center">
                지성
              </button>
              <button type="button" data-value="복합성" onclick="selectSingleOption('skinType', this)" class="single-opt-btn py-2.5 px-3 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition text-center">
                복합성
              </button>
              <button type="button" data-value="수부지" onclick="selectSingleOption('skinType', this)" class="single-opt-btn py-2.5 px-3 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition text-center">
                수부지
              </button>
              <button type="button" data-value="민감성" onclick="selectSingleOption('skinType', this)" class="single-opt-btn col-span-2 sm:col-span-1 py-2.5 px-3 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition text-center">
                민감성
              </button>
            </div>
          </div>

          <!-- 1-3. 3일간 주 사용 시간대 (다중 선택) -->
          <div id="field-usageTimes">
            <label class="block text-sm font-semibold text-slate-700 mb-1">
              3. 3일간 주 사용 시간대 <span class="text-terracotta-500">*</span>
              <span class="text-xs font-normal text-slate-400 ml-1">(복수 선택 가능)</span>
            </label>
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-2.5 mt-2" id="usageTimeOptions">
              <button type="button" data-value="아침 스킨케어" onclick="toggleMultiOption('usageTimes', this)" class="multi-opt-btn flex items-center justify-center gap-2 py-2.5 px-4 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition">
                <span class="check-circle w-4 h-4 rounded-full border border-slate-300 flex items-center justify-center text-[10px]">✓</span>
                <span>아침 스킨케어</span>
              </button>
              <button type="button" data-value="저녁 나이트케어" onclick="toggleMultiOption('usageTimes', this)" class="multi-opt-btn flex items-center justify-center gap-2 py-2.5 px-4 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition">
                <span class="check-circle w-4 h-4 rounded-full border border-slate-300 flex items-center justify-center text-[10px]">✓</span>
                <span>저녁 나이트케어</span>
              </button>
              <button type="button" data-value="메이크업 전" onclick="toggleMultiOption('usageTimes', this)" class="multi-opt-btn flex items-center justify-center gap-2 py-2.5 px-4 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition">
                <span class="check-circle w-4 h-4 rounded-full border border-slate-300 flex items-center justify-center text-[10px]">✓</span>
                <span>메이크업 전</span>
              </button>
            </div>
          </div>
        </div>

        <!-- SECTION 2: 제형 및 사용감 평가 (5점 척도) -->
        <div class="bg-white rounded-2xl p-5 sm:p-7 border border-ivory-300 shadow-sm space-y-6">
          <div class="border-b border-ivory-200 pb-3.5">
            <span class="text-xs font-bold uppercase tracking-wider text-sage-600">SECTION 02</span>
            <h3 class="text-lg font-bold text-slate-800 mt-0.5">제형 및 사용감 평가</h3>
            <p class="text-xs text-slate-500 mt-1">1점(불만족)부터 5점(매우 만족)까지 해당되는 점수를 눌러주세요.</p>
          </div>

          <div id="sensoryRatingsContainer" class="space-y-6">
            <!-- Rendered dynamically via JS for consistent interactive UI -->
          </div>
        </div>

        <!-- SECTION 3: 3일 사용 후 피부 체감 -->
        <div class="bg-white rounded-2xl p-5 sm:p-7 border border-ivory-300 shadow-sm space-y-6">
          <div class="border-b border-ivory-200 pb-3.5">
            <span class="text-xs font-bold uppercase tracking-wider text-sage-600">SECTION 03</span>
            <h3 class="text-lg font-bold text-slate-800 mt-0.5">3일 사용 후 피부 체감 및 구매 의사</h3>
          </div>

          <div id="efficacyRatingsContainer" class="space-y-6">
            <!-- Rendered dynamically via JS -->
          </div>

          <!-- 피부 자극 및 트러블 여부 (단일 선택) -->
          <div id="field-irritation" class="pt-2 border-t border-ivory-200">
            <label class="block text-sm font-semibold text-slate-700 mb-2">
              7. 피부 자극 및 트러블 여부 <span class="text-terracotta-500">*</span>
            </label>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5" id="irritationOptions">
              <button type="button" data-value="전혀 없음(순함)" onclick="selectSingleOption('irritation', this)" class="single-opt-btn flex items-center justify-between py-3 px-4 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-sage-400 transition text-left">
                <span>전혀 없음 (순하고 편안함)</span>
                <span class="text-xs px-2 py-0.5 rounded-full bg-sage-100 text-sage-700 font-semibold">무자극</span>
              </button>
              <button type="button" data-value="약간의 따가움·열감" onclick="selectSingleOption('irritation', this)" class="single-opt-btn flex items-center justify-between py-3 px-4 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-terracotta-400 transition text-left">
                <span>약간의 따가움 · 일시적 열감</span>
                <span class="text-xs px-2 py-0.5 rounded-full bg-amber-100 text-amber-800 font-semibold">경미</span>
              </button>
              <button type="button" data-value="좁쌀·트러블 발생" onclick="selectSingleOption('irritation', this)" class="single-opt-btn flex items-center justify-between py-3 px-4 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-terracotta-400 transition text-left">
                <span>좁쌀 · 붉은기 · 트러블 발생</span>
                <span class="text-xs px-2 py-0.5 rounded-full bg-terracotta-100 text-terracotta-600 font-semibold">주의</span>
              </button>
              <button type="button" data-value="기타 불편함" onclick="selectSingleOption('irritation', this)" class="single-opt-btn flex items-center justify-between py-3 px-4 rounded-xl border border-ivory-300 bg-ivory-50 text-sm font-medium text-slate-700 hover:border-terracotta-400 transition text-left">
                <span>기타 불편함 (눈시림, 밀림 등)</span>
                <span class="text-xs px-2 py-0.5 rounded-full bg-slate-200 text-slate-700 font-semibold">기타</span>
              </button>
            </div>
          </div>

          <!-- 정식 출시 시 구매·추천 의사 (5점 척도) -->
          <div id="purchaseRatingContainer" class="pt-2 border-t border-ivory-200">
            <!-- Rendered dynamically via JS -->
          </div>
        </div>

        <!-- SECTION 4: 주관식 상세 피드백 -->
        <div class="bg-white rounded-2xl p-5 sm:p-7 border border-ivory-300 shadow-sm space-y-5">
          <div class="border-b border-ivory-200 pb-3.5">
            <span class="text-xs font-bold uppercase tracking-wider text-sage-600">SECTION 04</span>
            <h3 class="text-lg font-bold text-slate-800 mt-0.5">주관식 상세 피드백 (R&amp;D 전달용)</h3>
            <p class="text-xs text-slate-500 mt-1">자유롭게 적어주신 의견은 처방 개선 회의에 그대로 공유됩니다.</p>
          </div>

          <div>
            <label for="prosText" class="block text-sm font-semibold text-slate-700 mb-1.5">
              9. 가장 마음에 들었던 점 (장점)
            </label>
            <textarea
              id="prosText"
              rows="3"
              placeholder="예: 바르자마자 겉돌지 않고 쫀쫀하게 흡수되어 다음 날 아침까지 속당김이 없었어요."
              class="w-full px-4 py-3 rounded-xl bg-ivory-50 border border-ivory-300 text-sm text-slate-800 placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-sage-500/30 focus:border-sage-500 transition resize-y"
            ></textarea>
          </div>

          <div>
            <label for="consText" class="block text-sm font-semibold text-slate-700 mb-1.5">
              10. 보완이 필요한 점 (제형, 향, 사용감 등 개선 의견)
            </label>
            <textarea
              id="consText"
              rows="3"
              placeholder="예: 아침에 선크림 전에 바르면 살짝 밀리는 느낌이 있어 유분감을 조금만 줄이면 더 좋을 것 같아요."
              class="w-full px-4 py-3 rounded-xl bg-ivory-50 border border-ivory-300 text-sm text-slate-800 placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-sage-500/30 focus:border-sage-500 transition resize-y"
            ></textarea>
          </div>
        </div>

        <!-- Submit Button Area -->
        <div class="pt-2 pb-6 flex flex-col sm:flex-row items-center justify-end gap-3">
          <button
            type="button"
            onclick="resetSurveyForm()"
            class="w-full sm:w-auto px-5 py-3.5 rounded-xl border border-ivory-300 bg-white text-sm font-semibold text-slate-600 hover:bg-ivory-100 transition"
          >
            입력 초기화
          </button>
          <button
            type="submit"
            id="submitSurveyBtn"
            class="w-full sm:w-auto sm:min-w-[240px] px-7 py-3.5 rounded-xl bg-sage-600 hover:bg-sage-700 text-white text-sm sm:text-base font-bold shadow-md shadow-sage-600/20 flex items-center justify-center gap-2 transition disabled:opacity-60 disabled:cursor-not-allowed"
          >
            <span id="submitBtnText">품평 결과 제출하기</span>
            <svg id="submitBtnIcon" class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M14 5l7 7m0 0l-7 7m7-7H3" />
            </svg>
          </button>
        </div>
      </form>
    </section>

    <!-- =================================================================== -->
    <!-- TAB 2: LIVE REPORT DASHBOARD (실시간 결과 리포트)                      -->
    <!-- =================================================================== -->
    <section id="reportView" class="hidden space-y-6">
      <!-- Top Filter & Export Bar -->
      <div class="bg-white rounded-2xl p-4 sm:p-5 border border-ivory-300 shadow-sm flex flex-col lg:flex-row lg:items-center lg:justify-between gap-4">
        <!-- Skin Type Filter Pills -->
        <div class="space-y-1.5">
          <span class="text-xs font-semibold text-slate-500 block">피부 타입별 데이터 필터</span>
          <div class="flex flex-wrap gap-1.5" id="reportFilterPills">
            <!-- Rendered dynamically -->
          </div>
        </div>

        <!-- Utility Buttons: Share Link & CSV Export -->
        <div class="flex flex-wrap items-center gap-2 pt-2 lg:pt-0 border-t lg:border-t-0 border-ivory-200">
          <button
            type="button"
            onclick="copyShareLink()"
            class="inline-flex items-center gap-1.5 px-3.5 py-2.5 rounded-xl border border-ivory-300 bg-ivory-50 hover:bg-ivory-200 text-xs sm:text-sm font-semibold text-slate-700 transition"
          >
            <svg class="w-4 h-4 text-sage-600" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z" />
            </svg>
            <span>설문 링크 복사</span>
          </button>

          <button
            type="button"
            onclick="exportToCSV()"
            class="inline-flex items-center gap-1.5 px-4 py-2.5 rounded-xl bg-sage-600 hover:bg-sage-700 text-white text-xs sm:text-sm font-semibold shadow-sm transition"
          >
            <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
            </svg>
            <span>CSV(엑셀) 다운로드</span>
          </button>
        </div>
      </div>

      <!-- Empty State (Shown when 0 responses match) -->
      <div id="reportEmptyState" class="hidden bg-white rounded-2xl p-10 border border-ivory-300 text-center space-y-3">
        <div class="w-12 h-12 rounded-2xl bg-ivory-200 text-slate-400 flex items-center justify-center mx-auto">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" stroke-width="1.8" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
          </svg>
        </div>
        <h3 class="text-base font-bold text-slate-800" id="emptyStateTitle">아직 등록된 품평 데이터가 없습니다</h3>
        <p class="text-sm text-slate-500 max-w-md mx-auto">
          첫 번째 품평 후기를 작성하시면 항목별 평균 점수와 피부 타입별 통계가 실시간으로 자동 집계됩니다.
        </p>
        <div class="pt-2">
          <button
            type="button"
            onclick="switchTab('survey')"
            class="inline-flex items-center gap-1.5 px-5 py-2.5 rounded-xl bg-sage-600 text-white text-sm font-semibold hover:bg-sage-700 transition"
          >
            <span>설문 작성하러 가기</span>
          </button>
        </div>
      </div>

      <!-- Dashboard Content Wrapper (Hidden when empty) -->
      <div id="reportDashboardContent" class="space-y-6">
        <!-- 1. KPI Summary Cards -->
        <div class="grid grid-cols-2 lg:grid-cols-4 gap-3 sm:gap-4">
          <!-- KPI 1: 총 참여 인원수 -->
          <div class="bg-white rounded-2xl p-4 sm:p-5 border border-ivory-300 shadow-sm">
            <div class="flex items-center justify-between">
              <span class="text-xs font-semibold text-slate-500">총 참여 인원수</span>
              <span class="w-7 h-7 rounded-lg bg-ivory-200 text-slate-600 flex items-center justify-center">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0z" />
                </svg>
              </span>
            </div>
            <div class="mt-2.5 flex items-baseline gap-1.5">
              <span id="kpiTotalCount" class="text-2xl sm:text-3xl font-bold font-display text-slate-800">0</span>
              <span class="text-sm font-medium text-slate-500">명</span>
            </div>
            <p id="kpiCountSubtext" class="text-xs text-slate-400 mt-1">전체 응답 기준</p>
          </div>

          <!-- KPI 2: 종합 만족도 평균 -->
          <div class="bg-white rounded-2xl p-4 sm:p-5 border border-ivory-300 shadow-sm">
            <div class="flex items-center justify-between">
              <span class="text-xs font-semibold text-slate-500">종합 만족도 평균</span>
              <span class="w-7 h-7 rounded-lg bg-sage-50 text-sage-600 flex items-center justify-center">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
                  <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
                </svg>
              </span>
            </div>
            <div class="mt-2.5 flex items-baseline gap-1">
              <span id="kpiOverallAvg" class="text-2xl sm:text-3xl font-bold font-display text-sage-700">0.0</span>
              <span class="text-sm font-medium text-slate-400">/ 5.0</span>
            </div>
            <p class="text-xs text-slate-400 mt-1">제형·효능 6개 항목 평균</p>
          </div>

          <!-- KPI 3: 트러블 무자극 비율 -->
          <div class="bg-white rounded-2xl p-4 sm:p-5 border border-ivory-300 shadow-sm">
            <div class="flex items-center justify-between">
              <span class="text-xs font-semibold text-slate-500">트러블 무자극 비율</span>
              <span class="w-7 h-7 rounded-lg bg-emerald-50 text-emerald-600 flex items-center justify-center">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z" />
                </svg>
              </span>
            </div>
            <div class="mt-2.5 flex items-baseline gap-1">
              <span id="kpiNoIrritationRate" class="text-2xl sm:text-3xl font-bold font-display text-slate-800">0</span>
              <span class="text-sm font-medium text-slate-500">%</span>
            </div>
            <p id="kpiNoIrritationSub" class="text-xs text-slate-400 mt-1">'전혀 없음(순함)' 응답</p>
          </div>

          <!-- KPI 4: 재구매·출시 추천율 -->
          <div class="bg-white rounded-2xl p-4 sm:p-5 border border-ivory-300 shadow-sm">
            <div class="flex items-center justify-between">
              <span class="text-xs font-semibold text-slate-500">출시 구매·추천율</span>
              <span class="w-7 h-7 rounded-lg bg-terracotta-50 text-terracotta-500 flex items-center justify-center">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z" />
                </svg>
              </span>
            </div>
            <div class="mt-2.5 flex items-baseline gap-1">
              <span id="kpiRecommendRate" class="text-2xl sm:text-3xl font-bold font-display text-slate-800">0</span>
              <span class="text-sm font-medium text-slate-500">%</span>
            </div>
            <p id="kpiRecommendSub" class="text-xs text-slate-400 mt-1">4점 이상 긍정 (평균 0.0점)</p>
          </div>
        </div>

        <!-- 2. Visual Charts Section -->
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-5">
          <!-- Left 7 Cols: Category Average Horizontal Bar Chart -->
          <div class="lg:col-span-7 bg-white rounded-2xl p-5 sm:p-6 border border-ivory-300 shadow-sm flex flex-col justify-between">
            <div class="flex items-center justify-between mb-4">
              <div>
                <h3 class="text-base font-bold text-slate-800">항목별 평균 점수 비교</h3>
                <p class="text-xs text-slate-500 mt-0.5">5점 만점 기준 제형·보습·구매의사 항목별 지표</p>
              </div>
              <div class="inline-flex rounded-lg bg-ivory-200 p-0.5 text-xs font-semibold">
                <button type="button" id="chartTypeBarBtn" onclick="setCategoryChartType('bar')" class="px-2.5 py-1 rounded-md bg-white text-sage-700 shadow-sm transition">
                  막대
                </button>
                <button type="button" id="chartTypeRadarBtn" onclick="setCategoryChartType('radar')" class="px-2.5 py-1 rounded-md text-slate-600 hover:text-slate-800 transition">
                  레이더
                </button>
              </div>
            </div>
            <div class="relative w-full h-[270px] sm:h-[300px]">
              <canvas id="categoryScoresChart"></canvas>
            </div>
          </div>

          <!-- Right 5 Cols: Skin Type Doughnut & Irritation Status -->
          <div class="lg:col-span-5 bg-white rounded-2xl p-5 sm:p-6 border border-ivory-300 shadow-sm flex flex-col justify-between space-y-5">
            <div>
              <h3 class="text-base font-bold text-slate-800">참여자 피부 타입 분포</h3>
              <p class="text-xs text-slate-500 mt-0.5">피부 타입별 응답 구성 비율</p>
              <div class="relative w-full h-[175px] mt-3">
                <canvas id="skinTypeChart"></canvas>
              </div>
            </div>

            <!-- Irritation Status Progress Bars -->
            <div class="pt-4 border-t border-ivory-200">
              <h4 class="text-xs font-bold uppercase tracking-wider text-slate-500 mb-2.5">피부 자극 및 트러블 현황</h4>
              <div id="irritationBreakdownList" class="space-y-2">
                <!-- Rendered dynamically -->
              </div>
            </div>
          </div>
        </div>

        <!-- 3. Qualitative Feedback Cards (주관식 피드백 모아보기) -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-5">
          <!-- 좋았던 점 (장점) -->
          <div class="bg-white rounded-2xl p-5 sm:p-6 border border-ivory-300 shadow-sm flex flex-col">
            <div class="flex items-center justify-between pb-3.5 border-b border-ivory-200">
              <div class="flex items-center gap-2">
                <span class="w-2.5 h-2.5 rounded-full bg-sage-500"></span>
                <h3 class="text-base font-bold text-slate-800">가장 마음에 들었던 점 (장점)</h3>
              </div>
              <span id="prosCountBadge" class="text-xs font-semibold px-2.5 py-0.5 rounded-full bg-sage-50 text-sage-700">0건</span>
            </div>
            <div id="prosFeedbackList" class="mt-4 space-y-3 max-h-[380px] overflow-y-auto pr-1">
              <!-- Cards rendered dynamically -->
            </div>
          </div>

          <!-- 보완/개선 의견 -->
          <div class="bg-white rounded-2xl p-5 sm:p-6 border border-ivory-300 shadow-sm flex flex-col">
            <div class="flex items-center justify-between pb-3.5 border-b border-ivory-200">
              <div class="flex items-center gap-2">
                <span class="w-2.5 h-2.5 rounded-full bg-terracotta-500"></span>
                <h3 class="text-base font-bold text-slate-800">보완 및 개선 의견 (R&amp;D 참고)</h3>
              </div>
              <span id="consCountBadge" class="text-xs font-semibold px-2.5 py-0.5 rounded-full bg-terracotta-50 text-terracotta-600">0건</span>
            </div>
            <div id="consFeedbackList" class="mt-4 space-y-3 max-h-[380px] overflow-y-auto pr-1">
              <!-- Cards rendered dynamically -->
            </div>
          </div>
        </div>

        <!-- 4. Raw Data Table & Individual Entry Delete -->
        <div class="bg-white rounded-2xl border border-ivory-300 shadow-sm overflow-hidden">
          <div class="p-5 sm:px-6 border-b border-ivory-200 flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2">
            <div>
              <h3 class="text-base font-bold text-slate-800">개별 응답 상세 내역</h3>
              <p class="text-xs text-slate-500 mt-0.5">테스트 제출 건은 우측 휴지통 아이콘을 눌러 개별 삭제할 수 있습니다.</p>
            </div>
            <span id="tableRowCountLabel" class="text-xs font-semibold text-slate-500">총 0건 표시 중</span>
          </div>

          <div class="overflow-x-auto">
            <table class="w-full text-left border-collapse min-w-[780px]">
              <thead>
                <tr class="bg-ivory-100/90 border-b border-ivory-300 text-[11px] font-bold text-slate-500 uppercase tracking-wider">
                  <th class="py-3 px-4">참여자</th>
                  <th class="py-3 px-3">피부타입</th>
                  <th class="py-3 px-3">주 사용시간</th>
                  <th class="py-3 px-3 text-center">발림/흡수/마무리/향</th>
                  <th class="py-3 px-3 text-center">보습/피부결</th>
                  <th class="py-3 px-3 text-center">구매의사</th>
                  <th class="py-3 px-3">자극여부</th>
                  <th class="py-3 px-3">작성일시</th>
                  <th class="py-3 px-3 text-center">관리</th>
                </tr>
              </thead>
              <tbody id="rawResponsesTableBody" class="divide-y divide-ivory-200 text-xs sm:text-sm text-slate-700">
                <!-- Rows rendered dynamically -->
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </section>
  </main>

  <!-- Footer -->
  <footer class="border-t border-ivory-300 bg-white/70 py-5 px-4 text-center text-xs text-slate-400">
    R&amp;D Formulation Lab · 신규 개발 크림 3일 사용 품평회 통합 설문 시스템
  </footer>

  <!-- ===================================================================== -->
  <!-- CUSTOM MODALS & TOAST (NO alert() OR confirm())                       -->
  <!-- ===================================================================== -->

  <!-- Submission Success Modal -->
  <div id="successModal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/45 backdrop-blur-xs">
    <div class="bg-white rounded-3xl max-w-md w-full p-6 sm:p-7 border border-ivory-300 shadow-2xl text-center space-y-4 transform transition-all">
      <div class="w-14 h-14 rounded-2xl bg-sage-100 text-sage-600 flex items-center justify-center mx-auto">
        <svg class="w-8 h-8" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7" />
        </svg>
      </div>
      <div>
        <h3 class="text-xl font-bold text-slate-800">품평 의견이 등록되었습니다!</h3>
        <p class="text-sm text-slate-600 mt-1.5 leading-relaxed">
          소중한 3일 사용 후기가 실시간 통합 리포트에 즉시 반영되었습니다. 다른 임직원들의 평가 결과도 지금 바로 확인해보세요.
        </p>
      </div>
      <div class="pt-2 flex flex-col sm:flex-row gap-2.5">
        <button
          type="button"
          onclick="closeSuccessModalAndGoReport()"
          class="flex-1 py-3 px-4 rounded-xl bg-sage-600 hover:bg-sage-700 text-white text-sm font-bold shadow-sm transition"
        >
          실시간 결과 리포트 보기
        </button>
        <button
          type="button"
          onclick="closeSuccessModalAndReset()"
          class="py-3 px-4 rounded-xl border border-ivory-300 bg-ivory-50 hover:bg-ivory-200 text-slate-700 text-sm font-semibold transition"
        >
          추가 작성하기
        </button>
      </div>
    </div>
  </div>

  <!-- Delete Confirmation Modal -->
  <div id="deleteConfirmModal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-900/45 backdrop-blur-xs">
    <div class="bg-white rounded-2xl max-w-sm w-full p-6 border border-ivory-300 shadow-2xl space-y-4">
      <div class="flex items-center gap-3 text-terracotta-600">
        <div class="w-10 h-10 rounded-xl bg-terracotta-50 flex items-center justify-center shrink-0">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
          </svg>
        </div>
        <div>
          <h3 class="text-base font-bold text-slate-800">응답 내역 삭제</h3>
          <p class="text-xs text-slate-500">테스트 제출 데이터를 정리할 수 있습니다.</p>
        </div>
      </div>
      <p class="text-sm text-slate-600 leading-relaxed">
        선택하신 <span id="deleteTargetName" class="font-bold text-slate-800">익명</span>님의 품평 데이터를 삭제하시겠습니까? 삭제된 데이터는 복구할 수 없습니다.
      </p>
      <div class="flex items-center justify-end gap-2 pt-2">
        <button
          type="button"
          onclick="closeDeleteModal()"
          class="px-4 py-2.5 rounded-xl border border-ivory-300 bg-white hover:bg-ivory-100 text-xs sm:text-sm font-semibold text-slate-600 transition"
        >
          취소
        </button>
        <button
          type="button"
          id="confirmDeleteBtn"
          onclick="executeDeleteEntry()"
          class="px-4 py-2.5 rounded-xl bg-terracotta-500 hover:bg-terracotta-600 text-white text-xs sm:text-sm font-bold shadow-sm transition"
        >
          삭제하기
        </button>
      </div>
    </div>
  </div>

  <!-- Floating Toast Notification -->
  <div id="toastNotification" class="hidden fixed bottom-5 left-1/2 -translate-x-1/2 z-50 px-4 py-3 rounded-2xl bg-slate-800 text-white text-xs sm:text-sm font-medium shadow-xl flex items-center gap-2.5 max-w-[92vw]">
    <span class="w-2 h-2 rounded-full bg-sage-400 shrink-0"></span>
    <span id="toastMessage">알림 메시지</span>
  </div>

  <!-- ===================================================================== -->
  <!-- FIREBASE & APPLICATION LOGIC                                          -->
  <!-- ===================================================================== -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import {
      getFirestore,
      collection,
      addDoc,
      deleteDoc,
      doc,
      onSnapshot,
      serverTimestamp
    } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

    // -----------------------------------------------------------------------
    // 1. Survey Configuration & State
    // -----------------------------------------------------------------------
    const RATING_QUESTIONS = [
      {
        key: "spreadability",
        section: "sensory",
        number: "4",
        title: "발림성",
        minLabel: "1: 뻑뻑하고 겉돎",
        maxLabel: "5: 매우 부드럽게 발림"
      },
      {
        key: "absorption",
        section: "sensory",
        number: "5",
        title: "흡수력",
        minLabel: "1: 겉돌고 흡수가 느림",
        maxLabel: "5: 피부에 빠르게 쏙 흡수됨"
      },
      {
        key: "finish",
        section: "sensory",
        number: "6",
        title: "마무리감 / 끈적임",
        minLabel: "1: 많이 끈적이고 답답함",
        maxLabel: "5: 산뜻하고 촉촉함"
      },
      {
        key: "scent",
        section: "sensory",
        number: "7",
        title: "향 만족도",
        minLabel: "1: 불호 / 원료취·향이 강함",
        maxLabel: "5: 은은하고 호감 가는 향"
      },
      {
        key: "moistureRetention",
        section: "efficacy",
        number: "5",
        title: "보습 지속력",
        minLabel: "1: 바른 뒤 금방 건조해짐",
        maxLabel: "5: 다음 날까지 촉촉함 유지"
      },
      {
        key: "skinTextureSoothing",
        section: "efficacy",
        number: "6",
        title: "피부결 / 진정 개선 체감",
        minLabel: "1: 사용 전후 변화 없음",
        maxLabel: "5: 매끄럽고 편안하게 진정됨"
      },
      {
        key: "purchaseIntent",
        section: "purchase",
        number: "8",
        title: "정식 출시 시 구매 · 추천 의사",
        minLabel: "1: 전혀 없음",
        maxLabel: "5: 무조건 구매·추천하고 싶음"
      }
    ];

    const SKIN_TYPES = ["전체", "건성", "지성", "복합성", "수부지", "민감성"];
    const IRRITATION_LEVELS = [
      { label: "전혀 없음(순함)", colorClass: "bg-sage-500", badgeClass: "bg-sage-50 text-sage-700" },
      { label: "약간의 따가움·열감", colorClass: "bg-amber-400", badgeClass: "bg-amber-50 text-amber-700" },
      { label: "좁쌀·트러블 발생", colorClass: "bg-terracotta-500", badgeClass: "bg-terracotta-50 text-terracotta-600" },
      { label: "기타 불편함", colorClass: "bg-slate-400", badgeClass: "bg-slate-100 text-slate-600" }
    ];

    // Form State
    const formState = {
      skinType: "",
      usageTimes: [],
      spreadability: 0,
      absorption: 0,
      finish: 0,
      scent: 0,
      moistureRetention: 0,
      skinTextureSoothing: 0,
      irritation: "",
      purchaseIntent: 0
    };

    // Live Data State
    let allResponses = [];
    let activeSkinFilter = "전체";
    let categoryChartType = "bar";
    let categoryChartInstance = null;
    let skinTypeChartInstance = null;
    let pendingDeleteId = null;

    // Firebase References
    let db = null;
    let auth = null;
    let currentUser = null;
    const appId = typeof __app_id !== "undefined" ? __app_id : "default-app-id";
    const COLLECTION_NAME = "cream_sample_reviews";

    // -----------------------------------------------------------------------
    // 2. Render Interactive Rating Controls
    // -----------------------------------------------------------------------
    function renderRatingControls() {
      const sensoryContainer = document.getElementById("sensoryRatingsContainer");
      const efficacyContainer = document.getElementById("efficacyRatingsContainer");
      const purchaseContainer = document.getElementById("purchaseRatingContainer");

      sensoryContainer.innerHTML = "";
      efficacyContainer.innerHTML = "";
      purchaseContainer.innerHTML = "";

      RATING_QUESTIONS.forEach((q, idx) => {
        const displayNum = idx + 4; // Questions 4 through 10 sequence
        const questionEl = document.createElement("div");
        questionEl.id = `field-${q.key}`;
        questionEl.className = "space-y-2.5";

        questionEl.innerHTML = `
          <div class="flex flex-wrap items-baseline justify-between gap-1">
            <label class="text-sm font-semibold text-slate-700">
              ${q.title} <span class="text-terracotta-500">*</span>
            </label>
            <span id="scoreLabel-${q.key}" class="text-xs font-semibold text-sage-600">미선택</span>
          </div>
          <div class="grid grid-cols-5 gap-2" role="radiogroup" aria-label="${q.title}">
            ${[1, 2, 3, 4, 5].map(score => `
              <button
                type="button"
                data-key="${q.key}"
                data-score="${score}"
                onclick="selectRatingScore('${q.key}', ${score})"
                class="rating-btn rating-opt-${q.key} py-3 rounded-xl border border-ivory-300 bg-ivory-50 hover:border-sage-400 text-sm sm:text-base font-bold font-display text-slate-600 flex flex-col items-center justify-center gap-0.5"
              >
                <span>${score}</span>
              </button>
            `).join("")}
          </div>
          <div class="flex items-center justify-between text-[11px] sm:text-xs text-slate-400 px-0.5">
            <span>${q.minLabel}</span>
            <span>${q.maxLabel}</span>
          </div>
        `;

        if (q.section === "sensory") {
          sensoryContainer.appendChild(questionEl);
        } else if (q.section === "efficacy") {
          efficacyContainer.appendChild(questionEl);
        } else {
          purchaseContainer.appendChild(questionEl);
        }
      });
    }

    // -----------------------------------------------------------------------
    // 3. Interactive Form Handlers (Exposed to window)
    // -----------------------------------------------------------------------
    window.selectSingleOption = function (field, btnEl) {
      const value = btnEl.getAttribute("data-value");
      formState[field] = value;

      const parent = btnEl.parentElement;
      parent.querySelectorAll(".single-opt-btn").forEach(btn => {
        btn.classList.remove("bg-sage-600", "text-white", "border-sage-600", "shadow-sm");
        btn.classList.add("bg-ivory-50", "text-slate-700", "border-ivory-300");
      });

      btnEl.classList.remove("bg-ivory-50", "text-slate-700", "border-ivory-300");
      btnEl.classList.add("bg-sage-600", "text-white", "border-sage-600", "shadow-sm");
      hideValidationAlert();
    };

    window.toggleMultiOption = function (field, btnEl) {
      const value = btnEl.getAttribute("data-value");
      const idx = formState[field].indexOf(value);
      const circle = btnEl.querySelector(".check-circle");

      if (idx === -1) {
        formState[field].push(value);
        btnEl.classList.remove("bg-ivory-50", "text-slate-700", "border-ivory-300");
        btnEl.classList.add("bg-sage-50", "text-sage-700", "border-sage-500", "font-semibold");
        if (circle) {
          circle.classList.remove("border-slate-300");
          circle.classList.add("bg-sage-600", "border-sage-600", "text-white");
        }
      } else {
        formState[field].splice(idx, 1);
        btnEl.classList.remove("bg-sage-50", "text-sage-700", "border-sage-500", "font-semibold");
        btnEl.classList.add("bg-ivory-50", "text-slate-700", "border-ivory-300");
        if (circle) {
          circle.classList.remove("bg-sage-600", "border-sage-600", "text-white");
          circle.classList.add("border-slate-300");
        }
      }
      hideValidationAlert();
    };

    window.selectRatingScore = function (key, score) {
      formState[key] = score;
      const buttons = document.querySelectorAll(`.rating-opt-${key}`);
      buttons.forEach(btn => {
        const btnScore = Number(btn.getAttribute("data-score"));
        if (btnScore === score) {
          btn.classList.remove("bg-ivory-50", "text-slate-600", "border-ivory-300");
          btn.classList.add("bg-sage-600", "text-white", "border-sage-600", "shadow-sm");
        } else {
          btn.classList.remove("bg-sage-600", "text-white", "border-sage-600", "shadow-sm");
          btn.classList.add("bg-ivory-50", "text-slate-600", "border-ivory-300");
        }
      });

      const scoreTextMap = {
        1: "1점 (매우 아쉽다)",
        2: "2점 (아쉽다)",
        3: "3점 (보통이다)",
        4: "4점 (만족한다)",
        5: "5점 (매우 만족한다)"
      };
      const labelEl = document.getElementById(`scoreLabel-${key}`);
      if (labelEl) {
        labelEl.textContent = scoreTextMap[score];
      }
      hideValidationAlert();
    };

    function showValidationAlert(msg, targetElementId) {
      const alertBox = document.getElementById("formValidationAlert");
      const msgEl = document.getElementById("formValidationMessage");
      msgEl.textContent = msg;
      alertBox.classList.remove("hidden");

      if (targetElementId) {
        const target = document.getElementById(targetElementId);
        if (target) {
          target.scrollIntoView({ behavior: "smooth", block: "center" });
        }
      } else {
        alertBox.scrollIntoView({ behavior: "smooth", block: "center" });
      }
    }

    function hideValidationAlert() {
      document.getElementById("formValidationAlert").classList.add("hidden");
    }

    window.resetSurveyForm = function () {
      document.getElementById("creamSurveyForm").reset();
      formState.skinType = "";
      formState.usageTimes = [];
      formState.spreadability = 0;
      formState.absorption = 0;
      formState.finish = 0;
      formState.scent = 0;
      formState.moistureRetention = 0;
      formState.skinTextureSoothing = 0;
      formState.irritation = "";
      formState.purchaseIntent = 0;

      // Reset single option buttons
      document.querySelectorAll(".single-opt-btn").forEach(btn => {
        btn.classList.remove("bg-sage-600", "text-white", "border-sage-600", "shadow-sm");
        btn.classList.add("bg-ivory-50", "text-slate-700", "border-ivory-300");
      });

      // Reset multi option buttons
      document.querySelectorAll(".multi-opt-btn").forEach(btn => {
        btn.classList.remove("bg-sage-50", "text-sage-700", "border-sage-500", "font-semibold");
        btn.classList.add("bg-ivory-50", "text-slate-700", "border-ivory-300");
        const circle = btn.querySelector(".check-circle");
        if (circle) {
          circle.classList.remove("bg-sage-600", "border-sage-600", "text-white");
          circle.classList.add("border-slate-300");
        }
      });

      // Reset rating buttons & labels
      RATING_QUESTIONS.forEach(q => {
        document.querySelectorAll(`.rating-opt-${q.key}`).forEach(btn => {
          btn.classList.remove("bg-sage-600", "text-white", "border-sage-600", "shadow-sm");
          btn.classList.add("bg-ivory-50", "text-slate-600", "border-ivory-300");
        });
        const labelEl = document.getElementById(`scoreLabel-${q.key}`);
        if (labelEl) labelEl.textContent = "미선택";
      });

      hideValidationAlert();
    };

    // -----------------------------------------------------------------------
    // 4. Tab Navigation & Modals
    // -----------------------------------------------------------------------
    window.switchTab = function (tabName) {
      const surveyView = document.getElementById("surveyView");
      const reportView = document.getElementById("reportView");
      const btnSurvey = document.getElementById("tabBtnSurvey");
      const btnReport = document.getElementById("tabBtnReport");

      if (tabName === "survey") {
        surveyView.classList.remove("hidden");
        reportView.classList.add("hidden");

        btnSurvey.classList.add("bg-white", "text-sage-700", "shadow-sm");
        btnSurvey.classList.remove("text-slate-600");

        btnReport.classList.remove("bg-white", "text-sage-700", "shadow-sm");
        btnReport.classList.add("text-slate-600");
      } else {
        surveyView.classList.add("hidden");
        reportView.classList.remove("hidden");

        btnReport.classList.add("bg-white", "text-sage-700", "shadow-sm");
        btnReport.classList.remove("text-slate-600");

        btnSurvey.classList.remove("bg-white", "text-sage-700", "shadow-sm");
        btnSurvey.classList.add("text-slate-600");

        renderReportDashboard();
      }
      window.scrollTo({ top: 0, behavior: "smooth" });
    };

    window.closeSuccessModalAndGoReport = function () {
      document.getElementById("successModal").classList.add("hidden");
      resetSurveyForm();
      switchTab("report");
    };

    window.closeSuccessModalAndReset = function () {
      document.getElementById("successModal").classList.add("hidden");
      resetSurveyForm();
      window.scrollTo({ top: 0, behavior: "smooth" });
    };

    function showToast(message) {
      const toast = document.getElementById("toastNotification");
      const toastMsg = document.getElementById("toastMessage");
      toastMsg.textContent = message;
      toast.classList.remove("hidden");

      clearTimeout(window._toastTimer);
      window._toastTimer = setTimeout(() => {
        toast.classList.add("hidden");
      }, 3000);
    }

    window.copyShareLink = function () {
      const url = window.location.href;
      const tempInput = document.createElement("input");
      tempInput.value = url;
      document.body.appendChild(tempInput);
      tempInput.select();
      try {
        document.execCommand("copy");
        showToast("설문 링크가 클립보드에 복사되었습니다. 사내 메신저에 공유하세요!");
      } catch (err) {
        showToast("주소창의 URL을 복사하여 공유해 주세요.");
      }
      document.body.removeChild(tempInput);
    };

    // -----------------------------------------------------------------------
    // 5. Firebase Initialization & Real-Time Firestore Listener
    // -----------------------------------------------------------------------
    async function initFirebaseAndSync() {
      const statusBadge = document.getElementById("connectionStatusBadge");
      const statusDot = document.getElementById("connectionDot");
      const statusText = document.getElementById("connectionText");

      try {
        if (typeof __firebase_config === "undefined" || !__firebase_config) {
          throw new Error("Firebase configuration unavailable");
        }

        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        auth = getAuth(app);
        db = getFirestore(app);

        // RULE 3: Authenticate FIRST before any Firestore operations
        if (typeof __initial_auth_token !== "undefined" && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
        currentUser = auth.currentUser;

        if (!currentUser) return;

        statusDot.classList.remove("bg-amber-400", "bg-red-500");
        statusDot.classList.add("bg-emerald-500", "live-dot");
        statusText.textContent = "실시간 통합 서버 연결됨";
        statusBadge.classList.remove("bg-ivory-200", "text-slate-600");
        statusBadge.classList.add("bg-sage-50", "text-sage-700", "border", "border-sage-200");

        // RULE 1 & RULE 2: Strict public collection path, simple query without compound orderBy/where
        const reviewsColRef = collection(db, "artifacts", appId, "public", "data", COLLECTION_NAME);

        onSnapshot(
          reviewsColRef,
          (snapshot) => {
            const items = [];
            snapshot.forEach((docSnap) => {
              items.push({
                id: docSnap.id,
                ...docSnap.data()
              });
            });

            // Sort in JS memory by createdAtMs descending (newest first)
            items.sort((a, b) => (b.createdAtMs || 0) - (a.createdAtMs || 0));
            allResponses = items;

            // Update header badge
            document.getElementById("tabParticipantCount").textContent = `${allResponses.length}명 참여`;

            // Re-render dashboard if currently visible or keep data ready
            renderReportDashboard();
          },
          (error) => {
            console.error("Firestore onSnapshot error:", error);
            statusDot.classList.remove("bg-emerald-500");
            statusDot.classList.add("bg-red-500");
            statusText.textContent = "동기화 오류 발생";
          }
        );
      } catch (err) {
        console.error("Firebase init error:", err);
        statusDot.classList.remove("bg-amber-400");
        statusDot.classList.add("bg-terracotta-500");
        statusText.textContent = "오프라인 로컬 모드";
        renderReportDashboard();
      }
    }

    // -----------------------------------------------------------------------
    // 6. Survey Submission Handler
    // -----------------------------------------------------------------------
    document.getElementById("creamSurveyForm").addEventListener("submit", async function (e) {
      e.preventDefault();

      // Validate required fields
      if (!formState.skinType) {
        showValidationAlert("본인 피부 타입을 선택해 주세요.", "field-skinType");
        return;
      }
      if (formState.usageTimes.length === 0) {
        showValidationAlert("3일간 주 사용 시간대를 1개 이상 선택해 주세요.", "field-usageTimes");
        return;
      }

      const requiredRatings = [
        { key: "spreadability", label: "발림성" },
        { key: "absorption", label: "흡수력" },
        { key: "finish", label: "마무리감/끈적임" },
        { key: "scent", label: "향 만족도" },
        { key: "moistureRetention", label: "보습 지속력" },
        { key: "skinTextureSoothing", label: "피부결/진정 개선 체감" }
      ];

      for (const item of requiredRatings) {
        if (!formState[item.key] || formState[item.key] < 1) {
          showValidationAlert(`'${item.label}' 점수를 선택해 주세요.`, `field-${item.key}`);
          return;
        }
      }

      if (!formState.irritation) {
        showValidationAlert("피부 자극 및 트러블 여부를 선택해 주세요.", "field-irritation");
        return;
      }

      if (!formState.purchaseIntent || formState.purchaseIntent < 1) {
        showValidationAlert("정식 출시 시 구매·추천 의사 점수를 선택해 주세요.", "field-purchaseIntent");
        return;
      }

      const submitBtn = document.getElementById("submitSurveyBtn");
      const submitBtnText = document.getElementById("submitBtnText");
      submitBtn.disabled = true;
      submitBtnText.textContent = "제출 및 데이터 반영 중...";

      const rawName = document.getElementById("participantName").value.trim();
      const pros = document.getElementById("prosText").value.trim();
      const cons = document.getElementById("consText").value.trim();

      const payload = {
        participantName: rawName || "익명",
        skinType: formState.skinType,
        usageTimes: [...formState.usageTimes],
        spreadability: Number(formState.spreadability),
        absorption: Number(formState.absorption),
        finish: Number(formState.finish),
        scent: Number(formState.scent),
        moistureRetention: Number(formState.moistureRetention),
        skinTextureSoothing: Number(formState.skinTextureSoothing),
        irritation: formState.irritation,
        purchaseIntent: Number(formState.purchaseIntent),
        pros: pros,
        cons: cons,
        createdAtMs: Date.now(),
        submittedByUid: currentUser ? currentUser.uid : "local-user"
      };

      try {
        if (db && currentUser) {
          const reviewsColRef = collection(db, "artifacts", appId, "public", "data", COLLECTION_NAME);
          await addDoc(reviewsColRef, {
            ...payload,
            createdAt: serverTimestamp()
          });
        } else {
          // Local memory fallback if offline
          allResponses.unshift({
            id: "local-" + Date.now(),
            ...payload
          });
          document.getElementById("tabParticipantCount").textContent = `${allResponses.length}명 참여`;
          renderReportDashboard();
        }

        document.getElementById("successModal").classList.remove("hidden");
      } catch (err) {
        console.error("Failed to submit survey:", err);
        showValidationAlert("제출 중 네트워크 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.");
      } finally {
        submitBtn.disabled = false;
        submitBtnText.textContent = "품평 결과 제출하기";
      }
    });

    // -----------------------------------------------------------------------
    // 7. Real-Time Report Dashboard Rendering & Filtering
    // -----------------------------------------------------------------------
    window.setSkinFilter = function (skinType) {
      activeSkinFilter = skinType;
      renderReportDashboard();
    };

    window.setCategoryChartType = function (type) {
      categoryChartType = type;
      const barBtn = document.getElementById("chartTypeBarBtn");
      const radarBtn = document.getElementById("chartTypeRadarBtn");

      if (type === "bar") {
        barBtn.classList.add("bg-white", "text-sage-700", "shadow-sm");
        barBtn.classList.remove("text-slate-600");
        radarBtn.classList.remove("bg-white", "text-sage-700", "shadow-sm");
        radarBtn.classList.add("text-slate-600");
      } else {
        radarBtn.classList.add("bg-white", "text-sage-700", "shadow-sm");
        radarBtn.classList.remove("text-slate-600");
        barBtn.classList.remove("bg-white", "text-sage-700", "shadow-sm");
        barBtn.classList.add("text-slate-600");
      }
      renderReportDashboard();
    };

    function getFilteredResponses() {
      if (activeSkinFilter === "전체") return allResponses;
      return allResponses.filter(r => r.skinType === activeSkinFilter);
    }

    function renderFilterPills() {
      const container = document.getElementById("reportFilterPills");
      container.innerHTML = SKIN_TYPES.map(type => {
        const count = type === "전체"
          ? allResponses.length
          : allResponses.filter(r => r.skinType === type).length;
        const isActive = activeSkinFilter === type;

        return `
          <button
            type="button"
            onclick="setSkinFilter('${type}')"
            class="px-3 py-1.5 rounded-xl text-xs font-semibold transition border ${
              isActive
                ? "bg-sage-600 text-white border-sage-600 shadow-xs"
                : "bg-ivory-50 text-slate-600 border-ivory-300 hover:border-sage-400"
            }"
          >
            ${type} <span class="opacity-80 ml-0.5">(${count})</span>
          </button>
        `;
      }).join("");
    }

    function renderReportDashboard() {
      renderFilterPills();

      const filtered = getFilteredResponses();
      const emptyState = document.getElementById("reportEmptyState");
      const dashboardContent = document.getElementById("reportDashboardContent");

      if (filtered.length === 0) {
        emptyState.classList.remove("hidden");
        dashboardContent.classList.add("hidden");
        document.getElementById("emptyStateTitle").textContent =
          activeSkinFilter === "전체"
            ? "아직 등록된 품평 데이터가 없습니다"
            : `'${activeSkinFilter}' 피부 타입의 응답이 아직 없습니다`;
        return;
      }

      emptyState.classList.add("hidden");
      dashboardContent.classList.remove("hidden");

      const total = filtered.length;

      // Calculate averages
      const avg = (key) => filtered.reduce((acc, item) => acc + (Number(item[key]) || 0), 0) / total;

      const avgSpread = avg("spreadability");
      const avgAbsorb = avg("absorption");
      const avgFinish = avg("finish");
      const avgScent = avg("scent");
      const avgMoisture = avg("moistureRetention");
      const avgTexture = avg("skinTextureSoothing");
      const avgPurchase = avg("purchaseIntent");

      const overallAvg = (avgSpread + avgAbsorb + avgFinish + avgScent + avgMoisture + avgTexture) / 6;

      // No irritation rate ('전혀 없음(순함)')
      const noIrritationCount = filtered.filter(r => r.irritation === "전혀 없음(순함)").length;
      const noIrritationRate = Math.round((noIrritationCount / total) * 100);

      // Recommend / repurchase rate (purchaseIntent >= 4)
      const recommendCount = filtered.filter(r => Number(r.purchaseIntent) >= 4).length;
      const recommendRate = Math.round((recommendCount / total) * 100);

      // Update KPI DOM
      document.getElementById("kpiTotalCount").textContent = total;
      document.getElementById("kpiCountSubtext").textContent =
        activeSkinFilter === "전체"
          ? "전체 누적 참여 기준"
          : `전체 ${allResponses.length}명 중 '${activeSkinFilter}' 필터`;

      document.getElementById("kpiOverallAvg").textContent = overallAvg.toFixed(1);
      document.getElementById("kpiNoIrritationRate").textContent = noIrritationRate;
      document.getElementById("kpiNoIrritationSub").textContent = `${total}명 중 ${noIrritationCount}명 무자극`;

      document.getElementById("kpiRecommendRate").textContent = recommendRate;
      document.getElementById("kpiRecommendSub").textContent = `4점 이상 긍정 (평균 ${avgPurchase.toFixed(1)}점)`;

      // Update Charts
      updateCategoryChart([
        Number(avgSpread.toFixed(2)),
        Number(avgAbsorb.toFixed(2)),
        Number(avgFinish.toFixed(2)),
        Number(avgScent.toFixed(2)),
        Number(avgMoisture.toFixed(2)),
        Number(avgTexture.toFixed(2)),
        Number(avgPurchase.toFixed(2))
      ]);

      updateSkinTypeChart(allResponses); // Always show overall skin type distribution for context
      renderIrritationBreakdown(filtered);
      renderQualitativeFeedback(filtered);
      renderRawTable(filtered);
    }

    function updateCategoryChart(scoresArray) {
      const ctx = document.getElementById("categoryScoresChart").getContext("2d");
      const labels = [
        "발림성",
        "흡수력",
        "마무리감(산뜻함)",
        "향 만족도",
        "보습 지속력",
        "피부결·진정 개선",
        "출시 구매·추천 의사"
      ];

      if (categoryChartInstance) {
        categoryChartInstance.destroy();
      }

      const isRadar = categoryChartType === "radar";

      categoryChartInstance = new Chart(ctx, {
        type: isRadar ? "radar" : "bar",
        data: {
          labels,
          datasets: [
            {
              label: "평균 점수 (5점 만점)",
              data: scoresArray,
              backgroundColor: isRadar
                ? "rgba(74, 124, 111, 0.2)"
                : [
                    "rgba(74, 124, 111, 0.85)",
                    "rgba(74, 124, 111, 0.85)",
                    "rgba(74, 124, 111, 0.85)",
                    "rgba(74, 124, 111, 0.85)",
                    "rgba(59, 102, 91, 0.9)",
                    "rgba(59, 102, 91, 0.9)",
                    "rgba(200, 109, 81, 0.85)"
                  ],
              borderColor: isRadar ? "#3B665B" : "transparent",
              borderWidth: isRadar ? 2 : 0,
              borderRadius: isRadar ? 0 : 8,
              barThickness: isRadar ? undefined : 22,
              pointBackgroundColor: "#3B665B",
              pointRadius: isRadar ? 4 : 0
            }
          ]
        },
        options: {
          indexAxis: isRadar ? "x" : "y",
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: { display: false },
            tooltip: {
              backgroundColor: "#1E293B",
              padding: 10,
              cornerRadius: 10,
              callbacks: {
                label: (context) => {
                  const val = isRadar ? context.parsed.r : context.parsed.x;
                  return ` 평균 ${val.toFixed(2)}점 / 5.0점`;
                }
              }
            }
          },
          scales: isRadar
            ? {
                r: {
                  min: 0,
                  max: 5,
                  ticks: { stepSize: 1, font: { size: 10 }, backdropColor: "transparent" },
                  pointLabels: { font: { size: 11, weight: "600" }, color: "#334155" },
                  grid: { color: "#E6E0D5" }
                }
              }
            : {
                x: {
                  min: 0,
                  max: 5,
                  ticks: { stepSize: 1, font: { size: 11 }, color: "#64748B" },
                  grid: { color: "#F3EFE8" }
                },
                y: {
                  ticks: { font: { size: 12, weight: "600" }, color: "#334155" },
                  grid: { display: false }
                }
              }
        }
      });
    }

    function updateSkinTypeChart(responses) {
      const ctx = document.getElementById("skinTypeChart").getContext("2d");
      const types = ["건성", "지성", "복합성", "수부지", "민감성"];
      const counts = types.map(t => responses.filter(r => r.skinType === t).length);

      if (skinTypeChartInstance) {
        skinTypeChartInstance.destroy();
      }

      skinTypeChartInstance = new Chart(ctx, {
        type: "doughnut",
        data: {
          labels: types,
          datasets: [
            {
              data: counts,
              backgroundColor: [
                "#3B665B", // 건성 - Sage Dark
                "#6B9E8F", // 지성 - Sage Light
                "#A3C4B9", // 복합성 - Soft Mint
                "#D99B82", // 수부지 - Warm Sand/Peach
                "#C86D51"  // 민감성 - Terracotta
              ],
              borderWidth: 2,
              borderColor: "#FFFFFF"
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          cutout: "62%",
          plugins: {
            legend: {
              position: "right",
              labels: {
                boxWidth: 10,
                boxHeight: 10,
                usePointStyle: true,
                pointStyle: "circle",
                padding: 12,
                font: { size: 11, weight: "600" },
                color: "#334155"
              }
            },
            tooltip: {
              backgroundColor: "#1E293B",
              padding: 10,
              cornerRadius: 10,
              callbacks: {
                label: (ctx) => {
                  const total = responses.length || 1;
                  const val = ctx.parsed;
                  const pct = Math.round((val / total) * 100);
                  return ` ${ctx.label}: ${val}명 (${pct}%)`;
                }
              }
            }
          }
        }
      });
    }

    function renderIrritationBreakdown(filtered) {
      const container = document.getElementById("irritationBreakdownList");
      const total = filtered.length || 1;

      container.innerHTML = IRRITATION_LEVELS.map(level => {
        const count = filtered.filter(r => r.irritation === level.label).length;
        const pct = Math.round((count / total) * 100);
        return `
          <div class="space-y-1">
            <div class="flex items-center justify-between text-xs">
              <span class="font-medium text-slate-700">${level.label}</span>
              <span class="font-semibold text-slate-600">${count}명 (${pct}%)</span>
            </div>
            <div class="w-full h-2 bg-ivory-200 rounded-full overflow-hidden">
              <div class="h-full ${level.colorClass} rounded-full transition-all duration-300" style="width: ${pct}%"></div>
            </div>
          </div>
        `;
      }).join("");
    }

    function formatDateTime(ms) {
      if (!ms) return "-";
      const d = new Date(ms);
      const month = String(d.getMonth() + 1).padStart(2, "0");
      const day = String(d.getDate()).padStart(2, "0");
      const hour = String(d.getHours()).padStart(2, "0");
      const min = String(d.getMinutes()).padStart(2, "0");
      return `${month}.${day} ${hour}:${min}`;
    }

    function escapeHTML(str) {
      if (!str) return "";
      return str
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
    }

    function renderQualitativeFeedback(filtered) {
      const prosItems = filtered.filter(r => r.pros && r.pros.trim().length > 0);
      const consItems = filtered.filter(r => r.cons && r.cons.trim().length > 0);

      document.getElementById("prosCountBadge").textContent = `${prosItems.length}건`;
      document.getElementById("consCountBadge").textContent = `${consItems.length}건`;

      const prosListEl = document.getElementById("prosFeedbackList");
      const consListEl = document.getElementById("consFeedbackList");

      if (prosItems.length === 0) {
        prosListEl.innerHTML = `<p class="text-xs text-slate-400 py-6 text-center">작성된 장점 의견이 없습니다.</p>`;
      } else {
        prosListEl.innerHTML = prosItems.map(item => `
          <div class="p-3.5 rounded-xl bg-ivory-50 border border-ivory-200 space-y-2">
            <div class="flex flex-wrap items-center justify-between gap-1.5">
              <div class="flex items-center gap-1.5">
                <span class="text-xs font-bold text-slate-800">${escapeHTML(item.participantName || "익명")}</span>
                <span class="px-2 py-0.5 rounded-md bg-sage-100 text-sage-700 text-[11px] font-semibold">${escapeHTML(item.skinType)}</span>
              </div>
              <span class="text-[11px] text-slate-400">${formatDateTime(item.createdAtMs)}</span>
            </div>
            <p class="text-xs sm:text-sm text-slate-700 leading-relaxed whitespace-pre-line">${escapeHTML(item.pros)}</p>
          </div>
        `).join("");
      }

      if (consItems.length === 0) {
        consListEl.innerHTML = `<p class="text-xs text-slate-400 py-6 text-center">작성된 보완·개선 의견이 없습니다.</p>`;
      } else {
        consListEl.innerHTML = consItems.map(item => `
          <div class="p-3.5 rounded-xl bg-terracotta-50/40 border border-terracotta-100 space-y-2">
            <div class="flex flex-wrap items-center justify-between gap-1.5">
              <div class="flex items-center gap-1.5">
                <span class="text-xs font-bold text-slate-800">${escapeHTML(item.participantName || "익명")}</span>
                <span class="px-2 py-0.5 rounded-md bg-terracotta-100 text-terracotta-600 text-[11px] font-semibold">${escapeHTML(item.skinType)}</span>
              </div>
              <span class="text-[11px] text-slate-400">${formatDateTime(item.createdAtMs)}</span>
            </div>
            <p class="text-xs sm:text-sm text-slate-700 leading-relaxed whitespace-pre-line">${escapeHTML(item.cons)}</p>
          </div>
        `).join("");
      }
    }

    function renderRawTable(filtered) {
      const tbody = document.getElementById("rawResponsesTableBody");
      document.getElementById("tableRowCountLabel").textContent = `총 ${filtered.length}건 표시 중`;

      tbody.innerHTML = filtered.map(item => {
        const isSafe = item.irritation === "전혀 없음(순함)";
        const irritationBadge = isSafe
          ? `<span class="inline-flex px-2 py-0.5 rounded-full bg-sage-50 text-sage-700 text-[11px] font-semibold">무자극</span>`
          : `<span class="inline-flex px-2 py-0.5 rounded-full bg-terracotta-50 text-terracotta-600 text-[11px] font-semibold">${escapeHTML(item.irritation)}</span>`;

        const usageSummary = Array.isArray(item.usageTimes) ? item.usageTimes.join(", ") : "-";

        return `
          <tr class="hover:bg-ivory-50/80 transition">
            <td class="py-3 px-4 font-semibold text-slate-800 whitespace-nowrap">${escapeHTML(item.participantName || "익명")}</td>
            <td class="py-3 px-3 whitespace-nowrap">
              <span class="px-2 py-0.5 rounded-md bg-ivory-200 text-slate-700 text-xs font-medium">${escapeHTML(item.skinType)}</span>
            </td>
            <td class="py-3 px-3 text-xs text-slate-500 max-w-[140px] truncate" title="${escapeHTML(usageSummary)}">${escapeHTML(usageSummary)}</td>
            <td class="py-3 px-3 text-center font-display text-xs font-semibold text-slate-700 whitespace-nowrap">
              ${item.spreadability} / ${item.absorption} / ${item.finish} / ${item.scent}
            </td>
            <td class="py-3 px-3 text-center font-display text-xs font-semibold text-slate-700 whitespace-nowrap">
              ${item.moistureRetention} / ${item.skinTextureSoothing}
            </td>
            <td class="py-3 px-3 text-center font-display text-xs font-bold text-sage-700 whitespace-nowrap">
              ${item.purchaseIntent}점
            </td>
            <td class="py-3 px-3 whitespace-nowrap">${irritationBadge}</td>
            <td class="py-3 px-3 text-xs text-slate-400 whitespace-nowrap">${formatDateTime(item.createdAtMs)}</td>
            <td class="py-3 px-3 text-center whitespace-nowrap">
              <button
                type="button"
                onclick="openDeleteModal('${item.id}', '${escapeHTML(item.participantName || "익명")}')"
                title="응답 삭제"
                class="p-1.5 rounded-lg text-slate-400 hover:text-terracotta-600 hover:bg-terracotta-50 transition"
              >
                <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                </svg>
              </button>
            </td>
          </tr>
        `;
      }).join("");
    }

    // -----------------------------------------------------------------------
    // 8. Delete Individual Entry Logic (Custom Modal)
    // -----------------------------------------------------------------------
    window.openDeleteModal = function (docId, name) {
      pendingDeleteId = docId;
      document.getElementById("deleteTargetName").textContent = name || "익명";
      document.getElementById("deleteConfirmModal").classList.remove("hidden");
    };

    window.closeDeleteModal = function () {
      pendingDeleteId = null;
      document.getElementById("deleteConfirmModal").classList.add("hidden");
    };

    window.executeDeleteEntry = async function () {
      if (!pendingDeleteId) return;
      const btn = document.getElementById("confirmDeleteBtn");
      btn.disabled = true;
      btn.textContent = "삭제 중...";

      try {
        if (db && currentUser && !pendingDeleteId.startsWith("local-")) {
          const docRef = doc(db, "artifacts", appId, "public", "data", COLLECTION_NAME, pendingDeleteId);
          await deleteDoc(docRef);
        } else {
          allResponses = allResponses.filter(item => item.id !== pendingDeleteId);
          document.getElementById("tabParticipantCount").textContent = `${allResponses.length}명 참여`;
          renderReportDashboard();
        }
        showToast("선택한 응답 데이터가 삭제되었습니다.");
      } catch (err) {
        console.error("Error deleting document:", err);
        showToast("삭제 중 오류가 발생했습니다.");
      } finally {
        btn.disabled = false;
        btn.textContent = "삭제하기";
        closeDeleteModal();
      }
    };

    // -----------------------------------------------------------------------
    // 9. CSV (Excel UTF-8 BOM) Export
    // -----------------------------------------------------------------------
    window.exportToCSV = function () {
      const targetData = getFilteredResponses();
      if (targetData.length === 0) {
        showToast("다운로드할 응답 데이터가 없습니다.");
        return;
      }

      const headers = [
        "번호",
        "작성일시",
        "참여자",
        "피부타입",
        "주사용시간대",
        "발림성(5점)",
        "흡수력(5점)",
        "마무리감(5점)",
        "향만족도(5점)",
        "보습지속력(5점)",
        "피부결·진정개선(5점)",
        "피부자극여부",
        "구매·추천의사(5점)",
        "마음에들었던점(장점)",
        "보완필요한점(개선의견)"
      ];

      const escapeCSV = (val) => {
        const str = val === null || val === undefined ? "" : String(val);
        return `"${str.replace(/"/g, '""').replace(/\r?\n/g, " ")}"`;
      };

      const rows = targetData.map((item, idx) => {
        const dt = item.createdAtMs ? new Date(item.createdAtMs).toLocaleString("ko-KR") : "";
        const times = Array.isArray(item.usageTimes) ? item.usageTimes.join(" / ") : "";
        return [
          idx + 1,
          dt,
          item.participantName || "익명",
          item.skinType || "",
          times,
          item.spreadability ?? "",
          item.absorption ?? "",
          item.finish ?? "",
          item.scent ?? "",
          item.moistureRetention ?? "",
          item.skinTextureSoothing ?? "",
          item.irritation || "",
          item.purchaseIntent ?? "",
          item.pros || "",
          item.cons || ""
        ].map(escapeCSV).join(",");
      });

      // UTF-8 BOM (\uFEFF) ensures Korean text renders properly in Microsoft Excel
      const csvContent = "\uFEFF" + [headers.map(escapeCSV).join(","), ...rows].join("\n");
      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
      const url = URL.createObjectURL(blob);

      const todayStr = new Date().toISOString().slice(0, 10);
      const filterSuffix = activeSkinFilter === "전체" ? "전체" : activeSkinFilter;
      const link = document.createElement("a");
      link.href = url;
      link.setAttribute("download", `신규개발크림_3일품평결과_${filterSuffix}_${todayStr}.csv`);
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
      URL.revokeObjectURL(url);

      showToast(`총 ${targetData.length}건의 품평 데이터가 CSV 파일로 다운로드되었습니다.`);
    };

    // -----------------------------------------------------------------------
    // 10. Boot Application
    // -----------------------------------------------------------------------
    renderRatingControls();
    initFirebaseAndSync();
  </script>
</body>
</html>
