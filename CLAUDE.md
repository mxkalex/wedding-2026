# Wedding Site — Frontend Guidelines

Свадебный сайт Александра и Анастасии, 22.08.2026.
Единый файл `index.html` (HTML + inline CSS + JS), без сборщика, без фреймворков.

---

## Ключевые данные

- **Дата**: 22 августа 2026, 15:30 (сбор гостей) / 16:00 (церемония)
- **Место**: Зелёная усадьба, Чувашская Республика, д. Кадикасы, ул. Пушкина, 33
- **Организатор**: Алина (+7 952 312-25-05, @alinalukiyanova)
- **Telegram-чат**: https://t.me/+T_9RkXi6uXw5MDky
- **Жених**: @mxkalex · **Невеста**: @mak_nast
- **RSVP дедлайн**: 22 июля 2026

---

## Файлы проекта

```
project 1/
├── index.html                        # весь сайт (единый файл, ~3760 строк)
├── first-photo.png                   # фото пары (hero, портретное 759×1139px)
├── venue.jpg                         # фото площадки
├── CLAUDE.md                         # этот файл
├── dresscode-background.jpg          # blur-фон секции дресс-кода
├── program-background.jpg            # blur-фон секции «План дня»
├── gifts-background.jpg              # blur-фон секции «Вишлист/Подарки»
├── maps_logo/                        # логотипы сервисов карт
│   ├── 2gis.webp
│   ├── Google_Maps_Logo_2020.svg.png
│   └── Yandex_Maps_icon.svg.png
├── music_back_logo/                  # фоны секции музыки (по артисту/треку)
│   ├── sqwzbab.jpg                   # SQWOZ BAB
│   ├── tokyo.webp                    # Tokyo Drift
│   ├── monetochka.jpeg               # Монеточка
│   ├── russkaya-dusha.jpg            # РУССКАЯ ДУША
│   ├── DDT.png                       # ДДТ
│   ├── korol-i-shut.jpg              # КиШ
│   ├── familia.png                   # Дайте Танк!
│   └── PLC.jpg                       # PLC (Вайб пары)
├── dresscode/                        # визуалы для секции дресс-кода
│   ├── 1x1-female-olive.png
│   ├── 1x1-female-pink.png
│   ├── 1x1-male-powder.png
│   ├── 1x1-beige.png
│   ├── 1x1-green.png
│   ├── 1x1-sand.png
│   ├── female-outfit-1.png           # карусель
│   ├── female-outfit-2.png
│   ├── male-outfit-1.png
│   └── male-outfit-2.png
└── playlist/
    ├── avatars/                      # фото для мини-плеера
    │   ├── wifey.jpg                 # аватар невесты
    │   ├── husband.jpg               # аватар жениха
    │   └── ourvibe.jpg               # аватар пары
    ├── bride/                        # 5 треков (.m4a) — SQWOZ BAB, Монеточка и др.
    │   ├── sqwoz-bab-baza.m4a
    │   ├── sqwoz-bab-tokyo.m4a
    │   ├── tokyo-drift.m4a
    │   ├── monetochka-kis-kis-kis.m4a
    │   └── russkaya-dusha.m4a
    ├── couple/                       # трек(и) пары
    │   └── plc-navylet.mp3           # Вайб пары
    └── groom/                        # 5 треков (.m4a) — Дайте Танк!, ДДТ, КиШ
        ├── dayte-tank-familia.m4a
        ├── ddt-vashi-veshi.m4a
        ├── korol-i-shut-na-krayu.m4a
        ├── korol-i-shut-panika.m4a
        └── korol-i-shut-schaste.m4a
```

---

## Структура страницы (секции в порядке)

| ID | Название | Фон |
|----|----------|-----|
| splash | Splash overlay: «Дорогой друг!» → fade-out через 3.5с | cream white |
| `#hero` | «Приглашаем вас на свадьбу Александра & Анастасии» | gradient green |
| `#music` | «Включи свой вайб» — аудиоплеер (три плейлиста) | beige-pale + blur фон артиста |
| `#details` | Дата, время, место, карты (логотипы 2GIS/Яндекс/Google) | white |
| `#program` | Таймлайн программы дня | gradient green + blur фото (`program-background.jpg`) |
| `#dresscode` | Дресс-код | beige-pale + blur фото (`dresscode-background.jpg`) |
| `#gifts` | Подарки (только конверт) | white + blur фото (`gifts-background.jpg`) |
| `#rsvp` | Анкета гостя | pink-light→beige-pale |
| `#contacts` | Контакты + Telegram-чат | green |
| `footer` | Обратный отсчёт + «Сохранить в календарь» + навигация | #0d2a00 |

---

## Реализованные компоненты

### Splash-экран
Полноэкранный overlay при загрузке сайта: «Дорогой друг! Мы к тебе с приглашением!»
- **CSS**: `#splash` (position fixed, z-index 9999), `.splash-text`, `.splash-loader`
- **Анимация**: текст fadeInUp → пульсирующие точки → fade-out через 3.5с → удаление из DOM
- **JS IIFE**: блокирует скролл (`overflow: hidden`) на время splash

### Hero-фото
- **Фото**: `first-photo.png` — портретное (759×1139px), отображается вертикально полностью
- **CSS**: `.hero-photo-wrap` max-width 480px, `.hero-photo` height: auto (no crop), border-radius 20px
- **Fallback**: `.hero-initials` (инициалы А&А) — скрыт через `display: none`

### Карусель дресс-кода (`#dresscode`)
Десктоп: 5 слайдов (правила → 4 фото). Мобильный (<=768px): 6 слайдов (Дамы → Джентльмены → 4 фото).
- **Навигация**: только кнопки `‹ ›` и точки-индикаторы (свайп/drag **убраны**)
- **Мобильный split**: JS динамически разделяет слайд правил на два (Дамы / Джентльмены) и добавляет 6-ю точку
- JS IIFE: `#dcTrack`, `#dcPrev`, `#dcNext`, `#dcDots`
- Все 4 фото-слайда заполнены из `dresscode/{female,male}-outfit-{1,2}.png`
- **Фон секции**: `.dc-bg` — `dresscode-background.jpg` в blur 40px, opacity 0.25
- **Кнопки**: glassmorphism 44×44px, spring-анимация hover, pill-dot индикаторы (width transition)

### Попап цветовой палитры (`.swatches`)
- Desktop: hover на `.swatch-c` → попап рядом со свотчем
- Mobile: tap → модал с затемнением, tap мимо → закрывается (350ms debounce защита от мгновенного закрытия)
- JS IIFE: `#swatchPopup`, `#spOverlay`, `#spImg`, `#spPlaceholder`, `#spCaption`
- Все `data-ref` заполнены из `dresscode/1x1-*.png`

### RSVP форма (`#rsvp`)
- Поля: имя, присутствие (да/нет), транспорт, напитки, комментарий
- **Попутчики удалены** — поля и JS убраны полностью
- **Напитки**: сетка 2 колонки, кнопка «Другое» на всю ширину (`grid-column: 1 / -1`), placeholder: «Кстати, будет самогон, но вы не стесняйтесь, пишите»
- Валидация на клиенте, success-state без перезагрузки
- **Бэкенд**: Google Apps Script Web App -> Google Sheets + Telegram-уведомление жениху (файл `apps-script.js`)

### Аудиоплеер (`#music`)
Кастомный HTML5 аудиоплеер, три плейлиста (невеста/жених/пара), переключение табами.
- **Архитектура**: один `Audio` объект, три состояния (`pState.bride` / `pState.groom` / `pState.couple`), при клике на таб плейлист автоматически начинает играть
- **CSS**: все классы с префиксом `ap-` (audio player): `.ap-panel`, `.ap-controls`, `.ap-btn--play/prev/next`, `.ap-progress-bar`, `.ap-tracklist`, `.ap-track`, `.ap-eq` (анимированный эквалайзер)
- **JS IIFE**: `PLAYLISTS` объект с массивами `{ title, src }`, `panels` DOM-кэш, функции `loadTrack()`, `activateTab()`, `seek()`
- **Файлы**: `playlist/bride/*.m4a` (5 шт.), `playlist/groom/*.m4a` (5 шт.), `playlist/couple/plc-navylet.mp3` (1 шт.)
- **Пути латиницей** (переименовано), `encodePath()` всё ещё используется для совместимости
- **A11y**: WAI-ARIA tablist, `role="slider"` на progress bar с `tabindex="0"` и keyboard seek (±5сек), `aria-live="polite"` на track title, `focus-visible` на всех кнопках, `aria-valuetext` на слайдере
- **Фон секции**: `.music-bg` (#musicBg) — по умолчанию `ourvibe.jpg` (фото пары) в blur 45px, opacity 0.3. При воспроизведении трека заменяется на фото артиста/группы через `TRACK_BG` маппинг в JS
- **Play-кнопка**: gradient + pulse ring анимация (`@keyframes playPulse`) при воспроизведении (класс `.playing`)
- Чтобы добавить/убрать треки: отредактировать массив `PLAYLISTS` и `TRACK_BG` в JS IIFE "Music audio player"

### Мини-плеер (sticky bottom bar)
Полупрозрачная фиксированная панель внизу экрана, появляется при воспроизведении.
- **CSS**: все классы с префиксом `mp-` (mini player): `.mp-bar`, `.mp-avatar`, `.mp-info`, `.mp-controls`, `.mp-btn`, `.mp-volume`
- **Фон**: по умолчанию `rgba(13,42,0,0.88)` (зелёный/жених) + `backdrop-filter: blur(20px)`, z-index 990
- **Цветовые темы**: `.mp-theme-bride` (нежный розовый `rgba(180,70,90,0.88)`), без класса (зелёный/жених, дефолт), `.mp-theme-couple` (белый/кремовый с тёмным текстом). Применяются через `applyMpTheme(key)`, плавный transition 0.5s
- **Слева**: аватар-кнопка с фото (`wifey.jpg` / `husband.jpg` / `ourvibe.jpg`), клик циклит плейлисты: bride → groom → couple → bride
- **Центр**: название трека (`aria-live="polite"`) + тонкий progress bar (click + touch drag seeking)
- **Справа**: prev/play/next + volume slider + mute toggle + close (×)
- **Логика dismiss**: кнопка close ставит `mpDismissed=true` и скрывает бар; автосмена трека (`ended`) не возвращает его; только явное взаимодействие с play/плеером сбрасывает dismiss
- **`#scrollTop`** автоматически поднимается на 60px через CSS-selector `.mp-bar.visible ~ #scrollTop`

### Кнопка «Сохранить в календарь» (footer)
- `.cal-btn` — gradient shimmer, скачивает `.ics` файл (vCalendar)
- Событие: 22.08.2026, 15:30–23:59 MSK, место + ссылка на сайт
- VALARM за 1 день до свадьбы
- JS: Blob + URL.createObjectURL + скрытый `<a>` download

### Вишлист (`#gifts`)
- Карточки жениха и невесты **удалены**
- Остался только блок «Денежный конверт»
- **Фон**: `.gifts-bg` — `gifts-background.jpg` в blur 40px, opacity 0.2

### «Как добраться» (`#details`)
- Кнопки 2GIS / Яндекс.Карты / Google Maps с **логотипами** (`.map-btn-logo` 18×18px из `maps_logo/`)

### Обратный отсчёт (footer)
- Считает до 22.08.2026 15:30 **MSK** (`+03:00`)
- Дни / часы / минуты / секунды

---

## Дизайн кнопок (обновлён 2026-03-28)

Все кнопки прошли полный редизайн:
- **CTA (.btn)**: gradient backgrounds, shimmer hover (`::after translateX`), spring animation (`cubic-bezier(0.34, 1.56, 0.64, 1)`), colored box-shadows, `isolation: isolate`
- **Варианты**: `.btn-green` (gradient green), `.btn-outline` (border), `.btn-pink` (gradient pink), `.btn-tg` (Telegram blue)
- **Карусель (.dc-nav-btn)**: glassmorphism `backdrop-filter: blur(12px)`, 44×44px, spring hover + scale
- **Карусель dots (.dc-dot)**: pill-shape, active = width 28px (ease-out)
- **Audio tabs (.ap-tab)**: glass effect + gradient active state
- **Play (.ap-btn--play)**: gradient + pulse ring `@keyframes playPulse` при воспроизведении
- **RSVP submit (.btn-submit)**: gradient slide (`background-position`), shimmer отключён
- **Calendar (.cal-btn)**: gradient shimmer
- **Scroll-to-top (#scrollTop)**: gradient + spring hover
- **`prefers-reduced-motion`**: глобальный блок для accessibility

---

## Aesthetic Direction: Luxury / Refined

**Тон**: тёплый, элегантный, романтичный. Не пышный — уточнённый.
**Запоминающийся элемент**: тёмно-зелёный (#184A00) + нежный розовый (#FF9C9C) на кремово-белом (#FFFAF6).

---

## Цвета (строго)

```
--pink:       #FF9C9C  // акцент, CTA, иконки
--pink-dark:  #e87878  // hover на акцентах
--pink-light: #fff0f0  // фоны секций
--green:      #184A00  // основной тёмный, текст, фон CTA
--green-light:#2a6b00  // hover на зелёном
--beige:      #DEB192  // тёплый акцент, линии, рамки
--beige-pale: #fdf6ee  // фон карточек
--white:      #FFFAF6  // основной фон
--text:       #2c2420  // основной текст
--text-light: #6b5a52  // вспомогательный текст
```

**Запрещено**: чистый белый (#fff), чистый чёрный (#000), фиолетовые градиенты.

---

## Типографика

- **Заголовки**: `Cormorant Garamond` — serif, курсив для имён и подзаголовков.
- **Тело**: `Lato` 300/400/700.
- **Запрещено**: Inter, Roboto, Arial, system fonts.
- Размеры через `clamp()`, минимум 3× разрыв между body и h1.

---

## Motion & Hover

- Hover на карточках: spring cubic-bezier `(0.34, 1.45, 0.64, 1)` для transform.
- Кнопки: spring `(0.34, 1.56, 0.64, 1)` + shimmer `::after`.
- Анимации появления: `IntersectionObserver` + CSS `opacity/translateY`, каскадные `animation-delay`.
- Одна срежиссированная анимация при загрузке лучше пяти случайных.
- `backdrop-filter: blur()` на картах поверх фона — глубина без тяжести.
- `@media (prefers-reduced-motion: reduce)` — все анимации/transitions урезаны.

---

## Компоненты — правила

| Элемент | Правило |
|---------|---------|
| Кнопки | `border-radius: 50px`, uppercase, `letter-spacing: 0.11em`, gradient + shimmer |
| Карточки | `border-radius: 16px`, `box-shadow` мягкая зелёная |
| Формы | `border-radius: 10px`, `accent-color: var(--green)` |
| Секции | чередовать белый / beige-pale / green / pink-light, blur-фон фото |

---

## Адаптивность

- Mobile-first. Breakpoints: `600px`, `768px`.
- На мобильном: `grid-template-columns: 1fr`, padding снижается.

---

## Head: meta-теги

```html
<meta name="theme-color" content="#184A00">
<meta property="og:type" content="website">
<meta property="og:title" content="Свадьба Александра & Анастасии · 22.08.2026">
<meta property="og:description" content="Приглашаем на свадьбу! 22 августа 2026, Зелёная усадьба, Чувашская Республика">
<meta property="og:image" content="https://mxkalex.github.io/wedding-2026/first-photo.png">
<meta property="og:url" content="https://mxkalex.github.io/wedding-2026/">
<meta property="og:locale" content="ru_RU">
<link rel="icon" href="data:image/svg+xml,..."> <!-- эмодзи 💍 -->
```

---

## Что ещё не сделано / план

- [x] **Добавить фото** в карусель дресс-кода — готово (`dresscode/*.png`)
- [x] **Добавить фото** в попапы палитры — готово (`dresscode/1x1-*.png`)
- [x] **Подключить RSVP** — Google Apps Script -> Google Sheets + Telegram-уведомление (apps-script.js)
- [x] **Публикация** — задеплоено на GitHub Pages
- [x] **OG meta tags** — Telegram/WhatsApp/VK превью
- [x] **Favicon** — inline SVG с 💍
- [x] **Theme-color** — #184A00 для мобильных браузеров
- [x] **Countdown timezone** — +03:00 MSK
- [x] **Кнопка «Сохранить в календарь»** — .ics скачивание
- [x] **Fix couple playlist** — кнопки play/prev/next для «Вайб пары»
- [x] **Hero photo** — портретный формат, полное отображение
- [x] **Редизайн кнопок** — gradient, shimmer, spring, glassmorphism, pulse ring
- [x] **Blur-фоны секций** — dresscode, program, gifts (фото на весь фон)
- [x] **Логотипы карт** — 2GIS/Яндекс/Google вместо эмодзи
- [x] **«Другое» в напитках** — на всю ширину + юмористический placeholder

### Идеи для будущих секций (из research)
- **«Наша история»** — вертикальный таймлайн с фото: знакомство → пара → предложение
- **Фотогалерея** — masonry grid или слайдер с фото пары
- **Встроенная карта** — Yandex Maps embed в `#details`
- **FAQ** — аккордеон с ответами на частые вопросы гостей
- **Запрос трека** — поле в RSVP: «Какую песню хотите услышать?»
- **Размещение** — карточки ближайших гостиниц (для иногородних)

---

## Деплой

- **Репозиторий**: https://github.com/mxkalex/wedding-2026
- **Хостинг**: GitHub Pages с ветки `main`
- **URL**: https://mxkalex.github.io/wedding-2026/
- **Деплой**: `git push origin main` → автоматическое обновление через 1–2 мин

---

## Frontend Design Ultimate — запреты

- Не использовать "AI slop": фиолетовые градиенты, Space Grotesk, одинаковые сетки карточек.
- Каждый раздел — свой визуальный характер.
- Секции не оставлять на сплошном цвете без атмосферы (фактура, градиент, фото).
- Анимации — намеренные, не декоративный шум.
