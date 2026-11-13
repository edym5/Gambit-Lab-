# Gambit Lab — publicare pe contul tău

Acest proiect este o copie portabilă a beta 0.4: Next.js + PostgreSQL. Nu depinde de contul ChatGPT pentru funcționare sau actualizări. Nu include schimbarea anulată cu mentorul AI.

## 1. GitHub

1. Creează sau deschide contul tău la https://github.com.
2. Creează un repository numit `gambit-lab`. Poate fi Private; site-ul public nu cere cod public.
3. Descarcă și dezarhivează arhiva proiectului.
4. În repository alege `uploading an existing file` (sau Add file → Upload files).
5. Încarcă CONȚINUTUL folderului `gambit-vercel`, nu arhiva ZIP. `package.json`, `app`, `public`, `lib`, `scripts`, `database` trebuie să fie la rădăcină. Încarcă și `package-lock.json`.
6. Apasă Commit changes. Fișierul WASM Stockfish trebuie încărcat și el; este în public/vendor și are sub 25 MB.

Nu încărca `node_modules`, `.next`, `.env.local` sau parole. Arhiva livrată nu le include.

## 2. Vercel

1. Creează/deschide contul tău la https://vercel.com. Conectarea cu GitHub simplifică importul.
2. Add New → Project → importă repository-ul `gambit-lab`.
3. Framework Preset: Next.js. Root Directory: folderul care conține package.json (rădăcina dacă ai urmat pasul 1).
4. Build Command: `npm run build`; Install Command: `npm ci`; Output Directory: implicit Next.js. Node.js: 22.x.
5. Apasă Deploy. Prima publicare poate funcționa fără DATABASE_URL: tabla, boții și lecțiile merg, iar conturile arată că baza de date nu este conectată.
6. Deschide adresa de producție returnată de Vercel. Dacă accesul cere autentificare Vercel, verifică Settings → Deployment Protection și setările pentru Production înainte să distribui linkul. Nu distribui un link de Preview în locul domeniului de producție.

## 3. Conturi și baza de date

1. În Vercel deschide Marketplace / Storage și alege Neon (PostgreSQL).
2. Creează baza de date în contul tău, verificând planul, limitele și eventualele costuri afișate. Alege o regiune apropiată de cea a funcțiilor.
3. Conecteaz-o la proiectul Gambit Lab pentru Production. Folosește baze/ramuri separate pentru Preview dacă testezi schimbări acolo.
4. În Settings → Environment Variables verifică existența lui `DATABASE_URL`. Dacă integrarea folosește alt nume, configurează DATABASE_URL cu șirul de conectare PostgreSQL furnizat de Neon. Nu pune valoarea în GitHub și nu o trimite în chat.
5. Fă Redeploy. Scriptul inclus creează tabelele la build, printr-o migrare tranzacțională care se aplică o singură dată. Dacă baza de date nu poate fi accesată, publicarea este oprită în loc să fie ascunsă eroarea.
6. Testează pe site: creează un cont, păstrează codul de recuperare, schimbă o preferință, joacă o partidă până la final, deconectează-te și autentifică-te din nou. Verifică progresul și istoricul.

Baza nouă începe fără conturile/partidele din ChatGPT Sites. Transferul lor este un pas separat; nu șterge baza veche. Progresul local din browser este legat de domeniu și nici el nu apare automat pe noul domeniu.

## 4. Actualizări fără ChatGPT

Modifică fișierele în repository-ul tău și fă Commit / push pe main. Vercel publică automat modificările dacă repository-ul este conectat. Nu trebuie să ai acest cont ChatGPT și nici să păstrezi un abonament ChatGPT pentru acest flux. Rămân necesare accesul la GitHub/Vercel/Neon și respectarea limitelor planurilor lor.

- texte/interfață: public/game.html
- design: public/style.css
- boți: public/bots.js
- lecții: public/lessons.js
- logica jocului: public/app.js
- API conturi: app/api/account/route.ts
- adaptor PostgreSQL: lib/database.js
- migrare inițială: database/001_initial.sql

Nu schimba migrarea deja aplicată pentru a adăuga câmpuri noi: adaugă o migrare nouă și actualizează runner-ul. Fă backup înainte de schimbări de schemă.

## Dezvoltare locală (opțional)

Instalează Node.js 22. În folderul proiectului:

```sh
npm ci
npm test
npm run dev
```

Pentru conturi locale configurează DATABASE_URL în mediul procesului; poți folosi Node `--env-file=.env.local` pentru scriptul de migrare. Next.js citește .env.local la rularea aplicației. Nu comite acest fișier.

```sh
node --env-file=.env.local scripts/migrate.mjs
```

## Limite și licențe

Parolele sunt hash-uite cu bcrypt. Sesiunile folosesc cookie HttpOnly; resetarea folosește codul secret de recuperare. Nu sunt configurate emailuri de verificare sau recuperare. Estimările Elo sunt pentru antrenament, nu ratinguri certificate. Nu există un LLM configurat în acest export.

Licențele chess.js, Stockfish GPLv3 și bcryptjs sunt incluse. Păstrează notele și legătura către sursa exactă Stockfish din subsol când redistribui motorul.

Referințe oficiale:
- https://vercel.com/docs/git
- https://vercel.com/docs/projects/managing-projects
- https://vercel.com/docs/marketplace-storage
- https://vercel.com/docs/environment-variables
