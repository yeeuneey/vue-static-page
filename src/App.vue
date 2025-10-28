<template>
  <div class="app-container" :style="hanjiBackground">
    <header class="app-header">
      <h1 class="title">
        풍요로운 한가위, <span class="highlight">마음</span>을 <span class="highlight">전합니다</span>
      </h1>
      <p class="subtitle">가장 빛나는 보름달 아래, 우리의 전통과 정성을 함께 나눠요.</p>
    </header>

    <main class="content-wrapper">
      <div class="bojagi-box" :class="{ 'unwrapped': isUnwrapped }">
        <div class="gift-inside" :class="{ 'visible': isUnwrapped }" aria-live="polite">
          <h2>보름달처럼 풍성한 복(福)</h2>
          <div class="gift-grid">
            <div v-for="item in gifts" :key="item.name" class="gift-item">
              <span class="gift-emoji">{{ item.emoji }}</span>
              <strong class="gift-name">{{ item.name }}</strong>
              <p class="gift-blessing">{{ item.blessing }}</p>
            </div>
          </div>
        </div>
        <div class="bojagi-cover">
          <div class="flap flap-top"></div>
          <div class="flap flap-right"></div>
          <div class="flap flap-bottom"></div>
          <div class="flap flap-left"></div>
          <div class="bojagi-knot"></div>
        </div>
      </div>
      <button class="action-btn" @click="toggleWrap">
        {{ buttonText }}
      </button>
    </main>

    <section class="tradition-section">
      <h2 class="section-title">함께 나누는 한가위 풍습</h2>
      <div class="tradition-grid">
        <div
            v-for="item in traditions"
            :key="item.title"
            class="tradition-card"
        >
          <div class="tradition-icon">{{ item.emoji }}</div>
          <h3>{{ item.title }}</h3>
          <p>{{ item.description }}</p>
        </div>
      </div>
    </section>

    <section class="wish-section" id="wish">
      <h2 class="section-title">보름달에 전하는 마음 🌕</h2>
      <form class="wish-form" @submit.prevent="addWish">
        <input
            v-model.trim="newWish"
            class="wish-input"
            type="text"
            maxlength="60"
            placeholder="예) 가족 모두 건강하고, 행복하세요!"
            aria-label="덕담 입력"
        />
        <button class="wish-submit" type="submit" :disabled="!newWish">
          남기기
        </button>
      </form>
      <ul class="wish-list" v-if="wishes.length > 0">
        <li v-for="wish in wishes" :key="wish.id" class="wish-item">
          <span class="wish-icon">🌕</span>
          <span class="wish-text">{{ wish.text }}</span>
        </li>
      </ul>
      <p v-else class="wish-empty">
        아직 덕담이 없네요. 첫 번째 덕담을 남겨주세요!
      </p>
    </section>

    <footer class="app-footer">
      <small>© {{ new Date().getFullYear() }} 풍성한 한가위 되세요.
        <span class="heart">💛</span>
      </small>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

// --- TypeScript 인터페이스 정의 ---
interface GiftItem {
  name: string;
  emoji: string;
  blessing: string;
}

interface Wish {
  id: number;
  text: string;
}

// [NEW] 한가위 풍습 인터페이스
interface TraditionItem {
  title: string;
  emoji: string;
  description: string;
}

// --- 반응형 상태 (Reactive State) ---

const isUnwrapped = ref(false);

// 선물 아이템 데이터 (보자기 안)
const gifts = ref<GiftItem[]>([
  {
    name: '황금빛 배',
    emoji: '🍐',
    blessing: '하는 일마다 시원하게 풀리길!',
  },
  {
    name: '달콤한 홍시',
    emoji: '🍊',
    blessing: '사랑과 행복이 무르익길!',
  },
  {
    name: '오색 송편',
    emoji: '🍡',
    blessing: '소원 성취! 만사형통하시길!',
  },
  {
    name: '고소한 한과',
    emoji: '🍪',
    blessing: '웃음꽃 피는 날만 가득하길!',
  },
]);

// 덕담 관련 반응형 상태
const newWish = ref<string>('');
const wishes = ref<Wish[]>([
  { id: 1, text: '보름달처럼 넉넉한 한가위 되세요!' },
  { id: 2, text: '모두의 앞날에 밝은 빛만 가득하길!' },
]);

// [NEW] 한가위 풍습 소개 데이터 (과제 항목)
const traditions = ref<TraditionItem[]>([
  {
    title: '오색 송편 빚기',
    emoji: '🍡',
    description: '한 해의 수확에 감사하며 햅쌀로 빚는 떡입니다. "송편을 예쁘게 빚으면 예쁜 딸을 낳는다"는 이야기가 전해져 내려옵니다.'
  },
  {
    title: '강강술래',
    emoji: '💃',
    description: '풍요로운 수확과 건강을 기원하며 밝은 보름달 아래에서 둥글게 모여 춤추고 노래하는 전통 놀이입니다. 모두가 하나되는 시간이죠.'
  },
  {
    title: '윷놀이',
    emoji: '🎲',
    description: '온 가족이 모여 편을 나누어 즐기는 전통 놀이입니다. 윷가락을 던지며 다 함께 웃고 떠드는 정겨운 풍경을 만듭니다.'
  }
]);


// --- 계산된 속성 (Computed Properties) ---

const buttonText = computed(() => {
  return isUnwrapped.value ? '다시 정성껏 묶기' : '복(福) 열어보기';
});

const hanjiBackground = computed(() => ({
  backgroundColor: '#fdfbf5',
  backgroundImage: `
    radial-gradient(circle at 1px 1px, #00000012 1px, transparent 0),
    radial-gradient(circle at 10px 10px, #00000010 1px, transparent 0)
  `,
  backgroundSize: '20px 20px',
}));

// --- 메소드 (Methods) ---

const toggleWrap = (): void => {
  isUnwrapped.value = !isUnwrapped.value;
};

const addWish = (): void => {
  if (!newWish.value) return;
  wishes.value.unshift({
    id: Date.now(),
    text: newWish.value,
  });
  newWish.value = '';
};
</script>

<style scoped>
/* 구글 폰트 */
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700;900&display=swap');

/* --- 기본 앱 스타일 --- */
.app-container {
  min-height: 100svh;
  font-family: 'Noto Sans KR', sans-serif;
  color: #333;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  padding: 2rem 1rem;
  overflow-x: hidden; /* 가로 스크롤 방지 */
}

.app-header {
  text-align: center;
  margin-bottom: 2rem;
  width: 100%;
}

.title {
  font-size: clamp(2rem, 5vw, 2.8rem);
  font-weight: 900;
  color: #4a3f35;
  margin: 0;
  line-height: 1.3;
}

.title .highlight {
  color: #b85a3a;
}

.subtitle {
  font-size: clamp(1rem, 2.5vw, 1.15rem);
  color: #6d5f56;
  margin-top: 0.5rem;
  max-width: 500px; /* 부제목 줄바꿈 관리 */
  margin-left: auto;
  margin-right: auto;
}

/* --- 메인 콘텐츠: 보자기 --- */
.content-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.bojagi-box {
  position: relative;
  width: clamp(300px, 80vw, 450px);
  height: clamp(300px, 80vw, 450px);
  margin-bottom: 2rem;
}

.bojagi-cover {
  position: absolute;
  inset: 0;
  z-index: 10;
  transition: transform 0.8s cubic-bezier(0.86, 0, 0.07, 1);
}

.flap {
  position: absolute;
  width: 0;
  height: 0;
  background-image: linear-gradient(135deg, #d87a5a 0%, #b85a3a 100%);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  border-style: solid;
  opacity: 1;
  transform: none;
  transition: transform 0.8s cubic-bezier(0.86, 0, 0.07, 1) 0.3s,
  opacity 0.6s ease 0.3s,
  visibility 0s 0.3s;
}

/* 4개의 삼각형(flap) 배치 */
.flap-top {
  top: 0; left: 0;
  border-width: 150px 150px 0 150px;
  border-color: #c86a4a transparent transparent transparent;
}
.flap-right {
  top: 0; right: 0;
  border-width: 150px 150px 150px 0;
  border-color: transparent #b85a3a transparent transparent;
}
.flap-bottom {
  bottom: 0; right: 0;
  border-width: 0 150px 150px 150px;
  border-color: transparent transparent #d87a5a transparent;
}
.flap-left {
  top: 0; left: 0;
  border-width: 150px 0 150px 150px;
  border-color: transparent transparent transparent #c86a4a;
}
@media (min-width: 450px) {
  .flap-top { border-width: 225px 225px 0 225px; }
  .flap-right { border-width: 225px 225px 225px 0; }
  .flap-bottom { border-width: 0 225px 225px 225px; }
  .flap-left { border-width: 225px 0 225px 225px; }
}

/* 매듭 스타일 */
.bojagi-knot {
  position: absolute;
  top: 50%; left: 50%;
  width: 50px;
  height: 50px;
  background: #f0a080;
  border-radius: 50%;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  opacity: 1;
  transform: translate(-50%, -50%) scale(1);
  visibility: visible;
  transition: transform 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55) 0.3s,
  opacity 0.3s ease 0.3s,
  visibility 0s 0.3s;
}
.bojagi-knot::before,
.bojagi-knot::after {
  content: '';
  position: absolute;
  width: 60px; height: 12px;
  background: #e88a6a;
  border-radius: 6px;
  top: 50%; left: 50%;
  transform-origin: center;
  margin-top: -6px; margin-left: -30px;
}
.bojagi-knot::before { transform: rotate(30deg); }
.bojagi-knot::after { transform: rotate(-30deg); }


/* 언래핑(Unwrapping) 애니메이션 (열릴 때) */
.bojagi-box.unwrapped .bojagi-knot {
  transform: translate(-50%, -50%) scale(0);
  opacity: 0;
  visibility: hidden;
  transition-delay: 0s;
}
.bojagi-box.unwrapped .flap {
  opacity: 0;
  visibility: hidden;
  transition-delay: 0s;
}
.bojagi-box.unwrapped .flap-top { transform: translateY(-100%) rotate(-30deg); }
.bojagi-box.unwrapped .flap-right { transform: translateX(100%) rotate(30deg); }
.bojagi-box.unwrapped .flap-bottom { transform: translateY(100%) rotate(30deg); }
.bojagi-box.unwrapped .flap-left { transform: translateX(-100%) rotate(-30deg); }


/* --- 보자기 내부 (선물) --- */
.gift-inside {
  position: absolute;
  inset: 0;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.08);
  border: 1px solid #eee;
  padding: 1.5rem;
  text-align: center;
  z-index: 5;
  opacity: 0;
  transform: translateY(20px) scale(0.95);
  visibility: hidden;
  transition: opacity 0.3s ease 0s,
  transform 0.3s ease 0s,
  visibility 0s 0.3s;
}

.gift-inside.visible {
  opacity: 1;
  transform: none;
  visibility: visible;
  transition: opacity 0.8s ease 0.5s,
  transform 0.6s ease 0.5s,
  visibility 0s 0.5s;
}

.gift-inside h2 {
  font-size: 1.3rem;
  font-weight: 700;
  color: #b85a3a;
  margin-top: 0;
}

.gift-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-top: 1.5rem;
}

.gift-item {
  background: #fdfbf5;
  border-radius: 8px;
  padding: 1rem 0.5rem;
}
.gift-emoji { font-size: 2.5rem; }
.gift-name { display: block; font-size: 1.1rem; font-weight: 700; margin: 0.25rem 0; }
.gift-blessing { font-size: 0.9rem; color: #666; margin: 0; }


/* --- 액션 버튼 --- */
.action-btn {
  font-family: 'Noto Sans KR', sans-serif;
  font-size: 1.1rem;
  font-weight: 700;
  background-color: #b85a3a;
  color: #fff;
  border: none;
  border-radius: 50px;
  padding: 0.9rem 2rem;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(184, 90, 58, 0.3);
  transition: all 0.2s ease-in-out;
}
.action-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(184, 90, 58, 0.4);
}
.action-btn:active {
  transform: translateY(0) scale(0.98);
}

/* [NEW] 공용 섹션 스타일
  - .tradition-section과 .wish-section에 모두 적용
*/
.tradition-section,
.wish-section {
  width: 100%;
  max-width: 600px; /* 최대 너비 설정 */
  margin-top: 3rem; /* 섹션 간 간격 */
  padding: 1.5rem;
  background: #fff;
  border-radius: 12px;
  border: 1px solid #eee;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.05);
}

/* [NEW] 공용 섹션 제목 스타일 */
.section-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: #b85a3a; /* 테마 하이라이트 컬러 */
  text-align: center;
  margin: 0 0 1.5rem 0;
}

/* [NEW] 풍습 소개 그리드 */
.tradition-grid {
  display: grid;
  gap: 1.25rem;
  grid-template-columns: 1fr; /* 모바일 기본 1단 */
}

/* [NEW] 풍습 소개 카드 */
.tradition-card {
  background: #fdfbf5; /* 한지 톤 */
  border: 1px solid #e0d9d3;
  border-radius: 10px;
  padding: 1.5rem;
  text-align: center;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.tradition-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.05);
}

.tradition-icon {
  font-size: 2.5rem;
  line-height: 1;
  margin-bottom: 0.75rem;
}

.tradition-card h3 {
  font-size: 1.2rem;
  font-weight: 700;
  color: #4a3f35;
  margin: 0 0 0.5rem 0;
}

.tradition-card p {
  font-size: 0.95rem;
  color: #6d5f56;
  line-height: 1.6;
  margin: 0;
}

/* --- 덕담(Wish) 섹션 --- */
/* .wish-section 스타일은 위 공용 스타일로 통합됨 */

.wish-form {
  display: flex;
  gap: 0.75rem;
}

.wish-input {
  flex-grow: 1;
  border: 2px solid #e0d9d3;
  background: #fdfbf5;
  padding: 0.75rem 1rem;
  border-radius: 8px;
  font-family: 'Noto Sans KR', sans-serif;
  font-size: 1rem;
  color: #4a3f35;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.wish-input:focus {
  outline: none;
  border-color: #b85a3a;
  box-shadow: 0 0 0 3px rgba(184, 90, 58, 0.15);
}

.wish-submit {
  font-family: 'Noto Sans KR', sans-serif;
  font-size: 1rem;
  font-weight: 700;
  background-color: #b85a3a;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 0.75rem 1.2rem;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}

.wish-submit:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(184, 90, 58, 0.3);
}

.wish-submit:disabled {
  background-color: #c9b7ab;
  opacity: 0.7;
  cursor: not-allowed;
}

/* 덕담 목록 */
.wish-list {
  list-style: none;
  padding: 0;
  margin: 1.5rem 0 0 0;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  max-height: 300px;
  overflow-y: auto;
}

.wish-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  background: #fdfbf5;
  padding: 0.8rem 1rem;
  border-radius: 8px;
  border: 1px solid #e0d9d3;
}

.wish-icon {
  font-size: 1.2rem;
}

.wish-text {
  font-size: 0.95rem;
  color: #4a3f35;
  line-height: 1.5;
  word-break: break-all;
}

.wish-empty {
  text-align: center;
  color: #8a7a70;
  margin-top: 1.5rem;
  font-size: 0.95rem;
}


/* --- 푸터 --- */
.app-footer {
  width: 100%;
  text-align: center;
  margin-top: 3rem;
  padding-bottom: 1rem;
  color: #aaa;
}
.heart {
  display: inline-block;
  transform: scale(1);
  animation: pulse-heart 1.5s infinite ease-in-out;
}
@keyframes pulse-heart {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

/* --- [NEW] 반응형: 데스크탑 --- */
@media (min-width: 640px) {
  .tradition-grid {
    /* 풍습 카드가 3개이므로 3단으로 변경 */
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;
  }
  .tradition-card {
    padding: 1.25rem 1rem;
  }
  .tradition-card h3 {
    font-size: 1.1rem; /* 3단일 땐 폰트 살짝 줄임 */
  }
  .tradition-card p {
    font-size: 0.9rem;
  }
}
</style>