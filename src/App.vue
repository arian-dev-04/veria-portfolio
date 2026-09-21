<template>
  <div
    class="site-shell"
    :class="[
      languageClass,
      { 'menu-open': isMenuOpen, 'site-ready': !isLoading },
    ]"
    :lang="language"
    :dir="language === 'fa' ? 'rtl' : 'ltr'"
  >
    <!-- Artistic loading screen -->
    <Transition name="art-loader">
      <div
        v-if="isLoading"
        class="art-loader"
        role="status"
        :aria-label="copy.loadingLabel"
      >
        <div class="art-loader__grain" aria-hidden="true"></div>

        <div class="art-loader__scene" aria-hidden="true">
          <span class="art-loader__label">STUDIO / 01</span>

          <div class="art-loader__canvas">
            <div class="art-loader__canvas-paper">
              <svg viewBox="0 0 640 430" fill="none">
                <defs>
                  <linearGradient
                    id="loaderWash"
                    x1="90"
                    y1="35"
                    x2="540"
                    y2="390"
                    gradientUnits="userSpaceOnUse"
                  >
                    <stop stop-color="#E8DCC6" stop-opacity=".18" />
                    <stop
                      offset=".52"
                      stop-color="#B96F4B"
                      stop-opacity=".42"
                    />
                    <stop offset="1" stop-color="#5E7587" stop-opacity=".18" />
                  </linearGradient>
                  <filter id="loaderBlur">
                    <feGaussianBlur stdDeviation="18" />
                  </filter>
                </defs>

                <rect width="640" height="430" fill="#E8E0D1" />
                <path
                  class="loader-paint-wash"
                  d="M-10 325C92 285 145 210 245 234C327 254 350 340 449 320C531 304 577 244 650 208V455H-10V325Z"
                  fill="url(#loaderWash)"
                />
                <path
                  class="loader-paint-circle"
                  d="M176 132C235 83 325 72 385 119C438 162 423 251 366 289C299 333 201 308 158 248C121 196 128 169 176 132Z"
                  fill="#B95F45"
                  fill-opacity=".78"
                  filter="url(#loaderBlur)"
                />
                <path
                  class="loader-paint-line loader-paint-line--one"
                  d="M62 332C136 289 185 311 247 278C313 242 352 145 425 157C485 167 525 224 584 183"
                />
                <path
                  class="loader-paint-line loader-paint-line--two"
                  d="M70 95C125 130 159 105 211 121C265 138 286 184 343 181C400 177 427 117 505 121C548 124 571 145 594 161"
                />
                <path
                  class="loader-paint-detail"
                  d="M106 349C198 328 237 344 298 323C358 303 404 258 476 264C521 268 553 286 590 312"
                />
              </svg>
            </div>
            <span class="art-loader__tape art-loader__tape--one"></span>
            <span class="art-loader__tape art-loader__tape--two"></span>
          </div>

          <div class="art-loader__easel">
            <span></span>
            <span></span>
            <i></i>
          </div>

          <div class="art-loader__palette">
            <b></b>
            <i></i>
            <i></i>
            <i></i>
            <i></i>
            <i></i>
          </div>

          <div class="art-loader__brush">
            <span></span>
            <i></i>
          </div>
        </div>

        <div class="art-loader__content">
          <span class="art-loader__eyebrow">Arian Kalantari</span>
          <div class="art-loader__rule" aria-hidden="true"><i></i></div>
        </div>

        <div class="art-loader__footer">
          <span>Visual practice</span>
          <span>© {{ year }}</span>
        </div>
      </div>
    </Transition>

    <!-- Desktop rail -->
    <aside class="sidebar" :aria-label="copy.primaryNavigation">
      <div class="sidebar__top">
        <a class="brand" href="#home" :aria-label="copy.homeLabel">
          <span class="brand__mark">AR</span>
        </a>

        <div class="brand__copy">
          <strong>Arian Kalantari</strong>
          <span>{{ copy.role }}</span>
        </div>
      </div>

      <nav class="sidebar__nav">
        <a
          v-for="item in navItems"
          :key="item.id"
          :href="`#${item.id}`"
          :class="{ active: activeSection === item.id }"
          @click="closeMenu"
        >
          <span class="nav-icon" v-html="item.icon" aria-hidden="true"></span>
          <span>{{ item.label }}</span>
          <span v-if="item.id === 'works'" class="nav-arrow" aria-hidden="true"
            >›</span
          >
        </a>
      </nav>

      <button
        class="language-switch"
        type="button"
        :aria-label="
          language === 'en' ? 'Switch to Persian' : 'تغییر زبان به انگلیسی'
        "
        :title="language === 'en' ? 'Switch to Persian' : 'Switch to English'"
        @click="toggleLanguage"
      >
        <span :class="{ active: language === 'en' }">EN</span>
        <span :class="{ active: language === 'fa' }">FA</span>
        <i :class="{ 'is-fa': language === 'fa' }" aria-hidden="true"></i>
      </button>

      <div class="sidebar__quote">
        <p v-html="copy.quote"></p>
        <span></span>
      </div>

      <div class="sidebar__mountain" aria-hidden="true"></div>

      <div class="sidebar__bottom">
        <div class="socials" :aria-label="copy.socialLinks">
          <a
            v-for="social in socials"
            :key="social.label"
            :href="social.href"
            target="_blank"
            rel="noreferrer"
            :aria-label="social.label"
            v-html="social.icon"
          ></a>
        </div>
        <p>© {{ year }} {{ copy.name }}</p>
        <span>{{ copy.rights }}</span>
      </div>
    </aside>

    <!-- Mobile header -->
    <header class="mobile-header">
      <a class="mobile-brand" href="#home" :aria-label="copy.homeLabel">
        <span class="brand__mark">AR</span>
        <span class="mobile-brand__text">
          <strong>{{ copy.name }}</strong>
          <small>{{ copy.mobileRole }}</small>
        </span>
      </a>

      <div class="mobile-actions">
        <button
          class="language-switch language-switch--mobile"
          type="button"
          :aria-label="
            language === 'en' ? 'Switch to Persian' : 'تغییر زبان به انگلیسی'
          "
          :title="language === 'en' ? 'Switch to Persian' : 'Switch to English'"
          @click="toggleLanguage"
        >
          <span :class="{ active: language === 'en' }">EN</span>
          <span :class="{ active: language === 'fa' }">FA</span>
          <i :class="{ 'is-fa': language === 'fa' }" aria-hidden="true"></i>
        </button>
        <button
          class="icon-button menu-button"
          type="button"
          :aria-expanded="isMenuOpen"
          :aria-label="copy.menu"
          @click="toggleMenu"
        >
          <span class="menu-line"></span>
          <span class="menu-line"></span>
        </button>
      </div>
    </header>

    <!-- Mobile menu -->
    <Transition name="fade">
      <div v-if="isMenuOpen" class="mobile-menu" @click.self="closeMenu">
        <nav>
          <a
            v-for="item in navItems"
            :key="item.id"
            :href="`#${item.id}`"
            @click="closeMenu"
          >
            <span>{{ item.label }}</span>
            <span>{{ item.id === "works" ? "↗" : "→" }}</span>
          </a>
        </nav>
      </div>
    </Transition>

    <main class="main-content">
      <!-- Hero -->
      <section id="home" ref="homeSection" class="hero section-anchor">
        <div class="hero__media">
          <img
            :src="localImage(1)"
            alt="Minimal stone sculpture illuminated by warm afternoon light"
            width="2200"
            height="1300"
            fetchpriority="high"
          />
        </div>
        <div class="hero__overlay"></div>

        <div class="hero__content reveal-item">
          <div class="eyebrow">
            <span>{{ copy.heroEyebrow }}</span>
            <i></i>
          </div>
          <h1>{{ copy.name }}</h1>
          <p>{{ copy.heroText }}</p>
          <a class="hero__cta" href="#works">
            <span>{{ copy.viewWork }}</span>
            <span class="circle-arrow">↗</span>
          </a>
        </div>

        <div class="hero__scroll" aria-hidden="true">
          <span></span>
          <b>Scroll</b>
        </div>
      </section>

      <!-- Works -->
      <section id="works" ref="worksSection" class="works section-anchor">
        <div class="works__toolbar reveal-item">
          <div class="filters" role="tablist" :aria-label="copy.projectFilters">
            <button
              v-for="filter in filters"
              :key="filter"
              type="button"
              :class="{ selected: activeFilter === filter }"
              @click="activeFilter = filter"
            >
              {{ filter }}
            </button>
          </div>

          <div class="works__meta">
            <div class="view-toggle" :aria-label="copy.viewOptions">
              <button
                class="is-active"
                type="button"
                :aria-label="copy.gridView"
                v-html="icons.grid"
              ></button>
              <button
                type="button"
                :aria-label="copy.listView"
                v-html="icons.list"
              ></button>
            </div>
            <span>{{ filteredWorks.length }} / {{ projects.length }}</span>
            <button class="pager" type="button" :aria-label="copy.nextProjects">
              ›
            </button>
          </div>
        </div>

        <div class="project-grid">
          <article
            v-for="project in filteredWorks"
            :key="project.id"
            class="project-card reveal-item"
            :class="project.size"
            tabindex="0"
            @click="openProject(project)"
            @keydown.enter="openProject(project)"
            @keydown.space.prevent="openProject(project)"
          >
            <img
              :src="project.image"
              :alt="project.alt"
              loading="lazy"
              width="1200"
              height="860"
            />
            <div class="project-card__shade"></div>
            <div
              v-if="project.video"
              class="play-badge"
              :aria-label="copy.videoProject"
            >
              ▶
            </div>
            <div class="project-card__body">
              <div>
                <span class="card-kicker">{{ project.category }}</span>
                <h2>{{ project.title }}</h2>
                <p>{{ project.description }}</p>
              </div>
              <span class="card-arrow" aria-hidden="true">↗</span>
            </div>
          </article>
        </div>
      </section>

      <!-- About -->
      <section id="about" ref="aboutSection" class="about section-anchor">
        <div class="about__image-wrap reveal-item">
          <img
            :src="localImage(2)"
            alt="Artist studio with tools and sculptural objects"
            loading="lazy"
            width="1500"
            height="1800"
          />
        </div>
        <div class="about__copy reveal-item">
          <span class="section-label">{{ copy.aboutLabel }}</span>
          <h2 v-html="copy.aboutTitle"></h2>
          <p>{{ copy.aboutText1 }}</p>
          <p>{{ copy.aboutText2 }}</p>
          <div class="stats">
            <div><strong>12+</strong><span>Years of practice</span></div>
            <div><strong>48</strong><span>Selected projects</span></div>
            <div><strong>16</strong><span>Exhibitions</span></div>
          </div>
        </div>
      </section>

      <!-- Journal -->
      <section id="journal" ref="journalSection" class="journal section-anchor">
        <div class="journal__head reveal-item">
          <div>
            <span class="section-label">{{ copy.journalLabel }}</span>
            <h2>{{ copy.journalTitle }}</h2>
          </div>
          <a href="#contact">{{ copy.viewArchive }} ↗</a>
        </div>
        <div class="journal-grid reveal-item">
          <article v-for="entry in journalEntries" :key="entry.title">
            <span>{{ entry.date }}</span>
            <h3>{{ entry.title }}</h3>
            <p>{{ entry.excerpt }}</p>
          </article>
        </div>
      </section>

      <!-- Contact -->
      <section id="contact" ref="contactSection" class="contact section-anchor">
        <div class="contact__visual reveal-item">
          <div class="contact__glow"></div>
          <span class="contact__monogram">AR</span>
        </div>
        <div class="contact__content reveal-item">
          <span class="section-label">{{ copy.contactLabel }}</span>
          <h2 v-html="copy.contactTitle"></h2>
          <a class="email-link" href="mailto:studio@arianKalantari.art"
            >studio@arianKalantari.art</a
          >
          <div class="contact__row">
            <span>{{ copy.locations }}</span>
            <a href="#home">{{ copy.backTop }} ↑</a>
          </div>
        </div>
      </section>

      <footer class="footer reveal-item">
        <div>
          <strong>Arian Kalantari</strong>
          <span>{{ copy.footerRole }}</span>
        </div>
        <p>© {{ year }} {{ copy.name }}. {{ copy.rights }}</p>
        <a href="#home">{{ copy.top }} ↑</a>
      </footer>
    </main>

    <!-- Project modal -->
    <Transition name="fade">
      <div
        v-if="selectedProject"
        class="modal"
        role="dialog"
        aria-modal="true"
        :aria-label="selectedProject.title"
        @click.self="closeProject"
      >
        <button
          class="modal__close"
          type="button"
          :aria-label="copy.close"
          @click="closeProject"
        >
          ×
        </button>

        <button
          class="modal__nav modal__nav--prev"
          type="button"
          :aria-label="copy.previousImage"
          :disabled="filteredWorks.length < 2"
          @click.stop="previousProject"
        >
          ‹
        </button>

        <button
          class="modal__nav modal__nav--next"
          type="button"
          :aria-label="copy.nextImage"
          :disabled="filteredWorks.length < 2"
          @click.stop="nextProject"
        >
          ›
        </button>

        <div class="modal__content">
          <div class="modal__image-wrap">
            <img
              :src="selectedProject.image"
              :alt="selectedProject.alt"
              width="1600"
              height="1100"
              decoding="async"
              fetchpriority="high"
              :class="{ 'is-loading': isModalImageLoading }"
              @load="handleModalImageLoad"
            />
            <div class="modal__counter" aria-live="polite">
              {{ formatGalleryNumber(currentProjectIndex + 1) }}
              <span>/</span>
              {{ formatGalleryNumber(filteredWorks.length) }}
            </div>
          </div>
          <div class="modal__copy" :key="selectedProject.id">
            <span class="card-kicker">{{ selectedProject.category }}</span>
            <h2>{{ selectedProject.title }}</h2>
            <p>{{ selectedProject.description }}</p>
            <div class="modal__hint">
              <span>{{ copy.previousImage }}</span>
              <i></i>
              <span>{{ copy.nextImage }}</span>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import {
  computed,
  nextTick,
  onBeforeUnmount,
  onMounted,
  ref,
  watch,
} from "vue";

// Local portfolio images. Put pic1.jpg ... pic11.jpg inside /public/images.
const localImage = (number) => `/images/pic${number}.jpg`;

const year = new Date().getFullYear();
const language = ref(localStorage.getItem("arian-language") || "en");
const languageClass = computed(() =>
  language.value === "fa" ? "is-fa" : "is-en",
);

const translations = {
  en: {
    primaryNavigation: "Primary navigation",
    homeLabel: "Arian Kalantari home",
    socialLinks: "Social links",
    name: "Arian Kalantari",
    role: "Artist / Photographer / Explorer",
    mobileRole: "Artist / Photographer",
    heroEyebrow: "Welcome to my portfolio",
    heroText:
      "I explore the intersection of light, space and material through photography, sculpture and visual studies.",
    viewWork: "View my work",
    projectFilters: "Project filters",
    viewOptions: "View options",
    gridView: "Grid view",
    listView: "List view",
    nextProjects: "Next projects",
    quote: "Light reveals<br>what time conceals.",
    aboutLabel: "About the artist",
    aboutTitle: "Objects, landscapes<br><em>and quiet light.</em>",
    aboutText1:
      "My practice moves between photography, material studies and slow observations of place. I collect fragments of architecture, natural forms and everyday objects, then rebuild them into quieter visual stories.",
    aboutText2:
      "Every project begins with the same question: what changes when we give an ordinary moment enough time to be seen?",
    years: "Years of practice",
    projects: "Selected projects",
    exhibitions: "Exhibitions",
    journalLabel: "Journal",
    journalTitle: "Notes from the studio",
    viewArchive: "View archive",
    contactLabel: "Start a project",
    contactTitle: "Have an idea worth<br><em>looking at slowly?</em>",
    locations: "London · Tehran",
    backTop: "Back to top",
    footerRole: "Visual artist & photographer",
    rights: "All rights reserved.",
    top: "Top",
    menu: "Open menu",
    close: "Close project",
    previousImage: "Previous image",
    nextImage: "Next image",
    loadingLabel: "Loading portfolio",
    videoProject: "Video project",
  },
  fa: {
    primaryNavigation: "ناوبری اصلی",
    homeLabel: "صفحه اصلی آرین کلانتری",
    socialLinks: "لینک‌های شبکه‌های اجتماعی",
    name: "آرین کلانتری",
    role: "هنرمند / عکاس / کاوشگر",
    mobileRole: "هنرمند / عکاس",
    heroEyebrow: "به پورتفولیوی من خوش آمدید",
    heroText:
      "در آثارم تلاقی نور، فضا و متریال را از مسیر عکاسی، مجسمه‌سازی و مطالعات بصری بررسی می‌کنم.",
    viewWork: "مشاهده آثار",
    projectFilters: "فیلتر آثار",
    viewOptions: "نوع نمایش",
    gridView: "نمایش شبکه‌ای",
    listView: "نمایش فهرستی",
    nextProjects: "آثار بعدی",
    quote: "نور، آنچه زمان پنهان می‌کند<br>را آشکار می‌سازد.",
    aboutLabel: "درباره هنرمند",
    aboutTitle: "اشیا، منظره‌ها<br><em>و نوری آرام.</em>",
    aboutText1:
      "کار من میان عکاسی، مطالعات متریال و مشاهده آرام مکان‌ها حرکت می‌کند. تکه‌هایی از معماری، فرم‌های طبیعی و اشیای روزمره را جمع می‌کنم و آن‌ها را به روایت‌هایی تصویری و آرام‌تر تبدیل می‌کنم.",
    aboutText2:
      "هر پروژه با یک پرسش آغاز می‌شود: وقتی به یک لحظه معمولی فرصت کافی برای دیده‌شدن بدهیم، چه چیزی تغییر می‌کند؟",
    years: "سال فعالیت",
    projects: "پروژه منتخب",
    exhibitions: "نمایشگاه",
    journalLabel: "ژورنال",
    journalTitle: "یادداشت‌هایی از استودیو",
    viewArchive: "مشاهده آرشیو",
    contactLabel: "شروع یک پروژه",
    contactTitle:
      "ایده‌ای داری که ارزش<br><em>آرام دیده‌شدن را داشته باشد؟</em>",
    locations: "لندن · تهران",
    backTop: "بازگشت به بالا",
    footerRole: "هنرمند بصری و عکاس",
    rights: "تمامی حقوق محفوظ است.",
    top: "بالا",
    menu: "باز کردن منو",
    close: "بستن پروژه",
    previousImage: "تصویر قبلی",
    nextImage: "تصویر بعدی",
    loadingLabel: "در حال بارگذاری پورتفولیو",
    videoProject: "پروژه ویدیویی",
  },
};

const copy = computed(() => translations[language.value]);

const toggleLanguage = () => {
  language.value = language.value === "en" ? "fa" : "en";
  activeFilter.value = language.value === "fa" ? "همه" : "All";
  localStorage.setItem("arian-language", language.value);
  document.documentElement.lang = language.value;
  document.documentElement.dir = language.value === "fa" ? "rtl" : "ltr";

  if (selectedProject.value) {
    selectedProject.value =
      localizedProjects.value.find(
        (project) => project.id === selectedProject.value.id,
      ) || null;
  }

  updateSeo();
};

const isMenuOpen = ref(false);
const isLoading = ref(true);
const selectedProject = ref(null);
const isModalImageLoading = ref(false);
const activeFilter = ref("All");
const activeSection = ref("home");
let loadingTimer = null;
let revealObserver = null;
const modalImageCache = new Map();

const icons = {
  home: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="m3 11 9-7 9 7"/><path d="M5 10v10h14V10"/><path d="M9 20v-6h6v6"/></svg>',
  grid: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="4" y="4" width="6" height="6" rx="1"/><rect x="14" y="4" width="6" height="6" rx="1"/><rect x="4" y="14" width="6" height="6" rx="1"/><rect x="14" y="14" width="6" height="6" rx="1"/></svg>',
  list: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M5 7h14M5 12h14M5 17h14"/></svg>',
  user: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><circle cx="12" cy="8" r="3.2"/><path d="M5.5 20c.8-3.5 3-5.3 6.5-5.3s5.7 1.8 6.5 5.3"/></svg>',
  journal:
    '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="5" y="3.5" width="14" height="17" rx="1.5"/><path d="M8 8h8M8 12h8M8 16h5"/></svg>',
  mail: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="4" y="5.5" width="16" height="13" rx="1.5"/><path d="m5 7 7 5 7-5"/></svg>',
  instagram:
    '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="4" y="4" width="16" height="16" rx="4"/><circle cx="12" cy="12" r="3.7"/><circle cx="17.3" cy="6.9" r=".7" fill="currentColor" stroke="none"/></svg>',
  x: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="m5 5 14 14M19 5 5 19"/></svg>',
  behance: '<span class="text-social">Bē</span>',
  link: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.4"><path d="M10 13.5 14 9.5"/><path d="m7.7 16.3-1.4 1.4a3.2 3.2 0 0 1-4.5-4.5l4.3-4.3a3.2 3.2 0 0 1 4.5 0"/><path d="m16.3 7.7 1.4-1.4a3.2 3.2 0 0 1 4.5 4.5l-4.3 4.3a3.2 3.2 0 0 1-4.5 0"/></svg>',
};

const navItems = computed(() => {
  const labels =
    language.value === "fa"
      ? {
          home: "خانه",
          works: "آثار",
          about: "درباره من",
          journal: "ژورنال",
          contact: "تماس",
        }
      : {
          home: "Home",
          works: "Works",
          about: "About",
          journal: "Journal",
          contact: "Contact",
        };
  return [
    { id: "home", label: labels.home, icon: icons.home },
    { id: "works", label: labels.works, icon: icons.grid },
    { id: "about", label: labels.about, icon: icons.user },
    { id: "journal", label: labels.journal, icon: icons.journal },
    { id: "contact", label: labels.contact, icon: icons.mail },
  ];
});

const socials = [
  { label: "Instagram", href: "https://instagram.com", icon: icons.instagram },
  { label: "X", href: "https://x.com", icon: icons.x },
  { label: "Behance", href: "https://behance.net", icon: icons.behance },
  { label: "Website link", href: "https://example.com", icon: icons.link },
];

const filters = computed(() =>
  language.value === "fa"
    ? [
        "همه",
        "هنری",
        "مطالعات",
        "آرشیو",
        "خبر",
        "پروژه‌های نقره‌ای",
        "دوچرخه‌سواری",
      ]
    : [
        "All",
        "Artwork",
        "Studies",
        "Archive",
        "News",
        "Silver Projects",
        "Cycling",
      ],
);

const filterMap = {
  همه: "All",
  هنری: "Artwork",
  مطالعات: "Studies",
  آرشیو: "Archive",
  خبر: "News",
  "پروژه‌های نقره‌ای": "Silver Projects",
  دوچرخه‌سواری: "Cycling",
};

const projects = [
  {
    id: 1,
    category: "Artwork",
    title: "Studio Fragments",
    description:
      "A series of visual experiments with form, light and material.",
    image: localImage(3),
    alt: "Image unavailable",
    size: "wide",
  },
  {
    id: 2,
    category: "Studies",
    title: "Light & Matter",
    description: "Explorations in texture, shadow and the unseen.",
    image: localImage(4),
    alt: "Image unavailable",
    size: "medium",
  },
  {
    id: 3,
    category: "Archive",
    title: "Rural Structures",
    description: "The quiet architecture of everyday life.",
    image: localImage(5),
    alt: "Image unavailable",
    size: "medium",
  },
  {
    id: 4,
    category: "Studies",
    title: "Material & Form",
    description: "How objects, surfaces and light shape our perception.",
    image: localImage(6),
    alt: "Image unavailable",
    size: "medium",
  },
  {
    id: 5,
    category: "Artwork",
    title: "Objects & Space",
    description: "Small objects. Big questions.",
    image: localImage(7),
    alt: "Image unavailable",
    size: "wide",
  },
  {
    id: 6,
    category: "Archive",
    title: "Interior Notes",
    description: "Spaces, moods and moments.",
    image: localImage(8),
    alt: "Image unavailable",
    size: "medium",
  },
  {
    id: 7,
    category: "Artwork",
    title: "The Hand & The Surface",
    description: "A tactile study of touch, motion and memory.",
    image: localImage(9),
    alt: "Image unavailable",
    size: "wide",
    video: true,
  },
  {
    id: 8,
    category: "Studies",
    title: "Concrete Silence",
    description: "Light passing through severe architectural geometry.",
    image: localImage(10),
    alt: "Image unavailable",
    size: "medium",
  },
  {
    id: 9,
    category: "News",
    title: "The Quiet Space",
    description: "New project — May 12, 2026.",
    image: localImage(11),
    alt: "Image unavailable",
    size: "medium",
  },
];

const projectText = {
  1: {
    fa: [
      "هنری",
      "قطعه‌های استودیو",
      "مجموعه‌ای از تجربه‌های بصری با فرم، نور و متریال.",
    ],
  },
  2: { fa: ["مطالعات", "نور و ماده", "کاوشی در بافت، سایه و چیزهای نادیده."] },
  3: { fa: ["آرشیو", "سازه‌های روستایی", "معماری آرام زندگی روزمره."] },
  4: {
    fa: [
      "مطالعات",
      "ماده و فرم",
      "چگونه اشیا، سطوح و نور ادراک ما را شکل می‌دهند.",
    ],
  },
  5: { fa: ["هنری", "اشیا و فضا", "اشیای کوچک؛ پرسش‌های بزرگ."] },
  6: { fa: ["آرشیو", "یادداشت‌های داخلی", "فضاها، حال‌وهوا و لحظه‌ها."] },
  7: { fa: ["هنری", "دست و سطح", "مطالعه‌ای لمسی درباره تماس، حرکت و خاطره."] },
  8: { fa: ["مطالعات", "سکوت بتن", "عبور نور از میان هندسه سخت معماری."] },
  9: { fa: ["خبر", "فضای آرام", "پروژه جدید — ۲۲ اردیبهشت ۱۴۰۵."] },
};

const localizedProject = (project) => {
  if (language.value !== "fa") return project;
  const [category, title, description] = projectText[project.id].fa;
  return { ...project, category, title, description };
};

const localizedProjects = computed(() => projects.map(localizedProject));

const journalEntries = computed(() => {
  if (language.value === "fa") {
    return [
      {
        date: "۲۷ مرداد ۱۴۰۵",
        title: "لبه یک سایه را پیدا کردن",
        excerpt:
          "درباره قدم‌زدن با دوربین، پیش از آنکه تصمیم بگیریم چه چیزی ارزش ثبت‌شدن دارد.",
      },
      {
        date: "۱۴ خرداد ۱۴۰۵",
        title: "یک اتاق می‌تواند خاطره را نگه دارد",
        excerpt:
          "یادداشت‌هایی درباره فضاهای داخلی، تکرار و اینکه چرا فضاهای خالی اغلب اشغال‌شده به نظر می‌رسند.",
      },
      {
        date: "۷ فروردین ۱۴۰۵",
        title: "جایی برای اتفاق‌ها باز کنیم",
        excerpt:
          "یادداشتی از استودیو درباره متریال‌های ناتمام، قاب‌های ناموفق و نقص‌های مفید.",
      },
    ];
  }
  return [
    {
      date: "18.08.2026",
      title: "Finding the edge of a shadow",
      excerpt:
        "On walking with a camera before deciding what deserves to be photographed.",
    },
    {
      date: "04.06.2026",
      title: "A room can hold a memory",
      excerpt:
        "Notes on interiors, repetition and why empty spaces often feel occupied.",
    },
    {
      date: "27.03.2026",
      title: "Making room for accidents",
      excerpt:
        "A studio note about unfinished materials, failed frames and useful imperfections.",
    },
  ];
});

const filteredWorks = computed(() => {
  const normalized =
    language.value === "fa"
      ? filterMap[activeFilter.value] || "All"
      : activeFilter.value;
  const localized = localizedProjects.value;
  if (normalized === "All") return localized;
  return localized.filter(
    (project) =>
      projects.find((item) => item.id === project.id)?.category === normalized,
  );
});

const currentProjectIndex = computed(() => {
  if (!selectedProject.value) return -1;
  return filteredWorks.value.findIndex(
    (project) => project.id === selectedProject.value.id,
  );
});

const formatGalleryNumber = (value) =>
  String(Math.max(value, 0)).padStart(2, "0");

const preloadImage = (src) => {
  if (!src) return Promise.resolve();

  if (modalImageCache.has(src)) {
    return modalImageCache.get(src);
  }

  const image = new Image();
  image.decoding = "async";
  image.src = src;

  const promise = (image.decode ? image.decode() : Promise.resolve())
    .catch(() => undefined)
    .then(() => image);

  modalImageCache.set(src, promise);
  return promise;
};

const preloadProjectWindow = (project) => {
  const gallery = filteredWorks.value;
  if (!project || gallery.length === 0) return Promise.resolve();

  const index = gallery.findIndex((item) => item.id === project.id);
  const safeIndex = index < 0 ? 0 : index;
  const urls = [
    gallery[safeIndex]?.image,
    gallery[(safeIndex - 1 + gallery.length) % gallery.length]?.image,
    gallery[(safeIndex + 1) % gallery.length]?.image,
  ].filter(Boolean);

  return Promise.all([...new Set(urls)].map(preloadImage));
};

const preloadAllProjectImages = () => {
  const urls = [
    ...new Set(projects.map((project) => project.image).filter(Boolean)),
  ];
  Promise.all(urls.map(preloadImage)).catch(() => undefined);
};

const handleModalImageLoad = () => {
  isModalImageLoading.value = false;
};

const goToProject = (step) => {
  const gallery = filteredWorks.value;
  if (gallery.length < 2) return;

  const currentIndex = currentProjectIndex.value;
  const safeIndex = currentIndex < 0 ? 0 : currentIndex;
  const nextIndex = (safeIndex + step + gallery.length) % gallery.length;
  const nextProjectItem = gallery[nextIndex];

  isModalImageLoading.value = true;
  selectedProject.value = nextProjectItem;

  preloadProjectWindow(nextProjectItem).then(() => {
    if (selectedProject.value?.id === nextProjectItem.id) {
      isModalImageLoading.value = false;
    }
  });
};

const previousProject = () => goToProject(-1);
const nextProject = () => goToProject(1);

// When the filter changes, replay the staggered card entrance
// (and make sure freshly created cards are observed too).
watch(
  () => filteredWorks.value.map((project) => project.id).join(","),
  async () => {
    await nextTick();
    document.querySelectorAll(".project-card").forEach((card) => {
      card.classList.remove("is-visible");
    });
    void document.body.offsetHeight; // flush styles so the animation restarts
    observeRevealElements();
  },
);

const closeMenu = () => {
  isMenuOpen.value = false;
};

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

const openProject = (project) => {
  isModalImageLoading.value = true;
  selectedProject.value = project;
  document.body.classList.add("modal-open");

  preloadProjectWindow(project).then(() => {
    if (selectedProject.value?.id === project.id) {
      isModalImageLoading.value = false;
    }
  });
};

const closeProject = () => {
  selectedProject.value = null;
  document.body.classList.remove("modal-open");
};

const updateActiveSection = () => {
  const sections = document.querySelectorAll(".section-anchor");
  let current = "home";
  sections.forEach((section) => {
    const rect = section.getBoundingClientRect();
    if (rect.top <= window.innerHeight * 0.35) current = section.id;
  });
  activeSection.value = current;
};

const onKeydown = (event) => {
  if (selectedProject.value) {
    if (event.key === "ArrowLeft") {
      event.preventDefault();
      previousProject();
      return;
    }

    if (event.key === "ArrowRight") {
      event.preventDefault();
      nextProject();
      return;
    }
  }

  if (event.key === "Escape") {
    isMenuOpen.value = false;
    closeProject();
  }
};

const observeRevealElements = () => {
  const elements = document.querySelectorAll(".reveal-item:not(.is-visible)");

  if (!elements.length) return;

  if (!("IntersectionObserver" in window)) {
    elements.forEach((element) => element.classList.add("is-visible"));
    return;
  }

  if (revealObserver) revealObserver.disconnect();

  revealObserver = new IntersectionObserver(
    (entries, observer) => {
      // Items entering the viewport together are staggered top-left → bottom-right
      const entering = entries
        .filter((entry) => entry.isIntersecting)
        .sort(
          (a, b) =>
            a.boundingClientRect.top - b.boundingClientRect.top ||
            a.boundingClientRect.left - b.boundingClientRect.left,
        );

      entering.forEach((entry, index) => {
        entry.target.style.setProperty(
          "--reveal-delay",
          String(Math.min(index, 5) * 110),
        );
        entry.target.classList.add("is-visible");
        observer.unobserve(entry.target);
      });
    },
    { threshold: 0.12, rootMargin: "0px 0px -7% 0px" },
  );

  elements.forEach((element) => revealObserver.observe(element));
};

const updateSeo = () => {
  const isFa = language.value === "fa";
  const title = isFa
    ? "آرین کلانتری — هنرمند، عکاس و کاوشگر بصری"
    : "Arian Kalantari — Artist, Photographer & Visual Explorer";
  const description = isFa
    ? "پورتفولیوی آرین کلانتری؛ هنرمند و عکاس با تمرکز بر نور، فضا، متریال، معماری و منظره‌های آرام."
    : "Arian Kalantari is a visual artist and photographer exploring light, space, material, architecture and quiet landscapes.";
  const ogDescription = isFa
    ? "آثار منتخب، مطالعات و یادداشت‌های بصری آرین کلانتری."
    : "Selected works, studies and visual notes by Arian Kalantari.";

  document.title = title;
  document.documentElement.lang = language.value;
  document.documentElement.dir = isFa ? "rtl" : "ltr";

  const meta = [
    ["name", "description", description],
    ["name", "robots", "index, follow"],
    ["name", "theme-color", "#0b0d10"],
    ["property", "og:title", title],
    ["property", "og:description", ogDescription],
    ["property", "og:type", "website"],
  ];

  meta.forEach(([attribute, key, content]) => {
    let tag = document.head.querySelector(`meta[${attribute}="${key}"]`);
    if (!tag) {
      tag = document.createElement("meta");
      tag.setAttribute(attribute, key);
      document.head.appendChild(tag);
    }
    tag.setAttribute("content", content);
  });
};

onMounted(() => {
  updateSeo();

  const canonical =
    document.head.querySelector('link[rel="canonical"]') ||
    document.createElement("link");
  canonical.rel = "canonical";
  canonical.href = window.location.href.split("#")[0];
  if (!canonical.parentNode) document.head.appendChild(canonical);

  window.addEventListener("scroll", updateActiveSection, { passive: true });
  window.addEventListener("keydown", onKeydown);
  updateActiveSection();

  // Reveal animations start only after the loader is gone,
  // so the visitor actually sees them.
  loadingTimer = window.setTimeout(() => {
    isLoading.value = false;
    nextTick(() => {
      requestAnimationFrame(observeRevealElements);

      // Warm the browser cache after the first paint so gallery opening
      // and previous/next navigation stay responsive on mobile.
      if ("requestIdleCallback" in window) {
        window.requestIdleCallback(preloadAllProjectImages, { timeout: 700 });
      } else {
        window.setTimeout(preloadAllProjectImages, 120);
      }
    });
  }, 1900);
});

onBeforeUnmount(() => {
  window.removeEventListener("scroll", updateActiveSection);
  window.removeEventListener("keydown", onKeydown);
  document.body.classList.remove("modal-open");

  if (loadingTimer) window.clearTimeout(loadingTimer);
  if (revealObserver) revealObserver.disconnect();
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Manrope:wght@400;500;600;700;800&family=Vazirmatn:wght@400;500;600;700;800&display=swap");

:root {
  --bg: #090b0d;
  --panel: #101316;
  --panel-soft: #14181c;
  --line: rgba(255, 255, 255, 0.11);
  --line-soft: rgba(255, 255, 255, 0.07);
  --text: #f5f3ee;
  --muted: #9b9d9e;
  --muted-light: #b8babb;
  --accent: #e8e5dc;
  --radius: 14px;
  --sidebar: 236px;
}

* {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  background: var(--bg);
}

body {
  margin: 0;
  min-width: 320px;
  background: var(--bg);
  color: var(--text);
  font-family: "DM Sans", system-ui, sans-serif;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
}

body.modal-open {
  overflow: hidden;
}

a {
  color: inherit;
  text-decoration: none;
}

button,
input {
  font: inherit;
}

button {
  color: inherit;
}

button:focus-visible,
a:focus-visible,
.project-card:focus-visible {
  outline: 2px solid rgba(255, 255, 255, 0.85);
  outline-offset: 3px;
}

img {
  display: block;
  width: 100%;
}

.site-shell {
  min-height: 100vh;
  background: var(--bg);
}

/* =========================================================
   ARTISTIC LOADER
========================================================= */
.art-loader {
  position: fixed;
  inset: 0;
  z-index: 300;
  display: grid;
  place-items: center;
  overflow: hidden;
  background: #0a0b0c;
  color: #efebe2;
  isolation: isolate;
}

.art-loader__grain {
  position: absolute;
  inset: -20%;
  z-index: -1;
  opacity: 0.5;
  background:
    repeating-linear-gradient(
      0deg,
      rgba(255, 255, 255, 0.016) 0 1px,
      transparent 1px 4px
    ),
    repeating-linear-gradient(
      90deg,
      rgba(255, 255, 255, 0.012) 0 1px,
      transparent 1px 6px
    );
  transform: rotate(-1deg);
}

.art-loader__scene {
  position: relative;
  width: min(690px, 82vw);
  height: min(500px, 58vh);
  margin-top: -38px;
}

.art-loader__label {
  position: absolute;
  top: 0;
  left: 0;
  color: rgba(239, 235, 226, 0.43);
  font-size: 8px;
  letter-spacing: 0.28em;
}

.art-loader__canvas {
  position: absolute;
  top: 56px;
  left: 50%;
  width: min(560px, 72vw);
  aspect-ratio: 1.48 / 1;
  transform: translateX(-50%) rotate(-1.6deg);
  animation: loaderCanvasIn 1.25s cubic-bezier(0.2, 0.76, 0.2, 1) both;
}

.art-loader__canvas::before {
  content: "";
  position: absolute;
  inset: 15px -16px -20px;
  border: 1px solid rgba(220, 211, 194, 0.08);
  background: rgba(255, 255, 255, 0.025);
  filter: blur(0.3px);
}

.art-loader__canvas-paper {
  position: absolute;
  inset: 0;
  overflow: hidden;
  border: 9px solid #1b1d1e;
  background: #e8e0d1;
  box-shadow:
    0 28px 65px rgba(0, 0, 0, 0.45),
    0 0 0 1px rgba(255, 255, 255, 0.06);
  animation: canvasPaperFloat 6s ease-in-out 1.3s infinite;
}

.art-loader__canvas-paper svg {
  display: block;
  width: 100%;
  height: 100%;
}

.art-loader__tape {
  position: absolute;
  top: -11px;
  width: 70px;
  height: 28px;
  background: rgba(213, 197, 164, 0.76);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.12);
}

.art-loader__tape--one {
  left: 11%;
  transform: rotate(-6deg);
}

.art-loader__tape--two {
  right: 9%;
  transform: rotate(7deg);
}

.loader-paint-wash {
  opacity: 0;
  transform-origin: 50% 100%;
  animation: paintReveal 1.85s 0.12s ease-out forwards;
}

.loader-paint-circle {
  opacity: 0;
  transform-origin: 40% 55%;
  animation: paintBloom 1.35s 0.25s cubic-bezier(0.22, 0.8, 0.2, 1) forwards;
}

.loader-paint-line,
.loader-paint-detail {
  stroke-linecap: round;
  stroke-linejoin: round;
  fill: none;
  stroke-dasharray: 1100;
  stroke-dashoffset: 1100;
}

.loader-paint-line--one {
  stroke: #5a6f7e;
  stroke-width: 13;
  stroke-opacity: 0.78;
  animation: paintDraw 1.8s 0.2s cubic-bezier(0.55, 0, 0.25, 1) forwards;
}

.loader-paint-line--two {
  stroke: #2c3236;
  stroke-width: 8;
  stroke-opacity: 0.7;
  animation: paintDraw 1.55s 0.48s cubic-bezier(0.55, 0, 0.25, 1) forwards;
}

.loader-paint-detail {
  stroke: #f6efe3;
  stroke-width: 4;
  stroke-opacity: 0.58;
  animation: paintDraw 1.2s 0.82s cubic-bezier(0.55, 0, 0.25, 1) forwards;
}

.art-loader__easel {
  position: absolute;
  left: 50%;
  bottom: 16px;
  width: 255px;
  height: 92px;
  transform: translateX(-50%);
}

.art-loader__easel span,
.art-loader__easel i {
  position: absolute;
  bottom: 0;
  display: block;
  width: 3px;
  height: 100%;
  transform-origin: bottom center;
  background: #1a1b1b;
  box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.03);
}

.art-loader__easel span:first-child {
  left: 42%;
  transform: rotate(13deg);
}

.art-loader__easel span:nth-child(2) {
  right: 42%;
  transform: rotate(-13deg);
}

.art-loader__easel i {
  left: 50%;
  width: 4px;
  height: 92%;
  transform: translateX(-50%);
}

.art-loader__palette {
  position: absolute;
  left: 8px;
  bottom: 32px;
  width: 102px;
  height: 70px;
  padding: 12px 15px;
  border-radius: 58% 43% 48% 52% / 53% 57% 45% 47%;
  background: #d2c4ad;
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.2);
  transform: rotate(-13deg);
  animation: paletteFloat 3.6s ease-in-out infinite;
}

.art-loader__palette b,
.art-loader__palette i {
  position: absolute;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.22);
}

.art-loader__palette b {
  right: 11px;
  bottom: 11px;
  width: 28px;
  height: 28px;
  border: 5px solid rgba(108, 96, 77, 0.2);
  background: transparent;
}

.art-loader__palette i:nth-child(2) {
  top: 12px;
  left: 16px;
  background: #9d493c;
}
.art-loader__palette i:nth-child(3) {
  top: 10px;
  left: 43px;
  background: #d49b54;
}
.art-loader__palette i:nth-child(4) {
  top: 24px;
  left: 28px;
  background: #5d7485;
}
.art-loader__palette i:nth-child(5) {
  top: 31px;
  right: 13px;
  background: #4d3d40;
}
.art-loader__palette i:nth-child(6) {
  bottom: 14px;
  left: 23px;
  background: #efe6d5;
}

.art-loader__brush {
  position: absolute;
  right: 5px;
  bottom: 24px;
  width: 160px;
  height: 18px;
  transform: rotate(-29deg);
  animation: brushFloat 2.8s ease-in-out infinite;
}

.art-loader__brush::before {
  content: "";
  position: absolute;
  left: 11px;
  top: 5px;
  width: 110px;
  height: 8px;
  border-radius: 999px;
  background: linear-gradient(
    90deg,
    #ad7656 0 48%,
    #e8d8bc 48% 62%,
    #1f2223 62% 100%
  );
}

.art-loader__brush span {
  position: absolute;
  left: 0;
  top: 2px;
  width: 30px;
  height: 14px;
  border-radius: 2px 60% 60% 2px;
  background: #e1d5bf;
  clip-path: polygon(0 50%, 70% 0, 100% 50%, 70% 100%);
}

.art-loader__brush i {
  position: absolute;
  right: 0;
  top: 5px;
  width: 28px;
  height: 8px;
  border-radius: 0 999px 999px 0;
  background: #8f744e;
}

.art-loader__content {
  position: absolute;
  right: 0;
  bottom: 10px;
  left: 0;
  z-index: 3;
  display: grid;
  justify-items: center;
  text-align: center;
}

.art-loader__eyebrow {
  color: rgba(239, 235, 226, 0.5);
  font-size: 8px;
  letter-spacing: 0.27em;
  text-transform: uppercase;
}

.art-loader__content strong {
  margin-top: 8px;
  font-family: Georgia, "Times New Roman", serif;
  font-size: clamp(28px, 4vw, 48px);
  font-weight: 400;
  line-height: 1;
  letter-spacing: -0.04em;
}

.art-loader__rule {
  position: relative;
  width: min(150px, 35vw);
  height: 1px;
  margin-top: 19px;
  overflow: hidden;
  background: rgba(255, 255, 255, 0.12);
}

.art-loader__rule i {
  display: block;
  width: 35%;
  height: 100%;
  background: #e8ddca;
  animation: loaderSweep 1.15s ease-in-out infinite;
}

.art-loader__footer {
  position: absolute;
  right: 28px;
  bottom: 24px;
  left: 28px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: rgba(214, 210, 201, 0.4);
  font-size: 7px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
}

@keyframes loaderCanvasIn {
  0% {
    opacity: 0;
    transform: translateX(-50%) translateY(18px) rotate(-4deg) scale(0.95);
  }
  100% {
    opacity: 1;
    transform: translateX(-50%) translateY(0) rotate(-1.6deg) scale(1);
  }
}

@keyframes paintReveal {
  0% {
    opacity: 0;
    transform: scaleY(0.2);
  }
  100% {
    opacity: 1;
    transform: scaleY(1);
  }
}

@keyframes paintBloom {
  0% {
    opacity: 0;
    transform: scale(0.25);
  }
  65% {
    opacity: 0.72;
    transform: scale(1.05);
  }
  100% {
    opacity: 0.62;
    transform: scale(1);
  }
}

@keyframes paintDraw {
  0% {
    stroke-dashoffset: 1100;
    opacity: 0;
  }
  12% {
    opacity: 1;
  }
  100% {
    stroke-dashoffset: 0;
    opacity: 1;
  }
}

@keyframes paletteFloat {
  0%,
  100% {
    transform: translateY(0) rotate(-13deg);
  }
  50% {
    transform: translateY(-7px) rotate(-9deg);
  }
}

@keyframes brushFloat {
  0%,
  100% {
    transform: rotate(-29deg) translate3d(0, 0, 0);
  }
  50% {
    transform: rotate(-24deg) translate3d(-5px, -5px, 0);
  }
}

@keyframes loaderSweep {
  0% {
    transform: translateX(-160%);
  }
  100% {
    transform: translateX(420%);
  }
}

@keyframes canvasPaperFloat {
  0%,
  100% {
    transform: translateY(0) rotate(0deg);
  }
  50% {
    transform: translateY(-4px) rotate(0.18deg);
  }
}

.art-loader-enter-active,
.art-loader-leave-active {
  transition:
    opacity 0.75s ease,
    visibility 0.75s ease;
}

.art-loader-enter-from,
.art-loader-leave-to {
  opacity: 0;
  visibility: hidden;
}

@media (max-width: 720px) {
  .art-loader__scene {
    height: min(430px, 56vh);
    margin-top: -28px;
  }

  .art-loader__canvas {
    top: 46px;
    width: min(480px, 88vw);
  }

  .art-loader__easel {
    bottom: 20px;
    width: 210px;
    height: 74px;
  }

  .art-loader__palette {
    left: 0;
    bottom: 28px;
    transform: scale(0.82) rotate(-13deg);
    transform-origin: left bottom;
  }

  .art-loader__brush {
    right: -8px;
    bottom: 19px;
    transform: scale(0.78) rotate(-29deg);
    transform-origin: right bottom;
  }

  .art-loader__content {
    bottom: 4px;
  }

  .art-loader__footer {
    right: 18px;
    bottom: 16px;
    left: 18px;
  }
}

@media (max-width: 420px) {
  .art-loader__label {
    top: 4px;
    font-size: 7px;
  }

  .art-loader__canvas {
    top: 50px;
    width: 92vw;
  }

  .art-loader__palette,
  .art-loader__brush {
    display: none;
  }

  .art-loader__easel {
    bottom: 32px;
  }

  .art-loader__content strong {
    font-size: 29px;
  }
}

.sidebar {
  position: fixed;
  inset: 0 auto 0 0;
  z-index: 20;
  display: flex;
  flex-direction: column;
  width: var(--sidebar);
  padding: 36px 30px 24px;
  overflow: hidden;
  border-right: 1px solid var(--line);
  background:
    linear-gradient(180deg, rgba(13, 16, 19, 0.98), rgba(8, 10, 12, 1)),
    var(--bg);
}

.sidebar::after {
  content: "";
  position: absolute;
  inset: auto -70px -130px -110px;
  height: 320px;
  background: radial-gradient(
    circle at 50% 10%,
    rgba(93, 96, 100, 0.18),
    transparent 62%
  );
  pointer-events: none;
}

.sidebar__top {
  position: relative;
  z-index: 1;
}

.brand {
  display: inline-flex;
}

.brand__mark {
  display: grid;
  width: 76px;
  height: 60px;
  place-items: center;
  color: #f4f1e8;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 29px;
  letter-spacing: -0.12em;
  border: 0;
}

.brand__copy {
  display: grid;
  gap: 3px;
  margin-top: 4px;
}

.brand__copy strong {
  font-family: Georgia, "Times New Roman", serif;
  font-size: 19px;
  font-weight: 500;
  letter-spacing: -0.03em;
}

.brand__copy span {
  max-width: 145px;
  color: #b4b5b4;
  font-size: 8px;
  line-height: 1.7;
  letter-spacing: 0.22em;
  text-transform: uppercase;
}

.sidebar__nav {
  position: relative;
  z-index: 1;
  display: grid;
  gap: 5px;
  margin-top: 38px;
}

.sidebar__nav a {
  display: grid;
  grid-template-columns: 20px 1fr auto;
  align-items: center;
  gap: 10px;
  min-height: 39px;
  padding: 0 12px 0 10px;
  border-radius: 999px;
  color: #cacbcc;
  font-size: 13px;
  transition:
    background 0.25s ease,
    color 0.25s ease,
    transform 0.25s ease;
}

.sidebar__nav a:hover,
.sidebar__nav a.active {
  color: #fff;
  background: rgba(255, 255, 255, 0.12);
}

.sidebar__nav a:hover {
  transform: translateX(2px);
}

.nav-icon {
  width: 19px;
  height: 19px;
}

.nav-icon svg {
  width: 100%;
  height: 100%;
}

.nav-arrow {
  color: #f0efeb;
  font-size: 20px;
  line-height: 1;
}

/* Language switch */
.language-switch {
  position: relative;
  display: inline-grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  width: 82px;
  height: 32px;
  margin-top: 22px;
  padding: 3px;
  border: 1px solid rgba(255, 255, 255, 0.13);
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.035);
  color: #777b7e;
  cursor: pointer;
  isolation: isolate;
  transition:
    border-color 0.25s ease,
    background 0.25s ease;
}

.language-switch:hover {
  border-color: rgba(255, 255, 255, 0.22);
}
.language-switch span {
  position: relative;
  z-index: 2;
  font-size: 8px;
  font-weight: 700;
  letter-spacing: 0.12em;
  transition: color 0.25s ease;
}
.language-switch span.active {
  color: #101214;
}
.language-switch i {
  position: absolute;
  inset: 3px auto 3px 3px;
  z-index: 1;
  width: 36px;
  border-radius: 999px;
  background: #ece9e0;
  transition: transform 0.28s cubic-bezier(0.22, 0.8, 0.22, 1);
}
.language-switch i.is-fa {
  transform: translateX(38px);
}
.language-switch--mobile {
  width: 72px;
  height: 34px;
  margin: 0;
}
.language-switch--mobile i {
  width: 31px;
}
.language-switch--mobile i.is-fa {
  transform: translateX(34px);
}

.is-fa {
  font-family: "Vazirmatn", "DM Sans", system-ui, sans-serif;
}
.is-fa .sidebar,
.is-fa .main-content,
.is-fa .mobile-header,
.is-fa .mobile-menu,
.is-fa .works,
.is-fa .about,
.is-fa .journal,
.is-fa .contact,
.is-fa .footer,
.is-fa .modal__content {
  direction: rtl;
}

.is-fa .sidebar {
  inset: 0 0 0 auto;
  border-right: 0;
  border-left: 1px solid var(--line);
}

.is-fa .main-content {
  margin-left: 0;
  margin-right: var(--sidebar);
}

.is-fa .sidebar__nav a {
  direction: rtl;
}

.is-fa .brand__copy,
.is-fa .sidebar__quote,
.is-fa .hero__content,
.is-fa .about__copy,
.is-fa .journal,
.is-fa .journal-grid article,
.is-fa .contact__content,
.is-fa .modal__copy,
.is-fa .footer div,
.is-fa .footer p {
  text-align: right;
}

.is-fa .brand__copy,
.is-fa .sidebar__quote,
.is-fa .hero__content,
.is-fa .about__copy,
.is-fa .journal,
.is-fa .contact__content,
.is-fa .modal__copy,
.is-fa .mobile-brand__text {
  direction: rtl;
}

.is-fa .language-switch {
  direction: ltr;
}

.is-fa .hero__overlay {
  background:
    linear-gradient(
      270deg,
      rgba(5, 7, 8, 0.76) 0%,
      rgba(6, 7, 9, 0.5) 34%,
      rgba(6, 7, 9, 0.08) 74%,
      rgba(6, 7, 9, 0.64) 100%
    ),
    linear-gradient(180deg, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.44));
}

.is-fa .hero__scroll {
  right: auto;
  left: 24px;
}

.is-fa .hero__cta {
  flex-direction: row;
  letter-spacing: 0.06em;
}

.is-fa .eyebrow {
  flex-direction: row;
  letter-spacing: 0.06em;
}

.is-fa .project-card__body {
  direction: rtl;
}

.is-fa .card-arrow {
  transform: scaleX(-1);
}

.is-fa .project-card:hover .card-arrow {
  transform: scaleX(-1) translate(-2px, -2px) rotate(2deg);
}

.is-fa .play-badge {
  right: 14px;
  left: auto;
}

.is-fa .mobile-brand__text {
  text-align: right;
}

.is-fa .mobile-menu nav,
.is-fa .mobile-menu a,
.is-fa .mobile-actions {
  direction: rtl;
}

.is-fa .mobile-menu a {
  font-family: "Vazirmatn", sans-serif;
}

.is-fa .journal-grid article {
  border-right: 0;
  border-left: 1px solid var(--line);
}

.is-fa .journal-grid article + article {
  padding-left: 24px;
  padding-right: 24px;
}

.is-fa .journal-grid article:last-child {
  border-left: 0;
}

.is-fa .contact__row {
  direction: rtl;
}

.is-fa .modal__close {
  right: auto;
  left: 22px;
}

.is-fa .modal__counter {
  right: auto;
  left: 18px;
}

/* Keep gallery navigation semantically mirrored in RTL. */
.is-fa .modal__nav--prev {
  left: auto;
  right: 22px;
}

.is-fa .modal__nav--next {
  right: auto;
  left: 22px;
}

.sidebar__quote {
  position: relative;
  z-index: 1;
  margin-top: auto;
  padding-top: 32px;
}

.sidebar__quote p {
  margin: 0;
  color: #c0c2c3;
  font-size: 11px;
  line-height: 1.7;
  letter-spacing: 0.04em;
}

.sidebar__quote span {
  display: block;
  width: 28px;
  height: 1px;
  margin-top: 18px;
  background: #dedbd2;
}

.sidebar__mountain {
  position: absolute;
  inset: auto -54px 92px -42px;
  height: 285px;
  opacity: 0.45;
  background:
    radial-gradient(
      circle at 50% 80%,
      rgba(102, 106, 108, 0.35),
      transparent 44%
    ),
    linear-gradient(
      135deg,
      transparent 38%,
      rgba(145, 150, 153, 0.14) 38%,
      rgba(145, 150, 153, 0.14) 52%,
      transparent 52%
    ),
    linear-gradient(
      25deg,
      transparent 31%,
      rgba(57, 62, 66, 0.85) 31%,
      rgba(57, 62, 66, 0.85) 56%,
      transparent 56%
    );
  clip-path: polygon(
    0 100%,
    10% 84%,
    19% 76%,
    29% 64%,
    38% 75%,
    50% 53%,
    60% 68%,
    72% 42%,
    82% 67%,
    95% 54%,
    100% 72%,
    100% 100%
  );
}

.sidebar__bottom {
  position: relative;
  z-index: 1;
  margin-top: 18px;
}

.socials {
  display: flex;
  gap: 13px;
  margin-bottom: 18px;
}

.socials a {
  display: inline-grid;
  width: 18px;
  height: 18px;
  place-items: center;
  color: #bfc0c1;
  transition:
    color 0.2s ease,
    transform 0.2s ease;
}

.socials a:hover {
  color: #fff;
  transform: translateY(-2px);
}

.socials svg {
  width: 100%;
  height: 100%;
}

.text-social {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: -0.05em;
}

.sidebar__bottom p,
.sidebar__bottom span {
  display: block;
  margin: 0;
  color: #808385;
  font-size: 9px;
  line-height: 1.7;
  letter-spacing: 0.04em;
}

.main-content {
  width: calc(100% - var(--sidebar));
  margin-left: var(--sidebar);
}

.hero {
  position: relative;
  min-height: 350px;
  height: min(45vw, 430px);
  overflow: hidden;
  border-bottom: 1px solid var(--line);
}

.hero__media,
.hero__media img,
.hero__overlay {
  position: absolute;
  inset: 0;
}

.hero__media img {
  height: 100%;
  object-fit: cover;
  object-position: center;
  filter: saturate(0.74) contrast(1.04) brightness(0.83);
}

.hero__overlay {
  background:
    linear-gradient(
      90deg,
      rgba(5, 7, 8, 0.76) 0%,
      rgba(6, 7, 9, 0.5) 34%,
      rgba(6, 7, 9, 0.08) 74%,
      rgba(6, 7, 9, 0.64) 100%
    ),
    linear-gradient(180deg, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.44));
}

.hero__content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  height: 100%;
  max-width: 610px;
  padding: 60px 68px;
}

.eyebrow,
.section-label,
.card-kicker {
  color: #e2dfd6;
  font-size: 15px;
  font-weight: 700;
  text-transform: uppercase;
}

.eyebrow {
  display: flex;
  align-items: center;
  gap: 11px;
}

.eyebrow i {
  display: block;
  width: 52px;
  height: 1px;
  background: rgba(255, 255, 255, 0.38);
}

.hero h1 {
  margin: 16px 0 13px;
  font-family: Georgia, "Times New Roman", serif;
  font-size: clamp(50px, 6vw, 84px);
  font-weight: 400;
  line-height: 0.94;
  letter-spacing: -0.05em;
}

.hero p {
  max-width: 500px;
  margin: 0;
  color: #d1d2d2;
  font-size: 13px;
  line-height: 1.75;
}

.hero__cta {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  margin-top: 24px;
  color: #f0eee8;
  font-size: 12px;
  font-weight: 700;
  text-transform: uppercase;
}

.circle-arrow {
  display: grid;
  width: 33px;
  height: 33px;
  place-items: center;
  border: 1px solid rgba(255, 255, 255, 0.65);
  border-radius: 50%;
  font-size: 14px;
  transition:
    background 0.25s ease,
    transform 0.25s ease;
}

.hero__cta:hover .circle-arrow {
  background: rgba(255, 255, 255, 0.11);
  transform: rotate(45deg);
}

.hero__scroll {
  position: absolute;
  right: 24px;
  top: 50%;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 9px;
  transform: translateY(-50%);
}

.hero__scroll span {
  width: 1px;
  height: 52px;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.9), transparent);
}

.hero__scroll b {
  color: rgba(255, 255, 255, 0.68);
  font-size: 7px;
  font-weight: 600;
  letter-spacing: 0.23em;
  text-transform: uppercase;
  writing-mode: vertical-rl;
}

.works {
  padding: 0 32px 46px;
}

.works__toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  min-height: 55px;
  gap: 20px;
}

.filters {
  display: flex;
  align-items: center;
  gap: 8px;
  overflow-x: auto;
  scrollbar-width: none;
  padding: 5px 0;
}

.filters::-webkit-scrollbar {
  display: none;
}

.filters button {
  flex: 0 0 auto;
  min-height: 30px;
  padding: 0 17px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 999px;
  background: transparent;
  color: #9b9ea0;
  font-size: 9px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.filters button:hover,
.filters button.selected {
  color: #111316;
  background: #efede6;
  border-color: #efede6;
}

.works__meta {
  display: flex;
  align-items: center;
  gap: 15px;
  flex: 0 0 auto;
  color: #777a7c;
  font-size: 8px;
}

.view-toggle {
  display: flex;
  align-items: center;
  gap: 4px;
}

.view-toggle button {
  display: grid;
  width: 22px;
  height: 22px;
  place-items: center;
  padding: 0;
  border: 0;
  background: transparent;
  color: #676b6e;
  cursor: pointer;
}

.view-toggle button.is-active {
  color: #dedbd2;
}

.view-toggle svg {
  width: 14px;
  height: 14px;
}

.pager {
  display: grid;
  width: 23px;
  height: 23px;
  place-items: center;
  padding: 0;
  border: 0;
  background: transparent;
  color: #d8d5ce;
  cursor: pointer;
  font-size: 18px;
}

.project-grid {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  gap: 12px;
}

.project-card {
  position: relative;
  min-height: 220px;
  overflow: hidden;
  border: 1px solid var(--line-soft);
  border-radius: var(--radius);
  background: var(--panel);
  cursor: pointer;
  isolation: isolate;
}

.project-card.wide {
  grid-column: span 4;
}

.project-card.medium {
  grid-column: span 3;
}

.project-card:nth-child(1),
.project-card:nth-child(5),
.project-card:nth-child(7) {
  grid-column: span 4;
}

.project-card img,
.project-card__shade {
  position: absolute;
  inset: 0;
  height: 100%;
}

.project-card img {
  object-fit: cover;
  object-position: center;
  transition:
    transform 0.65s cubic-bezier(0.2, 0.7, 0.2, 1),
    filter 0.4s ease;
  filter: saturate(0.76) contrast(1.03) brightness(0.87);
}

.project-card__shade {
  z-index: 1;
  background: linear-gradient(
    180deg,
    rgba(0, 0, 0, 0.02) 20%,
    rgba(0, 0, 0, 0.78) 100%
  );
}

.project-card:hover img,
.project-card:focus-visible img {
  transform: scale(1.045);
  filter: saturate(0.84) contrast(1.04) brightness(0.94);
}

.project-card__body {
  position: absolute;
  inset: auto 16px 15px;
  z-index: 2;
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 10px;
}

.card-kicker {
  display: block;
  margin-bottom: 7px;
  font-size: 6px;
}

.project-card h2 {
  margin: 0;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 20px;
  font-weight: 400;
  line-height: 1;
  letter-spacing: -0.03em;
}

.project-card p {
  max-width: 290px;
  margin: 7px 0 0;
  color: #c0c1c2;
  font-size: 9px;
  line-height: 1.55;
}

.card-arrow {
  display: grid;
  width: 28px;
  height: 28px;
  flex: 0 0 auto;
  place-items: center;
  border: 1px solid rgba(255, 255, 255, 0.48);
  border-radius: 50%;
  color: #f1eee7;
  font-size: 13px;
  transition:
    transform 0.25s ease,
    background 0.25s ease;
}

.project-card:hover .card-arrow {
  transform: translate(2px, -2px) rotate(2deg);
  background: rgba(255, 255, 255, 0.1);
}

.play-badge {
  position: absolute;
  top: 14px;
  left: 14px;
  z-index: 2;
  display: grid;
  width: 27px;
  height: 27px;
  place-items: center;
  border: 1px solid rgba(255, 255, 255, 0.48);
  border-radius: 50%;
  color: #fff;
  font-size: 8px;
}

.about {
  display: grid;
  grid-template-columns: minmax(260px, 0.85fr) minmax(0, 1.15fr);
  gap: clamp(38px, 7vw, 100px);
  align-items: center;
  padding: 110px 68px;
  border-top: 1px solid var(--line);
  border-bottom: 1px solid var(--line);
}

.about__image-wrap {
  min-height: 470px;
  overflow: hidden;
  border-radius: var(--radius);
}

.about__image-wrap img {
  height: 100%;
  min-height: 470px;
  object-fit: cover;
  filter: grayscale(0.24) saturate(0.7) brightness(0.78);
}

.about__copy {
  max-width: 640px;
}

.about h2,
.journal h2,
.contact h2 {
  margin: 14px 0 20px;
  font-family: Georgia, "Times New Roman", serif;
  font-size: clamp(42px, 5vw, 72px);
  font-weight: 400;
  line-height: 0.98;
  letter-spacing: -0.05em;
}

.about h2 em,
.contact h2 em {
  color: #8f9395;
  font-style: normal;
}

.about p {
  max-width: 580px;
  margin: 0 0 15px;
  color: var(--muted-light);
  font-size: 14px;
  line-height: 1.8;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 12px;
  margin-top: 38px;
  padding-top: 24px;
  border-top: 1px solid var(--line);
}

.stats div {
  display: grid;
  gap: 5px;
}

.stats strong {
  font-family: Georgia, "Times New Roman", serif;
  font-size: 34px;
  font-weight: 400;
}

.stats span {
  color: #83878a;
  font-size: 8px;
  letter-spacing: 0.13em;
  text-transform: uppercase;
}

.journal {
  padding: 100px 68px;
}

.journal__head {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 30px;
  margin-bottom: 55px;
}

.journal__head h2 {
  margin-bottom: 0;
}

.journal__head > a {
  color: #dad7cf;
  font-size: 10px;
}

.journal-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  border-top: 1px solid var(--line);
}

.journal-grid article {
  min-height: 230px;
  padding: 28px 24px 24px 0;
  border-right: 1px solid var(--line);
}

.journal-grid article + article {
  padding-left: 24px;
}

.journal-grid article:last-child {
  border-right: 0;
}

.journal-grid span {
  color: #7f8386;
  font-size: 8px;
  letter-spacing: 0.12em;
}

.journal-grid h3 {
  max-width: 280px;
  margin: 20px 0 10px;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 29px;
  font-weight: 400;
  line-height: 1.03;
}

.journal-grid p {
  max-width: 320px;
  margin: 0;
  color: #909397;
  font-size: 11px;
  line-height: 1.65;
}

.contact {
  display: grid;
  grid-template-columns: 0.7fr 1.3fr;
  min-height: 510px;
  border-top: 1px solid var(--line);
}

.contact__visual {
  position: relative;
  overflow: hidden;
  background: #0d1013;
}

.contact__visual::before,
.contact__visual::after {
  content: "";
  position: absolute;
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 50%;
  transform: rotate(-28deg);
}

.contact__visual::before {
  inset: -30% -30% -22% -22%;
}

.contact__visual::after {
  inset: 14% -40% -8% 14%;
}

.contact__glow {
  position: absolute;
  inset: 28% 18% 18%;
  background: radial-gradient(
    circle,
    rgba(218, 208, 190, 0.13),
    transparent 58%
  );
  filter: blur(12px);
}

.contact__monogram {
  position: absolute;
  inset: 50% auto auto 50%;
  transform: translate(-50%, -50%);
  font-family: Georgia, "Times New Roman", serif;
  font-size: clamp(90px, 12vw, 175px);
  font-weight: 400;
  letter-spacing: -0.16em;
  color: rgba(237, 232, 219, 0.85);
}

.contact__content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 80px 68px;
}

.contact h2 {
  margin-bottom: 34px;
}

.email-link {
  width: fit-content;
  padding-bottom: 7px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.5);
  font-family: Georgia, "Times New Roman", serif;
  font-size: clamp(20px, 2vw, 30px);
}

.contact__row {
  display: flex;
  justify-content: space-between;
  gap: 25px;
  margin-top: 95px;
  padding-top: 18px;
  border-top: 1px solid var(--line);
  color: #7e8285;
  font-size: 9px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.contact__row a {
  color: #dad7cf;
}

.footer {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  align-items: center;
  gap: 25px;
  min-height: 90px;
  padding: 22px 32px;
  border-top: 1px solid var(--line);
  color: #7d8184;
  font-size: 9px;
}

.footer div {
  display: grid;
  gap: 4px;
}

.footer strong {
  color: #d5d3cd;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 15px;
  font-weight: 400;
}

.footer a {
  justify-self: end;
  color: #d0cdc5;
}

.mobile-header,
.mobile-menu {
  display: none;
}

/* =========================================================
   REFINED MOTION
========================================================= */
.reveal-item {
  opacity: 0;
  transform: translate3d(0, 26px, 0);
  filter: blur(4px);
  transition:
    opacity 0.8s cubic-bezier(0.22, 0.8, 0.24, 1),
    transform 0.9s cubic-bezier(0.22, 0.8, 0.24, 1),
    filter 0.8s ease;
  will-change: opacity, transform, filter;
}

.reveal-item.is-visible {
  opacity: 1;
  transform: translate3d(0, 0, 0);
  filter: blur(0);
}

.site-ready .main-content {
  animation: siteReveal 0.95s cubic-bezier(0.22, 0.8, 0.24, 1) both;
}

.site-ready .hero__media img {
  animation: heroBreath 16s ease-in-out 0.2s infinite alternate;
}

.hero__media {
  overflow: hidden;
}

.about__image-wrap,
.contact__visual {
  isolation: isolate;
}

.about__image-wrap::after {
  content: "";
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;
  background: linear-gradient(
    125deg,
    transparent 26%,
    rgba(255, 255, 255, 0.08) 48%,
    transparent 70%
  );
  transform: translateX(-115%);
  transition: transform 1.1s cubic-bezier(0.22, 0.8, 0.24, 1);
}

.about__image-wrap {
  position: relative;
}

.about__image-wrap:hover::after {
  transform: translateX(115%);
}

.project-card {
  transform: translate3d(0, 0, 0);
  transition:
    transform 0.45s cubic-bezier(0.22, 0.8, 0.22, 1),
    box-shadow 0.45s ease,
    border-color 0.35s ease;
}

.project-card::after {
  content: "";
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;
  background: linear-gradient(
    110deg,
    transparent 30%,
    rgba(255, 255, 255, 0.13) 48%,
    transparent 66%
  );
  transform: translateX(-125%);
  transition: transform 0.9s cubic-bezier(0.22, 0.8, 0.24, 1);
}

.project-card:hover,
.project-card:focus-visible {
  transform: translate3d(0, -5px, 0);
  border-color: rgba(255, 255, 255, 0.18);
  box-shadow: 0 18px 42px rgba(0, 0, 0, 0.22);
}

.project-card:hover::after,
.project-card:focus-visible::after {
  transform: translateX(125%);
}

.project-card:nth-child(2) {
  transition-delay: 0.03s;
}
.project-card:nth-child(3) {
  transition-delay: 0.06s;
}
.project-card:nth-child(4) {
  transition-delay: 0.09s;
}

.journal-grid article {
  position: relative;
  transition:
    color 0.3s ease,
    transform 0.35s cubic-bezier(0.22, 0.8, 0.24, 1),
    padding 0.35s ease;
}

.journal-grid article::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: 0;
  width: 30px;
  height: 1px;
  background: rgba(255, 255, 255, 0.48);
  transform-origin: left center;
  transform: scaleX(0.45);
  transition: transform 0.35s ease;
}

.journal-grid article:hover {
  transform: translateY(-5px);
}

.journal-grid article:hover::after {
  transform: scaleX(1);
}

.contact__glow {
  animation: contactGlow 7s ease-in-out infinite alternate;
}

.contact__monogram {
  animation: monogramFloat 5.5s ease-in-out infinite;
}

.modal__content {
  animation: modalRise 0.55s cubic-bezier(0.22, 0.8, 0.24, 1) both;
}

.modal__image-wrap {
  overflow: hidden;
}

.modal__content img {
  transform: scale(1.012);
  transition: transform 0.8s cubic-bezier(0.22, 0.8, 0.24, 1);
}

.modal:hover .modal__content img {
  transform: scale(1);
}

.modal__nav:hover:not(:disabled) {
  transform: translateY(-50%) scale(1.06);
}

@keyframes siteReveal {
  from {
    opacity: 0.86;
  }
  to {
    opacity: 1;
  }
}

@keyframes heroBreath {
  0% {
    transform: scale(1.015);
  }
  100% {
    transform: scale(1.06);
  }
}

@keyframes contactGlow {
  0% {
    transform: scale(0.98);
    opacity: 0.74;
  }
  100% {
    transform: scale(1.06);
    opacity: 1;
  }
}

@keyframes monogramFloat {
  0%,
  100% {
    transform: translateY(0) rotate(0deg);
  }
  50% {
    transform: translateY(-6px) rotate(-1deg);
  }
}

@keyframes modalRise {
  from {
    opacity: 0;
    transform: translateY(18px) scale(0.985);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    scroll-behavior: auto !important;
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }

  .reveal-item {
    opacity: 1;
    transform: none;
    filter: none;
  }
}

.modal {
  position: fixed;
  inset: 0;
  z-index: 100;
  display: grid;
  place-items: center;
  padding: 28px;
  background: rgba(3, 4, 5, 0.82);
  backdrop-filter: blur(16px);
}

.modal__content {
  width: min(1100px, 100%);
  max-height: calc(100vh - 56px);
  overflow: auto;
  border: 1px solid var(--line);
  border-radius: 18px;
  background: #0d1012;
  box-shadow: 0 30px 80px rgba(0, 0, 0, 0.38);
}

.modal__image-wrap {
  position: relative;
  background: #090b0d;
}

.modal__content img {
  max-height: 70vh;
  object-fit: cover;
}

.modal__counter {
  position: absolute;
  right: 18px;
  bottom: 16px;
  display: inline-flex;
  align-items: center;
  gap: 7px;
  min-width: 56px;
  padding: 7px 9px;
  border: 1px solid rgba(255, 255, 255, 0.18);
  border-radius: 999px;
  background: rgba(6, 8, 10, 0.62);
  backdrop-filter: blur(9px);
  color: rgba(242, 239, 231, 0.82);
  font-size: 8px;
  line-height: 1;
  letter-spacing: 0.12em;
}

.modal__counter span {
  color: rgba(242, 239, 231, 0.3);
}

.modal__copy {
  padding: 24px 26px 28px;
}

.modal__copy h2 {
  margin: 10px 0 6px;
  font-family: Georgia, "Times New Roman", serif;
  font-size: 34px;
  font-weight: 400;
}

.modal__copy p {
  max-width: 620px;
  margin: 0;
  color: #9fa2a5;
  font-size: 11px;
  line-height: 1.7;
}

.modal__hint {
  display: flex;
  align-items: center;
  gap: 9px;
  margin-top: 18px;
  color: #777b7e;
  font-size: 7px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.modal__hint i {
  width: 26px;
  height: 1px;
  background: rgba(255, 255, 255, 0.2);
}

.modal__close,
.modal__nav {
  position: absolute;
  z-index: 3;
  display: grid;
  place-items: center;
  border: 1px solid rgba(255, 255, 255, 0.28);
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.25);
  color: #fff;
  cursor: pointer;
  backdrop-filter: blur(9px);
  transition:
    background 0.2s ease,
    border-color 0.2s ease,
    transform 0.2s ease,
    opacity 0.2s ease;
}

.modal__close:hover,
.modal__nav:hover:not(:disabled) {
  border-color: rgba(255, 255, 255, 0.5);
  background: rgba(255, 255, 255, 0.08);
}

.modal__close {
  top: 18px;
  right: 22px;
  width: 38px;
  height: 38px;
  font-size: 24px;
  line-height: 1;
}

.modal__nav {
  top: 50%;
  width: 46px;
  height: 46px;
  margin-top: -23px;
  font-size: 31px;
  line-height: 1;
}

.modal__nav--prev {
  left: 22px;
}

.modal__nav--next {
  right: 22px;
}

.modal__nav:disabled {
  opacity: 0.35;
  cursor: default;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.visually-hidden {
  position: fixed;
  width: 1px;
  height: 1px;
  overflow: hidden;
  padding: 0;
  border: 0;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
}

@media (max-width: 1200px) {
  :root {
    --sidebar: 214px;
  }

  .sidebar {
    padding-inline: 24px;
  }

  .hero__content,
  .about,
  .journal,
  .contact__content {
    padding-inline: 48px;
  }

  .works {
    padding-inline: 24px;
  }

  .project-card:nth-child(n),
  .project-card.wide,
  .project-card.medium {
    grid-column: span 4;
  }

  .project-card {
    min-height: 210px;
  }
}

@media (max-width: 980px) {
  :root {
    --sidebar: 190px;
  }

  .sidebar {
    padding: 30px 20px 20px;
  }

  .brand__mark {
    font-size: 25px;
  }

  .sidebar__nav a {
    font-size: 12px;
  }

  .main-content {
    width: calc(100% - var(--sidebar));
    margin-left: var(--sidebar);
  }

  .hero {
    height: 390px;
  }

  .hero__content {
    padding-inline: 35px;
  }

  .hero h1 {
    font-size: clamp(46px, 7vw, 64px);
  }

  .works__toolbar {
    align-items: flex-start;
    flex-direction: column;
    padding: 10px 0 7px;
  }

  .works__meta {
    align-self: flex-end;
    margin-top: -40px;
  }

  .project-card:nth-child(n),
  .project-card.wide,
  .project-card.medium {
    grid-column: span 6;
  }

  .project-card:nth-child(1),
  .project-card:nth-child(5),
  .project-card:nth-child(7) {
    grid-column: span 6;
  }

  .about,
  .contact {
    grid-template-columns: 1fr;
  }

  .about {
    gap: 45px;
    padding-top: 80px;
    padding-bottom: 80px;
  }

  .about__image-wrap {
    min-height: 350px;
    max-width: 700px;
  }

  .about__image-wrap img {
    min-height: 350px;
  }

  .contact__visual {
    min-height: 320px;
  }
}

@media (max-width: 720px) {
  .sidebar {
    display: none;
  }

  .main-content {
    width: 100%;
    margin-left: 0;
    padding-top: 64px;
  }

  .mobile-header {
    position: fixed;
    inset: 0 0 auto;
    z-index: 60;
    display: flex;
    align-items: center;
    justify-content: space-between;
    min-height: 64px;
    padding: 0 18px;
    border-bottom: 1px solid var(--line);
    background: rgba(8, 10, 12, 0.86);
    backdrop-filter: blur(18px);
  }

  .mobile-brand {
    display: inline-flex;
    align-items: center;
    gap: 9px;
  }

  .mobile-brand .brand__mark {
    width: 41px;
    height: 41px;
    font-size: 19px;
  }

  .mobile-brand__text {
    display: grid;
    gap: 2px;
  }

  .mobile-brand__text strong {
    font-family: Georgia, "Times New Roman", serif;
    font-size: 14px;
    font-weight: 400;
  }

  .mobile-brand__text small {
    color: #85898b;
    font-size: 6px;
    letter-spacing: 0.14em;
    text-transform: uppercase;
  }

  .mobile-actions {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .icon-button {
    display: grid;
    width: 38px;
    height: 38px;
    place-items: center;
    border: 0;
    border-radius: 50%;
    background: transparent;
    cursor: pointer;
  }

  .icon-button svg {
    width: 18px;
    height: 18px;
  }

  .menu-button {
    position: relative;
    display: grid;
    gap: 5px;
  }

  .menu-line {
    display: block;
    width: 21px;
    height: 1px;
    background: #efede7;
  }

  .mobile-menu {
    position: fixed;
    inset: 64px 0 0;
    z-index: 55;
    display: block;
    padding: 12px 18px 30px;
    background: rgba(7, 9, 11, 0.97);
    backdrop-filter: blur(20px);
  }

  .mobile-menu nav {
    display: grid;
    border-top: 1px solid var(--line);
  }

  .mobile-menu a {
    display: flex;
    align-items: center;
    justify-content: space-between;
    min-height: 67px;
    border-bottom: 1px solid var(--line);
    font-family: Georgia, "Times New Roman", serif;
    font-size: 26px;
  }

  .hero {
    height: 73vh;
    min-height: 560px;
  }

  .hero__content {
    justify-content: flex-end;
    padding: 70px 22px 68px;
  }

  .hero h1 {
    max-width: 330px;
    font-size: clamp(51px, 15vw, 78px);
  }

  .hero p {
    max-width: 330px;
    font-size: 12px;
  }

  .hero__scroll {
    top: auto;
    right: 17px;
    bottom: 25px;
    transform: none;
  }

  .hero__scroll span {
    height: 34px;
  }

  .works {
    padding: 0 14px 34px;
  }

  .works__toolbar {
    gap: 4px;
    min-height: auto;
    padding: 10px 2px;
  }

  .filters {
    width: 100%;
  }

  .filters button {
    min-height: 29px;
    padding-inline: 14px;
    font-size: 8px;
  }

  .works__meta {
    width: 100%;
    justify-content: flex-end;
    margin-top: 0;
    margin-bottom: 2px;
  }

  .project-grid {
    grid-template-columns: 1fr;
    gap: 10px;
  }

  .project-card,
  .project-card:nth-child(n),
  .project-card.wide,
  .project-card.medium,
  .project-card:nth-child(1),
  .project-card:nth-child(5),
  .project-card:nth-child(7) {
    grid-column: auto;
    min-height: 325px;
  }

  .project-card h2 {
    font-size: 24px;
  }

  .project-card p {
    max-width: 260px;
    font-size: 9px;
  }

  .about,
  .journal,
  .contact__content {
    padding-inline: 22px;
  }

  .about {
    padding-top: 65px;
    padding-bottom: 65px;
  }

  .about__image-wrap,
  .about__image-wrap img {
    min-height: 340px;
  }

  .about h2,
  .journal h2,
  .contact h2 {
    font-size: clamp(42px, 12vw, 59px);
  }

  .about p {
    font-size: 13px;
  }

  .stats {
    margin-top: 28px;
  }

  .stats strong {
    font-size: 27px;
  }

  .stats span {
    font-size: 6.5px;
  }

  .journal {
    padding-top: 66px;
    padding-bottom: 64px;
  }

  .journal__head {
    align-items: flex-start;
    flex-direction: column;
    margin-bottom: 35px;
  }

  .journal-grid {
    grid-template-columns: 1fr;
  }

  .journal-grid article,
  .journal-grid article + article {
    min-height: auto;
    padding: 24px 0;
    border-right: 0;
    border-bottom: 1px solid var(--line);
  }

  .journal-grid article:last-child {
    border-bottom: 0;
  }

  .journal-grid h3 {
    max-width: none;
    font-size: 28px;
  }

  .contact__visual {
    min-height: 290px;
  }

  .contact__content {
    min-height: 430px;
    padding-top: 70px;
    padding-bottom: 55px;
  }

  .contact__row {
    margin-top: auto;
  }

  .footer {
    grid-template-columns: 1fr auto;
    gap: 12px;
    padding-inline: 18px;
  }

  .footer p {
    display: none;
  }

  .footer a {
    justify-self: end;
  }

  .modal {
    padding: 13px;
  }

  .modal__content {
    max-height: calc(100vh - 26px);
    border-radius: 14px;
  }

  .modal__content img {
    max-height: 54vh;
  }

  .modal__close {
    top: 13px;
    right: 13px;
  }

  .modal__nav {
    top: 34%;
    width: 40px;
    height: 40px;
    margin-top: -20px;
    font-size: 28px;
  }

  .modal__nav--prev {
    left: 9px;
  }

  .modal__nav--next {
    right: 9px;
  }

  .modal__counter {
    right: 11px;
    bottom: 11px;
  }

  .modal__copy {
    padding: 19px 18px 21px;
  }

  .modal__copy h2 {
    font-size: 29px;
  }
}

@media (max-width: 420px) {
  .hero {
    min-height: 530px;
  }

  .hero h1 {
    font-size: 50px;
  }

  .hero p {
    max-width: 285px;
  }

  .project-card,
  .project-card:nth-child(n),
  .project-card.wide,
  .project-card.medium,
  .project-card:nth-child(1),
  .project-card:nth-child(5),
  .project-card:nth-child(7) {
    min-height: 300px;
  }

  .about__image-wrap,
  .about__image-wrap img {
    min-height: 290px;
  }

  .stats {
    gap: 7px;
  }

  .stats strong {
    font-size: 24px;
  }

  .stats span {
    line-height: 1.35;
  }
}

@media (max-width: 720px) {
  .art-loader__painting {
    width: 112vw;
    height: 70vw;
  }

  .art-loader__monogram {
    width: 58px;
    height: 58px;
    margin-bottom: 16px;
  }

  .art-loader__footer {
    right: 17px;
    bottom: 17px;
    left: 17px;
  }
}

@media (max-width: 420px) {
  .art-loader__content strong {
    font-size: 31px;
  }

  .art-loader__painting {
    width: 128vw;
    height: 82vw;
  }
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    scroll-behavior: auto !important;
    transition-duration: 0.01ms !important;
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
  }
}

/* Persian layout overrides: mirror the full composition without changing English. */
@media (max-width: 1200px) {
  .is-fa .main-content {
    margin-left: 0;
    margin-right: var(--sidebar);
  }
}

@media (max-width: 980px) {
  .is-fa .main-content {
    margin-left: 0;
    margin-right: var(--sidebar);
  }
}

@media (max-width: 720px) {
  .is-fa .main-content {
    margin: 0;
  }

  .is-fa .mobile-header {
    direction: rtl;
  }

  .is-fa .hero__scroll {
    right: auto;
    left: 17px;
  }

  .is-fa .modal__close {
    right: auto;
    left: 13px;
  }

  .is-fa .modal__counter {
    right: auto;
    left: 11px;
  }

  .is-fa .modal__nav--prev {
    left: auto;
    right: 9px;
  }

  .is-fa .modal__nav--next {
    right: auto;
    left: 9px;
  }
}

/* =========================================================
   MOTION v2
   Keep this block LAST in <style>: it overrides the older
   reveal / hero motion rules above by source order.
========================================================= */
:root {
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --rail-from: -28px;
}

.is-fa {
  --rail-from: 28px;
}

/* Stagger index (--n) for children of animated groups */
:is(
    .hero__content,
    .about__copy,
    .contact__content,
    .journal__head,
    .journal-grid,
    .sidebar__nav,
    .modal__copy
  )
  > :nth-child(1) {
  --n: 0;
}
:is(
    .hero__content,
    .about__copy,
    .contact__content,
    .journal__head,
    .journal-grid,
    .sidebar__nav,
    .modal__copy
  )
  > :nth-child(2) {
  --n: 1;
}
:is(
    .hero__content,
    .about__copy,
    .contact__content,
    .journal__head,
    .journal-grid,
    .sidebar__nav,
    .modal__copy
  )
  > :nth-child(3) {
  --n: 2;
}
:is(
    .hero__content,
    .about__copy,
    .contact__content,
    .journal__head,
    .journal-grid,
    .sidebar__nav,
    .modal__copy
  )
  > :nth-child(4) {
  --n: 3;
}
:is(
    .hero__content,
    .about__copy,
    .contact__content,
    .journal__head,
    .journal-grid,
    .sidebar__nav,
    .modal__copy
  )
  > :nth-child(5) {
  --n: 4;
}

/* ---------- Generic reveal: no blur, quickly opaque, calm movement ---------- */
.reveal-item {
  filter: none;
  will-change: auto;
}

.reveal-item:not(.project-card) {
  transition: none;
}

.reveal-item:not(.is-visible) {
  opacity: 0;
}

.reveal-item.is-visible {
  opacity: 1;
  filter: none;
  animation: revealRise 1s var(--ease-out) backwards;
  animation-delay: calc(var(--reveal-delay, 0) * 1ms);
}

@keyframes revealRise {
  0% {
    opacity: 0;
    transform: translate3d(0, 42px, 0);
  }
  40% {
    opacity: 1;
  }
  100% {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}

/* ---------- Project cards: rising curtain + image settling from a slow zoom ---------- */
.project-card.reveal-item.is-visible {
  animation: cardUnveil 1.15s var(--ease-out) backwards;
  animation-delay: calc(var(--reveal-delay, 0) * 1ms);
}

.project-card.reveal-item.is-visible img {
  animation: imageSettle 1.6s var(--ease-out) backwards;
  animation-delay: calc(var(--reveal-delay, 0) * 1ms);
}

@keyframes cardUnveil {
  0% {
    opacity: 0;
    transform: translate3d(0, 56px, 0);
    clip-path: inset(22% 0 0 0 round 14px);
  }
  30% {
    opacity: 1;
  }
  100% {
    opacity: 1;
    transform: translate3d(0, 0, 0);
    clip-path: inset(0 0 0 0 round 14px);
  }
}

@keyframes imageSettle {
  from {
    transform: scale(1.28);
  }
  to {
    transform: scale(1);
  }
}

/* ---------- About image: rising curtain ---------- */
.about__image-wrap.reveal-item.is-visible {
  animation: curtainUp 1.5s var(--ease-out) backwards;
  animation-delay: calc(var(--reveal-delay, 0) * 1ms);
}

.about__image-wrap.reveal-item.is-visible img {
  animation: imageSettle 2s var(--ease-out) backwards;
  animation-delay: calc(var(--reveal-delay, 0) * 1ms);
}

@keyframes curtainUp {
  0% {
    opacity: 1;
    clip-path: inset(100% 0 0 0 round 14px);
  }
  100% {
    opacity: 1;
    clip-path: inset(0 0 0 0 round 14px);
  }
}

/* ---------- Text groups: container stays put, children stagger in ---------- */
.about__copy.reveal-item,
.journal__head.reveal-item,
.contact__content.reveal-item,
.journal-grid.reveal-item {
  opacity: 1;
  transform: none;
}

.about__copy.reveal-item.is-visible,
.journal__head.reveal-item.is-visible,
.contact__content.reveal-item.is-visible,
.journal-grid.reveal-item.is-visible {
  animation: none;
}

.about__copy.reveal-item:not(.is-visible) > *,
.journal__head.reveal-item:not(.is-visible) > *,
.contact__content.reveal-item:not(.is-visible) > *,
.journal-grid.reveal-item:not(.is-visible) > * {
  opacity: 0;
}

.about__copy.is-visible > *,
.journal__head.is-visible > *,
.contact__content.is-visible > *,
.journal-grid.is-visible > * {
  animation: textRise 1s var(--ease-out) backwards;
  animation-delay: calc(var(--reveal-delay, 0) * 1ms + var(--n, 0) * 90ms);
}

@keyframes textRise {
  0% {
    opacity: 0;
    transform: translate3d(0, 30px, 0);
  }
  45% {
    opacity: 1;
  }
  100% {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}

/* ---------- Hero: one orchestrated entrance right after the loader ---------- */
.hero__content.reveal-item,
.hero__content.reveal-item.is-visible {
  opacity: 1;
  transform: none;
  animation: none;
}

.hero__content > * {
  opacity: 0;
}

.site-ready .hero__content > * {
  animation: heroRise 1.3s var(--ease-out) both;
  animation-delay: calc(0.3s + var(--n, 0) * 0.14s);
}

.site-ready .hero__content > h1 {
  animation-name: heroTitle;
  animation-duration: 1.5s;
}

@keyframes heroRise {
  0% {
    opacity: 0;
    transform: translate3d(0, 34px, 0);
  }
  40% {
    opacity: 1;
  }
  100% {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}

@keyframes heroTitle {
  0% {
    opacity: 1;
    transform: translate3d(0, 56px, 0);
    clip-path: inset(100% -10% -20% -10%);
  }
  100% {
    opacity: 1;
    transform: translate3d(0, 0, 0);
    clip-path: inset(-20% -10% -20% -10%);
  }
}

.site-ready .hero__media img {
  animation:
    heroIntro 2.4s var(--ease-out) both,
    heroBreath 16s ease-in-out 2.4s infinite alternate;
}

@keyframes heroIntro {
  from {
    transform: scale(1.22);
    filter: saturate(0.74) contrast(1.04) brightness(0.4);
  }
  to {
    transform: scale(1.015);
    filter: saturate(0.74) contrast(1.04) brightness(0.83);
  }
}

.site-ready .hero__scroll {
  animation: fadeIn 1s ease 1.5s both;
}

.hero__scroll span {
  animation: scrollLine 2.4s cubic-bezier(0.65, 0, 0.35, 1) infinite;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes scrollLine {
  0% {
    transform: scaleY(0);
    transform-origin: top;
  }
  45% {
    transform: scaleY(1);
    transform-origin: top;
  }
  55% {
    transform: scaleY(1);
    transform-origin: bottom;
  }
  100% {
    transform: scaleY(0);
    transform-origin: bottom;
  }
}

/* ---------- Sidebar and mobile header ---------- */
.site-ready .sidebar {
  animation: railIn 1.1s var(--ease-out) 0.2s backwards;
}

.site-ready .sidebar__nav a {
  animation: railIn 0.9s var(--ease-out) backwards;
  animation-delay: calc(0.4s + var(--n, 0) * 0.07s);
}

.site-ready .mobile-header {
  animation: headerDrop 0.9s var(--ease-out) 0.2s backwards;
}

@keyframes railIn {
  from {
    opacity: 0;
    transform: translate3d(var(--rail-from), 0, 0);
  }
  to {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}

@keyframes headerDrop {
  from {
    opacity: 0;
    transform: translate3d(0, -100%, 0);
  }
  to {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}

/* ---------- Modal: image and copy re-animate when switching projects ---------- */
.modal__image-wrap img {
  animation: modalImageIn 0.8s var(--ease-out) backwards;
}

.modal__copy > * {
  animation: textRise 0.8s var(--ease-out) backwards;
  animation-delay: calc(0.12s + var(--n, 0) * 0.07s);
}

@keyframes modalImageIn {
  from {
    opacity: 0;
    transform: scale(1.06);
  }
  to {
    opacity: 1;
    transform: scale(1.012);
  }
}

/* ---------- Modal: no scrollbar flash while it opens ----------
   Children that are translated down during their entrance were
   momentarily enlarging the scrollable area of .modal__content,
   which made a scrollbar appear and vanish. Clipping the copy block
   keeps the entrance animation from affecting scroll overflow. */
.modal__content {
  overflow-x: hidden;
}

.modal__copy {
  overflow: hidden;
}

/* Less jank when opening: freeze heavy background loops behind the blur */
body.modal-open .hero__media img,
body.modal-open .hero__scroll span,
body.modal-open .contact__glow,
body.modal-open .contact__monogram {
  animation-play-state: paused;
}

/* =========================================================
   LIGHTBOX PERFORMANCE + STABLE NAVIGATION
   Keeps project switching visually calm and avoids image
   re-mount / scale jank on lower-powered mobile devices.
========================================================= */
.modal__content {
  overflow-x: hidden;
  overflow-y: auto;
  overscroll-behavior: contain;
  -webkit-overflow-scrolling: touch;
}

.modal__image-wrap {
  contain: paint;
  isolation: isolate;
}

.modal__content img {
  display: block;
  width: 100%;
  height: auto;
  object-fit: cover;
  transform: none !important;
  transition: opacity 0.18s ease !important;
  animation: none !important;
  will-change: opacity;
  backface-visibility: hidden;
}

.modal__content img.is-loading {
  opacity: 0.18;
}

.modal__nav {
  position: fixed;
  top: 50%;
  margin-top: 0;
  transform: translate3d(0, -50%, 0);
  will-change: transform;
  touch-action: manipulation;
}

.modal__nav:hover:not(:disabled) {
  transform: translate3d(0, -50%, 0) scale(1.06);
}

/* A softer overlay costs less on small screens than the original heavy blur. */
@media (max-width: 720px) {
  .modal {
    padding: max(10px, env(safe-area-inset-top))
      max(10px, env(safe-area-inset-right))
      max(10px, env(safe-area-inset-bottom))
      max(10px, env(safe-area-inset-left));
    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);
  }

  .modal__content {
    max-height: calc(100dvh - 20px);
  }

  .modal__nav {
    top: 50%;
    width: 42px;
    height: 42px;
    margin-top: 0;
  }

  .modal__nav--prev {
    left: max(8px, env(safe-area-inset-left));
  }

  .modal__nav--next {
    right: max(8px, env(safe-area-inset-right));
  }
}

/* =========================================================
   LOADER MOBILE UPGRADE
   Warmer easel / softer depth so the stand stays visible
   instead of disappearing into the black background.
========================================================= */
.art-loader {
  background:
    radial-gradient(
      circle at 50% 31%,
      rgba(121, 98, 72, 0.16),
      transparent 34%
    ),
    radial-gradient(
      circle at 50% 72%,
      rgba(83, 103, 119, 0.12),
      transparent 38%
    ),
    linear-gradient(145deg, #151413 0%, #0c0d0e 52%, #08090a 100%);
}

.art-loader__canvas-paper {
  border-color: #766957;
  box-shadow:
    0 28px 65px rgba(0, 0, 0, 0.48),
    0 0 0 1px rgba(236, 222, 199, 0.11),
    0 0 45px rgba(196, 153, 103, 0.06);
}

.art-loader__easel {
  filter: drop-shadow(0 11px 18px rgba(0, 0, 0, 0.28));
}

.art-loader__easel::before {
  content: "";
  position: absolute;
  left: 19%;
  right: 19%;
  bottom: 35%;
  height: 6px;
  border-radius: 999px;
  background: linear-gradient(180deg, #d8c8ab 0%, #9f8c70 55%, #665847 100%);
  box-shadow: 0 3px 7px rgba(0, 0, 0, 0.28);
}

.art-loader__easel::after {
  content: "";
  position: absolute;
  left: 12%;
  right: 12%;
  bottom: -5px;
  height: 16px;
  border-radius: 50%;
  background: radial-gradient(
    ellipse at center,
    rgba(213, 190, 153, 0.22),
    transparent 70%
  );
  filter: blur(5px);
}

.art-loader__easel span,
.art-loader__easel i {
  background: linear-gradient(180deg, #d6c5a8 0%, #a18f74 50%, #665847 100%);
  box-shadow:
    0 0 0 1px rgba(242, 229, 207, 0.08),
    0 2px 8px rgba(0, 0, 0, 0.24);
}

@media (max-width: 720px) {
  .art-loader__scene {
    width: min(640px, 92vw);
    height: min(430px, 55vh);
    margin-top: -18px;
  }

  .art-loader__canvas {
    top: 50px;
    width: min(500px, 88vw);
  }

  .art-loader__canvas-paper {
    border-width: 7px;
  }

  .art-loader__easel {
    bottom: 16px;
    width: 208px;
    height: 76px;
  }
}

@media (max-width: 420px) {
  .art-loader__scene {
    width: 94vw;
    height: 390px;
    margin-top: -8px;
  }

  .art-loader__canvas {
    top: 40px;
    width: 92vw;
  }

  .art-loader__canvas-paper {
    border-width: 6px;
  }

  .art-loader__easel {
    bottom: 18px;
    width: 174px;
    height: 64px;
  }

  .art-loader__easel::before {
    left: 16%;
    right: 16%;
    bottom: 34%;
    height: 5px;
  }

  .art-loader__easel span,
  .art-loader__easel i {
    box-shadow:
      0 0 0 1px rgba(242, 229, 207, 0.07),
      0 2px 7px rgba(0, 0, 0, 0.22);
  }
}
</style>
