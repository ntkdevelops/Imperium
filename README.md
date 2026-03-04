<p align="center">
  <img src="https://i.imgur.com/JpFF3n1.png" alt="Imperium Logo" width="120">
</p>

<h1 align="center">Imperium</h1>

<p align="center">
  <strong>Build your empire of ideas.</strong>
</p>

<p align="center">
  A pet project remake of <a href="https://notion.so">Notion</a> built from scratch to work on my development skills.<br>
  No frameworks. No libraries. Just vanilla HTML, CSS, and JavaScript.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5">
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3">
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript">
  <img src="https://img.shields.io/badge/Storage-localStorage-orange?style=for-the-badge" alt="localStorage">
</p>

---

## 🎯 Why I Built This

I wanted to challenge myself by rebuilding a complex, real-world productivity app without reaching for any framework. Notion's UI is packed with subtle interactions — contenteditable blocks, drag-and-drop, nested page trees, kanban boards — and recreating them from scratch taught me a ton about DOM manipulation, state management, and event-driven architecture.

This is **not** meant to replace Notion. It's a learning exercise, a skill sharpener, and a portfolio piece. 🏋️

---

## ✨ Features

### 📄 Block-Based Editor
- Rich text editing with `contenteditable` — **bold**, *italic*, <u>underline</u>, ~~strikethrough~~, `inline code`, links
- **11 block types:** paragraph, headings (H1–H3), bullet list, numbered list, to-do, code, quote, divider, callout
- ⚡ Slash command menu (`/`) with fuzzy filtering and keyboard navigation
- ✍️ Markdown shortcuts — type `# `, `- `, `[] `, `> `, `` ``` ``, `---` + space to auto-convert
- 📐 Block indentation (Tab / Shift+Tab, up to 6 levels)
- ✂️ Split & merge blocks with Enter and Backspace
- 🖊️ Floating toolbar appears on text selection

### 📋 Kanban Boards
- 🔀 Toggle any page between document and board view
- 🖱️ Drag-and-drop cards between columns
- 🏷️ Card details modal — title, description, priority, due date, color-coded labels
- 🎨 Column management — rename, recolor (8 colors), delete

### 🗂️ Sidebar & Navigation
- 🌳 Recursive page tree with infinite nesting
- 📂 Expand/collapse child pages
- ⭐ Favorites section for quick access
- 🔍 Full-text search across page titles and block content
- 📋 Right-click context menu — rename, duplicate, favorite, add sub-page, delete
- ↔️ Resizable sidebar (drag handle, 200–480px)
- 🗑️ Soft-delete with trash and restore

### 🌗 Theming
- ☀️ Light and 🌙 dark mode with smooth Notion-style neutral palettes
- 💾 Persisted preference across sessions

### ⌨️ Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + N` | 📄 New page |
| `Ctrl + P` | 🔍 Focus search |
| `Ctrl + /` | 📂 Toggle sidebar |
| `Ctrl + Shift + D` | 🌗 Toggle dark mode |
| `Ctrl + B / I / U` | ✏️ Bold / Italic / Underline |
| `Ctrl + E` | 💻 Inline code |
| `Ctrl + K` | 🔗 Insert link |
| `Ctrl + D` | 📋 Duplicate block |
| `Ctrl + Shift + ↑/↓` | 🔀 Move block up/down |
| `Tab / Shift+Tab` | 📐 Indent / Outdent |
| `/` | ⚡ Open slash command menu |

### 💾 Persistence
- All data lives in `localStorage` — nothing is sent to any server
- Instant save on every edit (debounced at 300ms)
- Survives page refreshes and browser restarts

---

## 🏗️ Architecture

Zero dependencies. ~2,500 lines of JavaScript organized into **8 focused modules**:

```
imperium/
├── 🏠 index.html              SPA shell — sidebar, main content, overlays
├── 🎨 css/
│   └── styles.css              1,350+ lines — CSS variables, light/dark themes, all components
└── ⚙️ js/
    ├── utils.js                🔧 UUID generation, debounce, date formatting, caret helpers, toasts
    ├── store.js                💾 localStorage CRUD — pages, blocks, boards, cards, settings, trash
    ├── app.js                  🚀 Bootstrap, hash routing (#page/id), theme, global shortcuts
    ├── sidebar.js              🗂️  Page tree, search, favorites, trash, context menu, resize
    ├── editor.js               ✏️  Block editor — contenteditable, key handling, inline formatting
    ├── commands.js             ⚡ Slash command menu with filtering and keyboard nav
    ├── kanban.js               📋 Board view — columns, cards, detail modal
    └── dragdrop.js             🖱️  Drag-and-drop for cards between columns & block reorder
```

### 📊 Data Model

All state is stored under the `notion_data` key in localStorage as a single JSON object:

```
┌─────────────────────────────────────────────────┐
│  📦 notion_data                                 │
│                                                 │
│  ┌───────────┐        ┌───────────┐             │
│  │ 📄 pages  │───────▶│ 📝 blocks │             │
│  │   (tree)  │        │   (flat)  │             │
│  └───────────┘        └───────────┘             │
│                                                 │
│  ┌───────────┐        ┌───────────┐             │
│  │ 📋 boards │───────▶│ 🃏 cards  │             │
│  │  (kanban) │        │   (flat)  │             │
│  └───────────┘        └───────────┘             │
│                                                 │
│  ┌───────────┐        ┌───────────┐             │
│  │ ⚙️ settings│        │ 🗑️ trash  │             │
│  └───────────┘        └───────────┘             │
└─────────────────────────────────────────────────┘
```

| Entity | Description |
|---|---|
| **Pages** | Flat map keyed by ID. Parent/child references form a tree. |
| **Blocks** | Flat map keyed by ID, each belonging to a page. 11 types with type-specific properties. |
| **Boards** | One per kanban page. Contains ordered columns, each with ordered card IDs. |
| **Cards** | Flat map keyed by ID. Title, description, labels, priority, due date. |
| **Settings** | Theme, sidebar width, collapsed state, last opened page. |
| **Trash** | Soft-deleted pages and blocks — fully restorable. |

### 🔄 How Modules Talk

```
  ┌─────────┐        navigateTo()       ┌──────────┐
  │ Sidebar │ ──────────────────────────▶│   App    │  🚀 Router + coordinator
  └─────────┘                            └──────────┘
       │                                   │      │
       │ renderTree()          ┌───────────┘      │
       │                       ▼                  ▼
       │               ┌──────────┐       ┌──────────┐
       └──────────────▶│  Editor  │       │  Kanban  │
                       └──────────┘       └──────────┘
                           │                   │
                    ┌──────┘                   │
                    ▼                          ▼
              ┌──────────┐             ┌──────────┐
              │ Commands │             │ DragDrop │
              └──────────┘             └──────────┘
                    │                       │
                    └───────┐   ┌───────────┘
                            ▼   ▼
                       ┌──────────┐
                       │  Store   │ ◀──── 💾 Single source of truth
                       └──────────┘
                            │
                            ▼
                      localStorage
```

All modules read/write through **Store**, which handles serialization to `localStorage`. **App** acts as the router and coordinator — **Sidebar** triggers navigation, and App decides whether to render the **Editor** or **Kanban** view based on page type.

---

## 🚀 Getting Started

### Run Locally

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/imperium.git
cd imperium

# Install deps (only express for the dev server)
npm install

# Start the server
node server.js

# Open in browser 🌐
# http://localhost:3000/notion
```

### Or Just Open the File

Since Imperium is a pure client-side app, you can open `notion/index.html` directly in your browser — everything works without a server. 🎉

---

## 📸 Screenshots

> 🖼️ _Coming soon — screenshots of the editor, kanban board, dark mode, slash commands, and more._

---

## 🧠 What I Learned

- 🔤 How `contenteditable` really works — caret manipulation, range APIs, `execCommand`, and all its quirks
- 🧱 Building a block-based editor from scratch (splitting/merging blocks, indent management)
- 🖱️ Implementing drag-and-drop without any library
- 🗄️ Designing a flat data model with relational references that serializes cleanly to JSON
- 🎨 CSS custom properties for theming and structuring a large stylesheet
- 🧭 Hash-based SPA routing without a framework
- ⏱️ The importance of debouncing when dealing with frequent DOM events

---

## 🛣️ Roadmap

- [ ] 🤝 Real-time collaboration (WebSocket sync)
- [ ] 📤 Export to Markdown / PDF
- [ ] 🖼️ Image and file block types
- [ ] 📊 Database / table view
- [ ] ↩️ Undo/redo history stack
- [ ] 📱 Mobile-responsive layout
- [ ] 🗃️ IndexedDB for larger data sets
- [ ] 📲 PWA support (offline, installable)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<p align="center">
  <img src="https://i.imgur.com/JpFF3n1.png" alt="Imperium" width="40">
  <br>
  <em>Built with ☕ and curiosity — no frameworks were harmed in the making of this project.</em>
</p>
