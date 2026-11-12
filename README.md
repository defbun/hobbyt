# Хоббиты — найдите хобби по душе

Интерактивный путеводитель по творческим мастерским Москвы: тафтинг, рисование и гончарное дело. 30 мастерских, карта, фильтры, подбор хобби и переход в Telegram [@hobbyskrolling](https://t.me/hobbyskrolling).

Стек: React 19 + Vite 7 + Tailwind CSS 4 + Leaflet (OpenStreetMap).

---

## 🚀 Как выложить на GitHub и Vercel (без ошибки «too many files»)

Ошибка **«Yowza, that's a lot of files. Try uploading fewer than 100 at a time»** появляется,
когда вы загружаете в GitHub папку `node_modules` — в ней десятки тысяч файлов.
**Её загружать НЕ нужно.** Vercel сам установит все зависимости при сборке.

### Шаг 1. Подготовьте чистую папку

В скачанном проекте удалите (или просто не выбирайте при загрузке):

- ❌ `node_modules/` — папка с зависимостями (тысячи файлов, **главная причина ошибки**)
- ❌ `dist/` — старая сборка
- ❌ `.DS_Store`, `Thumbs.db` и прочие системные файлы

### Шаг 2. Загрузите в GitHub ТОЛЬКО эти 16 файлов

Весь проект — это меньше 20 файлов, лимит GitHub в 100 файлов не превышается:

```
.gitignore
index.html
package.json
tsconfig.json
vite.config.ts
vercel.json
README.md
src/main.tsx
src/App.tsx
src/index.css
src/vite-env.d.ts
src/data/places.ts
src/data/siteImages.ts
src/components/HobbyMap.tsx
src/components/VenueDrawer.tsx
src/components/HobbyQuiz.tsx
src/utils/cn.ts
src/utils/clipboard.ts
```

Как загрузить:

1. Откройте свой репозиторий на github.com → **Add file → Upload files**.
2. Перетащите файлы из списка выше **с сохранением структуры папок**:
   сначала корневые файлы, затем отдельно содержимое `src/`, `src/data/`,
   `src/components/`, `src/utils/` (GitHub веб-загрузка сохраняет папки,
   если перетаскивать саму папку `src` целиком — она содержит всего 11 файлов).
3. Нажмите **Commit changes**.

💡 Проще всего: перетащить в окно загрузки **корневые файлы + папку `src` целиком**
(без `node_modules` и `dist`). Это ~18 файлов — GitHub примет без проблем.

### Шаг 3. Подключите Vercel

1. На [vercel.com](https://vercel.com) → **Add New → Project** → выберите репозиторий.
2. Vercel сам определит Vite (настройки уже в `vercel.json`):
   - Build Command: `npm run build`
   - Output Directory: `dist`
3. Нажмите **Deploy**. Через ~1 минуту сайт готов.

Каждый следующий коммит в GitHub будет автоматически обновлять сайт на Vercel.

---

## 💻 Локальный запуск

```bash
npm install     # установить зависимости (создаст node_modules локально)
npm run dev     # запустить на http://localhost:5173
npm run build   # собрать в dist/
```

Если появляется ошибка `Failed to resolve import "../assets/images/..."` —
у вас старая версия `src/data/places.ts`. Замените её актуальной из этого проекта:
первая строка должна быть `import { SITE_IMAGES } from './siteImages';`.

---

## 🖼 Как устроены изображения

- **3 заглавных фото и 3 фото раздела «Три способа сделать своё»** вшиты прямо
  в код (`src/data/siteImages.ts`, base64) — грузятся всегда, без внешних серверов.
- **Фото 30 мастерских** подгружаются по ссылкам из Google-документа
  (`lh7-rt.googleusercontent.com`) c `referrerPolicy="no-referrer"`.
  Если ссылка недоступна — автоматически показывается фото направления из кода.
- **Карта** — тайлы OpenStreetMap, маршруты открываются в Яндекс Картах.
