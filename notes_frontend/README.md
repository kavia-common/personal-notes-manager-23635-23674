# Ocean Notes Frontend (SvelteKit)

A modern notes manager UI using the Ocean Professional theme (blue/amber accents, subtle gradients, rounded corners, soft shadows).

Features:
- Create, view, edit, delete notes
- Search notes by title or content
- Modal editor with accent color
- Mock API fallback via localStorage
- Ready to connect to a backend via REST

Getting started:
1) Install dependencies
   npm install
2) Run dev server
   npm run dev

Environment:
- VITE_API_BASE: Optional. Base URL for your backend (e.g. https://api.example.com).
  If not set, the app uses a mock in-memory/localStorage API.

Integration:
- API functions in src/lib/api.ts:
  - listNotes(q?), getNote(id), createNote(input), updateNote(id, input), deleteNote(id)
- To point to backend, set VITE_API_BASE and ensure the following REST endpoints exist:
  GET    {VITE_API_BASE}/api/notes?q=...
  GET    {VITE_API_BASE}/api/notes/:id
  POST   {VITE_API_BASE}/api/notes
  PUT    {VITE_API_BASE}/api/notes/:id
  DELETE {VITE_API_BASE}/api/notes/:id

Styling:
- Global theme styles in src/app.css
- Components in src/lib/components
