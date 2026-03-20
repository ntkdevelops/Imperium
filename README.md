<h1 align="center">Imperium</h1>

<p align="center">
  <strong>Build your empire of ideas.</strong>
</p>

<p align="center">
  A personal project inspired by <a href="https://notion.so">Notion</a>, created to deepen my understanding of modern front-end architecture.<br>
  Built entirely with vanilla HTML, CSS, and JavaScript — no frameworks, no external libraries.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5">
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3">
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript">
  <img src="https://img.shields.io/badge/Storage-localStorage-orange?style=for-the-badge" alt="localStorage">
</p>

---

## Overview

Imperium is a browser-based productivity application designed as a technical exercise in recreating a complex interface similar to Notion. The goal of the project was to implement advanced UI patterns — block-based editing, drag-and-drop interactions, hierarchical navigation, and dynamic views — using only native web technologies.

The application runs entirely on the client and stores all data locally. It is intended as a learning project and portfolio demonstration rather than a production alternative to Notion.

---

## Features

### Block-Based Editor

- Rich text editing using `contenteditable`
- Inline formatting: bold, italic, underline, strikethrough, inline code, and links
- Eleven block types:
  - Paragraph
  - Headings (H1–H3)
  - Bullet list
  - Numbered list
  - To-do
  - Code block
  - Quote
  - Divider
  - Callout
- Slash command menu with keyboard navigation and fuzzy filtering
- Markdown-style shortcuts (`#`, `-`, `>`, code fences, etc.)
- Block indentation with Tab / Shift+Tab (up to six levels)
- Block splitting and merging via Enter and Backspace
- Floating formatting toolbar on text selection

### Kanban Boards

- Toggle between document and board view
- Drag-and-drop cards across columns
- Card detail modal with description, labels, priority, and due date
- Column management including rename, color selection, and deletion

### Sidebar and Navigation

- Hierarchical page tree with unlimited nesting
- Expand and collapse child pages
- Favorites section for quick access
- Full-text search across page titles and content
- Context menu actions: rename, duplicate, favorite, create sub-page, delete
- Resizable sidebar
- Soft-delete system with restore from trash

### Theming

- Light and dark interface themes
- Theme preference persisted between sessions

### Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + N` | Create new page |
| `Ctrl + P` | Focus search |
| `Ctrl + /` | Toggle sidebar |
| `Ctrl + Shift + D` | Toggle dark mode |
| `Ctrl + B / I / U` | Text formatting |
| `Ctrl + E` | Inline code |
| `Ctrl + K` | Insert link |
| `Ctrl + D` | Duplicate block |
| `Ctrl + Shift + ↑/↓` | Move block |
| `Tab / Shift+Tab` | Indent / outdent |
| `/` | Open slash command menu |

### Data Persistence

- All data stored locally using `localStorage`
- Automatic save on edits with a debounced update cycle
- Data persists across page reloads and browser restarts

---

## Architecture

The application follows a modular architecture and avoids external dependencies. Approximately 2,500 lines of JavaScript are organized into eight focused modules.

```
imperium/
├── index.html
├── css/
│   └── styles.css
└── js/
    ├── utils.js
    ├── store.js
    ├── app.js
    ├── sidebar.js
    ├── editor.js
    ├── commands.js
    ├── kanban.js
    └── dragdrop.js
```

### Module Responsibilities

| Module | Responsibility |
|---|---|
| utils.js | Helper utilities such as UUID generation, debounce functions, caret utilities, and notifications |
| store.js | Data persistence layer managing `localStorage` serialization |
| app.js | Application bootstrap, routing, theme handling, and global keyboard shortcuts |
| sidebar.js | Page tree rendering, search, favorites, and context menus |
| editor.js | Block editor logic and text editing interactions |
| commands.js | Slash command system with filtering and keyboard navigation |
| kanban.js | Board view including columns, cards, and modal editing |
| dragdrop.js | Drag-and-drop behavior for cards and block ordering |

---

## Data Model

All application state is stored under a single `localStorage` key (`notion_data`) as a serialized JSON object.

```
notion_data
├── pages
├── blocks
├── boards
├── cards
├── settings
└── trash
```

| Entity | Description |
|---|---|
| Pages | Flat object keyed by ID; parent references form the page hierarchy |
| Blocks | Individual editor blocks associated with pages |
| Boards | Kanban board configuration for pages using board view |
| Cards | Card objects belonging to board columns |
| Settings | UI preferences such as theme and sidebar width |
| Trash | Soft-deleted items available for restoration |

The Store module acts as the single source of truth, coordinating read and write operations with `localStorage`.

---

## Application Flow

```
Sidebar → App Router → Editor or Kanban View
                     ↓
                  Store
                     ↓
                localStorage
```

Navigation events originate in the sidebar and are routed through the App module. Based on page configuration, either the editor or the kanban interface is rendered. All state changes pass through the Store module.

---

## Running the Project

### Local Development

```bash
git clone https://github.com/YOUR_USERNAME/imperium.git
cd imperium
npm install
node server.js
```

Open in a browser:

```
http://localhost:3000/notion
```

### Direct File Access

Because Imperium is entirely client-side, the application can also be opened directly by loading `notion/index.html` in a browser.

---

## Learning Outcomes

This project served as an exploration of several front-end engineering topics:

- Implementing a block-based text editor using `contenteditable`
- Caret and selection manipulation through the Range API
- Building drag-and-drop interfaces without external libraries
- Designing a normalized data model suitable for JSON serialization
- Implementing SPA-style routing without frameworks
- Managing frequent DOM updates with debounced persistence
- Structuring a large CSS codebase using variables and component patterns

---

## Future Improvements

- Real-time collaboration using WebSockets
- Markdown and PDF export
- Image and file block support
- Table and database-style views
- Undo and redo history
- Responsive mobile layout
- Migration from `localStorage` to IndexedDB
- Progressive Web App support

---

## License

This project is released under the MIT License.

---

<p align="center">
  <img src="https://i.imgur.com/JpFF3n1.png" alt="Imperium" width="40">
  <br>
  <em>Built as an educational project focused on understanding complex front-end systems without frameworks.</em>
</p>
