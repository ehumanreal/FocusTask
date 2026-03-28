# FocusTask

Aplikacja do zarządzania zadaniami (to-do + produktywność) z logowaniem, listami, priorytetami, tagami, deadline, podzadaniami, udostępnianiem i eksportem.

## Najważniejsze funkcje
- Logowanie i rejestracja użytkowników
- Listy zadań (wiele list na konto)
- Udostępnianie list przez zaproszenia (podgląd / edycja)
- Zadania z priorytetem, tagami i deadline
- Podzadania (checklisty) z edycją nazw i kolejnością drag-and-drop
- Tryb produktywności (Pomodoro)
- Powiadomienia o zbliżających się terminach
- Eksport do CSV i PDF

## Role i uprawnienia
- **Owner**: pełna kontrola nad listą i udostępnianiem
- **Editor**: może edytować zadania i podzadania
- **Viewer**: tylko podgląd, ale może **odznaczać swoje wykonanie** zadań i podzadań (prywatnie)

## Technologie
- Frontend: HTML, CSS, JS
- Backend: Node.js (Express)
- Baza danych: SQLite

## Uruchomienie lokalne
1. Zainstaluj zależności:
   ```bash
   npm install
   ```
2. Uruchom serwer:
   ```bash
   npm start
   ```
3. Wejdź na: `http://localhost:3000`

## Struktura projektu
- `server.js` - API + serwer statyczny
- `db.js` - inicjalizacja bazy i migracje
- `public/index.html` - UI
- `public/styles.css` - style
- `public/app.js` - logika frontu
- `data/` - plik SQLite (tworzy się automatycznie)

## Eksport
- **CSV**: przycisk `Eksport CSV` pobiera plik z aktualnej listy.
- **PDF**: przycisk `Eksport PDF` pobiera gotowy plik z backendu.

### PDF i polskie znaki
PDF korzysta z czcionki systemowej. Jeśli polskie znaki nie wyświetlają się poprawnie:
- ustaw zmienną `PDF_FONT_PATH` na ścieżkę do fontu `.ttf` (np. Arial/DejaVu/Noto), albo
- wgraj czcionkę do `public/fonts/NotoSans-Regular.ttf`.

Przykład (PowerShell):
```powershell
$env:PDF_FONT_PATH="C:\Windows\Fonts\arial.ttf"
npm start
```

## Jak działa udostępnianie
1. Właściciel listy wysyła zaproszenie (rola: podgląd lub edycja).
2. Zaproszony użytkownik akceptuje/odrzuca zaproszenie w panelu `Zaproszenia`.
3. Lista pojawia się u zaproszonego użytkownika.

## Dane prywatne dla zaproszonych
Viewerzy mogą oznaczać wykonanie zadań i podzadań **tylko u siebie**.
Status zapisywany jest per użytkownik i nie zmienia listy u innych.

## Przykładowe API (skrót)
- `POST /api/register`
- `POST /api/login`
- `GET /api/lists`
- `POST /api/lists`
- `POST /api/lists/:id/invites`
- `GET /api/invites`
- `POST /api/invites/:id/accept`
- `POST /api/invites/:id/decline`
- `GET /api/tasks?listId=...`
- `POST /api/tasks`
- `PATCH /api/tasks/:id`
- `POST /api/tasks/:id/subtasks`
- `PATCH /api/subtasks/:id`
- `PATCH /api/tasks/:id/subtasks/reorder`
- `GET /api/export/csv?listId=...`
- `GET /api/export/pdf?listId=...`

## Typowe problemy
- **SQLITE_ERROR: no such column: list_id**
  - Zwykle oznacza starą bazę. Uruchom ponownie serwer (migracje są automatyczne).
  - Jeśli problem zostaje, możesz usunąć `data/app.db` (utrata danych) i uruchomić ponownie.

- **Brak polskich znaków w PDF**
  - Ustaw `PDF_FONT_PATH` lub dodaj font do `public/fonts/`.

## Pomysły na rozbudowę
- Kategorie i podzadania z terminami
- Integracja z kalendarzem
- Wersja mobilna (PWA)
- Historia zmian i archiwum
- Powiadomienia email lub push
