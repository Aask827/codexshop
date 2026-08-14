<script setup>
import { computed, onBeforeUnmount, onMounted, reactive, ref } from "vue";

const navigation = [
  { label: "训练方式", href: "#features" },
  { label: "课程安排", href: "#classes" },
  { label: "会员方案", href: "#plans" },
  { label: "教练团队", href: "#trainers" },
];

const schedule = [
  {
    time: "06:30",
    title: "晨间燃脂",
    copy: "适合想快速唤醒身体的通勤人群。",
  },
  {
    time: "12:30",
    title: "核心力量",
    copy: "30 分钟高效率训练，午休也能完成。",
  },
  {
    time: "18:30",
    title: "HIIT 燃脂",
    copy: "节奏紧凑，适合减脂和心肺提升。",
  },
  {
    time: "20:00",
    title: "深度拉伸",
    copy: "训练后的恢复与柔韧性放松课。",
  },
];

const features = [
  {
    title: "力量训练",
    copy: "深蹲、硬拉、卧推和器械区一体化布局，适合新手到进阶会员。",
    icon: "barbell",
  },
  {
    title: "HIIT 燃脂",
    copy: "高效率课程，节奏明确，适合希望缩短训练时长的人群。",
    icon: "grid",
  },
  {
    title: "搏击训练",
    copy: "沙袋、步伐和节奏组合，偏好释放压力的会员会很喜欢。",
    icon: "boxing",
  },
  {
    title: "恢复拉伸",
    copy: "训练结束后的放松区域，帮助身体更稳定地恢复和进步。",
    icon: "recovery",
  },
];

const programs = [
  {
    tag: "早晨 06:30",
    title: "晨间激活",
    copy: "低门槛启动训练，适合清醒身体与准备一天工作的人。",
    meta: "20 - 35 分钟 · 中低强度 · 适合新手",
  },
  {
    tag: "午间 12:30",
    title: "午休快练",
    copy: "短时高效课程，训练、淋浴、回工位都能顺畅衔接。",
    meta: "30 - 40 分钟 · 核心+力量 · 小班制",
  },
  {
    tag: "晚间 18:30",
    title: "高能燃脂",
    copy: "为下班后来店的人设计，节奏快、汗点足、体验感强。",
    meta: "45 分钟 · HIIT · 晚高峰热门",
  },
];

const plans = [
  {
    title: "学生卡",
    price: "¥99",
    period: "/ 月",
    benefits: ["凭学生证办理", "全时段进场", "每周 2 次基础团课"],
  },
  {
    title: "月卡",
    price: "¥269",
    period: "/ 月",
    benefits: ["全时段进场", "团课 4 次", "1 次体测"],
  },
  {
    title: "季卡",
    price: "¥699",
    period: "/ 3 个月",
    benefits: ["全时段进场", "团课不限次", "2 次私教评估"],
    featured: true,
  },
  {
    title: "年卡",
    price: "¥1999",
    period: "/ 年",
    benefits: ["全年通行", "专属训练档案", "优先预约热门课程"],
  },
];

const trainers = [
  {
    name: "程野",
    specialty: "力量训练 / 增肌",
    copy: "负责大重量技术、动作纠正和新手训练周期规划。",
    icon: "barbell",
  },
  {
    name: "林瞳",
    specialty: "HIIT / 减脂",
    copy: "擅长高密度训练设计，节奏清晰，课堂氛围强。",
    icon: "grid",
  },
  {
    name: "许薇",
    specialty: "拉伸 / 恢复",
    copy: "帮助会员把恢复、灵活性和长期训练可持续性做好。",
    icon: "recovery",
  },
];

const form = reactive({
  name: "",
  phone: "",
  goal: "增肌塑形",
  note: "",
});

const availableSlots = ref(18);
const formStatus = ref("");
const formError = ref("");
const isSubmitting = ref(false);
const isSubmitted = ref(false);
let slotsTimer;
let submitTimer;

const formStatusClass = computed(() => ({
  "form-status--success": isSubmitted.value,
  "form-status--error": Boolean(formError.value),
}));

const updatePeakSlots = () => {
  const direction = Math.random() < 0.5 ? -1 : 1;
  const step = Math.random() > 0.72 ? 2 : 1;
  availableSlots.value = Math.max(
    6,
    Math.min(24, availableSlots.value + direction * step),
  );
};

const resetForm = () => {
  form.name = "";
  form.phone = "";
  form.goal = "增肌塑形";
  form.note = "";
};

const validateForm = () => {
  formError.value = "";
  formStatus.value = "";

  if (!form.name.trim()) {
    formError.value = "请填写姓名。";
    return false;
  }

  if (!/^1[3-9]\d{9}$/.test(form.phone.trim())) {
    formError.value = "请输入有效的 11 位手机号码。";
    return false;
  }

  return true;
};

const submitForm = () => {
  if (isSubmitting.value || !validateForm()) {
    return;
  }

  isSubmitting.value = true;
  isSubmitted.value = false;
  formStatus.value = "正在记录预约信息...";

  submitTimer = window.setTimeout(() => {
    isSubmitting.value = false;
    isSubmitted.value = true;
    formStatus.value = "预约信息已记录，稍后会有工作人员联系你。";
    resetForm();
  }, 700);
};

onMounted(() => {
  slotsTimer = window.setInterval(updatePeakSlots, 3600);
});

onBeforeUnmount(() => {
  window.clearInterval(slotsTimer);
  window.clearTimeout(submitTimer);
});
</script>

<template>
  <div class="page">
    <header class="hero">
      <div class="hero__media" aria-hidden="true">
        <img
          src="https://images.unsplash.com/photo-1517836357463-d25dfeac3438?auto=format&fit=crop&w=1600&q=80"
          alt=""
          fetchpriority="high"
        />
      </div>

      <div class="wrap">
        <div class="topbar">
          <a class="brand" href="#top" aria-label="返回首页">
            <span class="brand__mark" aria-hidden="true">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M3 9v6" />
                <path d="M21 9v6" />
                <path d="M6 7v10" />
                <path d="M18 7v10" />
                <path d="M6 12h12" />
              </svg>
            </span>
            <span>铁境健身</span>
          </a>

          <nav class="nav" aria-label="主导航">
            <a v-for="item in navigation" :key="item.href" :href="item.href">
              {{ item.label }}
            </a>
          </nav>
        </div>

        <div id="top" class="hero__content">
          <div>
            <span class="eyebrow">高强度力量区 · 团课 · 恢复区 · 24 小时自助开放</span>
            <h1>把训练，变成每天都愿意回来的一小时</h1>
            <p class="lead">
              这是一家面向上班族和训练爱好者的健身房。清晰展示课程、方案和教练信息，让你一眼找到适合自己的训练节奏。
            </p>

            <div class="hero__actions">
              <a class="btn btn--primary" href="#cta">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
                  <path d="M8 7V3" />
                  <path d="M16 7V3" />
                  <rect x="3" y="5" width="18" height="16" rx="3" />
                  <path d="M3 11h18" />
                </svg>
                预约体验课
              </a>
              <a class="btn btn--ghost" href="#classes">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
                  <path d="M5 12h14" />
                  <path d="m12 5 7 7-7 7" />
                </svg>
                查看课程表
              </a>
            </div>

            <div class="hero__stats" aria-label="核心数据">
              <div class="metric">
                <strong>1200+</strong>
                <span>活跃会员</span>
              </div>
              <div class="metric">
                <strong>14</strong>
                <span>每日课程</span>
              </div>
              <div class="metric">
                <strong>24h</strong>
                <span>自助开放</span>
              </div>
              <div class="metric">
                <strong>3</strong>
                <span>核心训练区</span>
              </div>
            </div>
          </div>

          <aside class="schedule" aria-label="今日课程">
            <div class="schedule__head">
              <h2>今日安排</h2>
              <span class="chip">晚高峰余位 <strong>{{ availableSlots }}</strong></span>
            </div>
            <div class="schedule__body">
              <article v-for="item in schedule" :key="item.time" class="slot">
                <div class="slot__time">{{ item.time }}</div>
                <div>
                  <h3 class="slot__title">{{ item.title }}</h3>
                  <p class="slot__copy">{{ item.copy }}</p>
                </div>
              </article>
            </div>
          </aside>
        </div>
      </div>
    </header>

    <main>
      <section id="features" class="section">
        <div class="wrap">
          <div class="section__head">
            <div>
              <span class="section__kicker">TRAIN SMART</span>
              <h2>训练方式</h2>
            </div>
            <p>把用户最关心的信息按层级摆好，方便快速判断这里是否适合你的训练目标。</p>
          </div>

          <div class="grid grid--4">
            <article v-for="item in features" :key="item.title" class="card">
              <div class="feature__icon" aria-hidden="true">
                <svg v-if="item.icon === 'barbell'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M3 12h18" />
                  <path d="M7 7v10" />
                  <path d="M17 7v10" />
                  <path d="M5 9v6" />
                  <path d="M19 9v6" />
                </svg>
                <svg v-else-if="item.icon === 'grid'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <rect x="4" y="4" width="6" height="6" rx="1" />
                  <rect x="14" y="4" width="6" height="6" rx="1" />
                  <rect x="4" y="14" width="6" height="6" rx="1" />
                  <rect x="14" y="14" width="6" height="6" rx="1" />
                </svg>
                <svg v-else-if="item.icon === 'boxing'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M6 4h12v5H6z" />
                  <path d="M8 9v11" />
                  <path d="M16 9v11" />
                  <path d="M5 20h14" />
                  <path d="M10 12h4" />
                </svg>
                <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M5 12h14" />
                  <path d="M12 5v14" />
                  <path d="M8 8l8 8" />
                  <path d="M16 8 8 16" />
                </svg>
              </div>
              <h3>{{ item.title }}</h3>
              <p>{{ item.copy }}</p>
            </article>
          </div>
        </div>
      </section>

      <section id="classes" class="section section--alt">
        <div class="wrap">
          <div class="section__head">
            <div>
              <span class="section__kicker">FIND YOUR RHYTHM</span>
              <h2>课程安排</h2>
            </div>
            <p>把最常见的训练时段摆到前面，让访客快速知道什么时候来最合适。</p>
          </div>

          <div class="programs">
            <article v-for="item in programs" :key="item.tag" class="card program">
              <div>
                <span class="program__tag">{{ item.tag }}</span>
                <h3>{{ item.title }}</h3>
                <p>{{ item.copy }}</p>
              </div>
              <div class="program__meta">{{ item.meta }}</div>
            </article>
          </div>
        </div>
      </section>

      <section id="plans" class="section">
        <div class="wrap">
          <div class="section__head">
            <div>
              <span class="section__kicker">MEMBERSHIP</span>
              <h2>会员方案</h2>
            </div>
            <p>价格层级清楚、权益容易对比，帮助访客快速选择长期训练方案。</p>
          </div>

          <div class="grid grid--4">
            <article
              v-for="item in plans"
              :key="item.title"
              class="card plan-card"
              :class="{ 'card--featured': item.featured }"
            >
              <span v-if="item.featured" class="chip chip--accent">最受欢迎</span>
              <h3>{{ item.title }}</h3>
              <div class="price"><strong>{{ item.price }}</strong><span>{{ item.period }}</span></div>
              <ul class="list">
                <li v-for="benefit in item.benefits" :key="benefit">{{ benefit }}</li>
              </ul>
            </article>
          </div>
        </div>
      </section>

      <section id="trainers" class="section section--alt">
        <div class="wrap">
          <div class="section__head">
            <div>
              <span class="section__kicker">MEET THE TEAM</span>
              <h2>教练团队</h2>
            </div>
            <p>用教练信息收尾，提升信任感和报名意愿，让访客知道自己会和谁一起训练。</p>
          </div>

          <div class="grid grid--3">
            <article v-for="item in trainers" :key="item.name" class="card">
              <div class="coach__row">
                <div class="coach__avatar" aria-hidden="true">
                  <svg v-if="item.icon === 'barbell'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M3 12h18" />
                    <path d="M7 7v10" />
                    <path d="M17 7v10" />
                  </svg>
                  <svg v-else-if="item.icon === 'grid'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect x="4" y="4" width="6" height="6" rx="1" />
                    <rect x="14" y="14" width="6" height="6" rx="1" />
                    <path d="m14 4 6 6" />
                    <path d="m20 4-6 6" />
                  </svg>
                  <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M5 12h14" />
                    <path d="M12 5v14" />
                    <path d="M8 8l8 8" />
                    <path d="M16 8 8 16" />
                  </svg>
                </div>
                <div class="coach__meta">
                  <strong>{{ item.name }}</strong>
                  <span>{{ item.specialty }}</span>
                </div>
              </div>
              <p>{{ item.copy }}</p>
            </article>
          </div>
        </div>
      </section>

      <section id="cta" class="cta">
        <div class="wrap">
          <div class="cta__panel">
            <div class="cta__copy">
              <span class="section__kicker">START TODAY</span>
              <h2>安排一次试练，先感受场地和节奏。</h2>
              <p>
                留下联系方式后，前台会按你的目标匹配课程和教练。现在先用本地模拟表单记录体验意向，后续可接入真实预约系统。
              </p>
            </div>

            <form class="form" novalidate @submit.prevent="submitForm">
              <div class="form__grid">
                <div class="field">
                  <label for="name">姓名 <span aria-hidden="true">*</span></label>
                  <input
                    id="name"
                    v-model="form.name"
                    name="name"
                    type="text"
                    autocomplete="name"
                    placeholder="怎么称呼你？"
                    :aria-invalid="Boolean(formError && !form.name.trim())"
                  />
                </div>
                <div class="field">
                  <label for="phone">电话 <span aria-hidden="true">*</span></label>
                  <input
                    id="phone"
                    v-model="form.phone"
                    name="phone"
                    type="tel"
                    inputmode="tel"
                    autocomplete="tel"
                    placeholder="方便联系的号码"
                    :aria-invalid="Boolean(formError && form.name.trim())"
                  />
                </div>
                <div class="field">
                  <label for="goal">目标</label>
                  <div class="select-shell">
                    <select id="goal" v-model="form.goal" name="goal">
                      <option>增肌塑形</option>
                      <option>减脂燃脂</option>
                      <option>力量提升</option>
                      <option>恢复拉伸</option>
                    </select>
                  </div>
                </div>
                <div class="field">
                  <label for="note">备注</label>
                  <textarea
                    id="note"
                    v-model="form.note"
                    name="note"
                    rows="3"
                    placeholder="比如你更想练哪个时段"
                  ></textarea>
                </div>
                <button class="submit" type="submit" :disabled="isSubmitting || isSubmitted">
                  {{ isSubmitting ? "正在提交..." : isSubmitted ? "已收到，稍后联系你" : "提交预约" }}
                </button>
                <p
                  v-if="formStatus || formError"
                  class="form-status"
                  :class="formStatusClass"
                  role="status"
                  aria-live="polite"
                >
                  {{ formError || formStatus }}
                </p>
              </div>
            </form>
          </div>
        </div>
      </section>
    </main>

    <footer class="foot">
      <div class="wrap">
        <div class="foot__row">
          <span>铁境健身 · 健身房首页</span>
          <span>地址 / 营业时间 / 联系方式可按真实信息替换</span>
        </div>
      </div>
    </footer>
  </div>
</template>
