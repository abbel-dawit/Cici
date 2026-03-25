# 💌 CiCi — Love Letters

> Send heartfelt letters to the people you love. Beautiful, shareable, and free.

**Live site:** https://abbel-dawit.github.io/Cici

---

## ✨ Features

- Write and send letters with a beautiful envelope UI
- Shareable links — recipients get a unique URL, not a long encoded blob
- Two-way replies — recipients can reply with one tap
- Stamp picker, confetti, background music
- Save letters as PNG images
- Fully serverless — hosted on GitHub Pages, stored in Supabase

---

## 🗂️ Repo Structure

```
/
├── index.html      ← The entire app (single file)
├── heart.mp3       ← Background music (you provide this)
└── README.md
```

---

## 🚀 Deploy in 5 Steps

### 1. Create your Supabase table

Go to your [Supabase project](https://supabase.com) → **SQL Editor** → run this:

```sql
create table letters (
  id          uuid        default gen_random_uuid() primary key,
  recipient   text        not null,
  sender      text        not null,
  body        text        not null,
  stamp       text        default '💌',
  letter_date text,
  created_at  timestamptz default now()
);

-- Enable Row Level Security
alter table letters enable row level security;

-- Anyone can send (insert) a letter
create policy "Anyone can send a letter"
  on letters for insert
  with check (true);

-- Anyone can read a letter if they know the ID
create policy "Anyone can read a letter by ID"
  on letters for select
  using (true);
```

### 2. Update your credentials in index.html

Open `index.html` and find these three lines near the top of the `<script>` block:

```js
const SUPABASE_URL = 'https://izfdodjvyqiptpllwnjp.supabase.co';
const SUPABASE_KEY = 'sb_publishable_ysgbkJ8BTi9wVwXahsMISA_alRVhQ-J';
const SITE_URL     = 'https://abbel-dawit.github.io/Cici';
```

Replace `YOUR_GITHUB_USERNAME` and `YOUR_REPO_NAME` with your actual values.  
Example: `https://sarah.github.io/cici`

### 3. Add your music file

Place your `heart.mp3` file in the **root of the repo** (same folder as `index.html`).

### 4. Push to GitHub

```bash
git add index.html heart.mp3 README.md
git commit -m "🚀 Launch CiCi"
git push origin main
```

### 5. Enable GitHub Pages

1. Go to your repo on GitHub
2. Click **Settings** → **Pages** (left sidebar)
3. Under **Source**, select `main` branch, folder `/` (root)
4. Click **Save**
5. Wait ~60 seconds — your site is live at:
   `https://abbel-dawit.github.io/Cici`

---

## 🔒 Security Notes

- Supabase Row Level Security (RLS) is enabled — letters are write-once, read-by-UUID
- The `SUPABASE_KEY` is a **publishable anon key** — safe to include in frontend code
- Letters are only accessible if you know the exact UUID — not browseable
- No user accounts or personal data beyond what's typed in the letter

---

## 🛠️ Local Development

Since this is a single HTML file you can just open it directly in a browser — but note:

- **Music** requires a server (browsers block local file audio). Run:
  ```bash
  npx serve .
  # or
  python3 -m http.server 8080
  ```
  Then open `http://localhost:8080`
- **Supabase** works fine locally as long as your credentials are correct

---

## 💅 Customisation

| What | Where in index.html |
|------|---------------------|
| App name / logo | Search `CiCi` |
| Colour scheme | `:root` CSS variables at the top |
| Stamp options | `const stamps = [...]` array |
| Footer social links | `<footer>` HTML section |
| Background music | Replace `heart.mp3` in root folder |

---

## 📄 License

MIT — do whatever you like with it. Made with 💕
