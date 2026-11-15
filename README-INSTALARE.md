# VALMEX.md – pachet v2 (redesign + panou de administrare)

Pachetul conține doar fișierele NOI sau MODIFICATE. Imaginile, `valmex.js`, `images-loader.js`, `projects*.js`, `products*.js`, `valmex.css` și celelalte rămân cele din repository. Nu șterge nimic.

## Instalare (nu publica direct pe `main`)
1. Creează în GitHub o ramură nouă din `main`, de exemplu `redesign-v2`.
2. Copiază fișierele din acest pachet în rădăcina repository-ului, păstrând directoarele (`blog/`, `cms/`, `prezentari/`). Fișierele cu același nume se înlocuiesc. Originalele rămân în istoricul Git.
3. Activează previzualizarea ramurii (sau deschide fișierele local) și verifică paginile.
4. Când ești mulțumit, fă Pull Request `redesign-v2` → `main` și îmbină-l.

## Fișiere
- Pagini: `index, acoperis, fatade, amk, pavaj, proiecte, produse, despre, contact, blog, politica-confidentialitate, calculator-fatada, calculator-amk` + `blog/*.html` (6 articole existente + 1 nou)
- Pagină nouă: `blog/etapele-montarii-acoperisului-tigla-metalica.html` + `prezentari/montare-acoperis-3d.html`
- Stil: `valmex-v3.css`, `valmex-compat.css`
- Modificat: `valmex.js` (doar eliminarea popup-ului vechi; restul funcțiilor neschimbat)
- CMS: `admin.html`, `cms-public.js`, `projects.js` (modificat: citește proiectele din panou), `cms/content.json`, `cms/apps-script-admin-api.gs` (pentru modulul Cereri primite)
- Smart Conversion: `smart-conversion.js` (inclus în toate paginile; stil în `valmex-compat.css`)
- SEO: `sitemap.xml` (cele 20 de adrese vechi + cea nouă)
- NEATINS: `calculator-acoperis.html` (aplicație 3D separată, rămâne cu stilul vechi, de aceea `valmex.css` trebuie să rămână în repo)

## Panou de administrare
1. Deschide `https://valmex.md/admin.html`.
2. Creează un token GitHub Fine-grained: doar repository-ul `valmex-site`, permisiunea Contents – Read and write, cu expirare.
3. Lipește tokenul (se ține doar în memoria paginii). Panoul scrie pe ramura `cms-staging`, nu pe `main`.
4. Pentru ca modificările din panou să apară pe site, ramura `cms-staging` trebuie îmbinată în `main` (Pull Request).

## Ce funcționează acum prin panou → site
- Prețurile 950 / 136 / 850 lei/m² și datele de contact (telefon, email, WhatsApp) din paginile site-ului, prin `data-cms` + `cms-public.js`.
- Dacă `cms/content.json` lipsește sau e invalid, paginile rămân cu textul din HTML.

## Smart Conversion (implementat, inactiv implicit)
- Pornește doar dacă în panou activezi „Sistem activ”, doar după acceptarea cookie-urilor (`valmexCookieConsent=accepted`) și doar pe baza semnalelor alese (timp activ în secțiune, revenire, calculator, prețuri, rate 0%).
- Un singur popup pe sesiune (configurabil 1–3), nu apare în primele 8 secunde, nici cât timp e deschisă o fereastră, bannerul de cookies sau un câmp de text. NU = nu mai apare pentru numărul de zile setat.
- În `cms/content.json` există deja popup-ul pentru acoperișuri, cu textul tău. Textul de după DA este orientativ, editează-l din panou. Popup-urile pentru fațade și rate 0% au texte propuse (editabile din panou).
- Popup-urile vechi („-5% valabil doar azi” la ieșire și cel după 45 s) au fost ELIMINATE: codul din `valmex.js` și fereastra din pagini. `valmex.js` din pachet îl înlocuiește pe cel din repo.

## Proiecte
- În panou: Proiecte → „Importă din site” aduce cele 11 proiecte din `projects-data.js` (cu poze rezolvate la căi reale). După aceea le administrezi din panou.
- Pe site, `projects.js` folosește proiectele publicate din `cms/content.json`. Dacă fișierul lipsește sau nu are niciun proiect publicat, rămâne `projects-data.js`. Ca să nu apară lista veche, păstrează cel puțin un proiect publicat.

## Cereri primite (formulare)
1. Creează un proiect Apps Script NOU cu `cms/apps-script-admin-api.gs` (instrucțiunile sunt în fișier): pui ID-ul Sheet-ului în care scriptul existent salvează cererile și o cheie ADMIN_KEY.
2. Implementează ca aplicație web (Acces: Oricine) și copiezi adresa /exec.
3. În panou: Cereri primite → adresa + cheia. Poți filtra după status/dată/text, schimba statusul (scris în coloana „Status” a foii) și exporta CSV.
- Scriptul tău existent nu este modificat. Cheia nu se salvează în panou.

## SEO în pagini
- SEO și sitemap → „Aplică în pagină” scrie Title, Description, keywords, Open Graph și Twitter direct în fișierul HTML (ramura cms-staging). Câmpurile goale nu modifică nimic. Fișierele se rescriu doar în `<head>`.

## Ce NU este încă legat de site (setările se salvează, dar nu apar pe site)
- Calculatoarele (rate, fațadă, AMK, acoperiș) au prețurile în codul lor și nu citesc panoul.

## Configurări manuale
- Token GitHub (vezi mai sus).
- Imagini lipsă în repository: `images/amk/fatade-amk-proiect-finisat-5.jpg`, `images/amk/fatade-amk-finisat-6.jpg`, `images/amk/fatda-amk-finisat-8.jpg`, `images/garduri/porti.jpg`.
- Garanția: stabilită la „până la 25 de ani" (aplicat în acoperis.html). Articolele de blog vorbesc despre durata de viață (30–50 de ani), nu despre garanție, și nu au fost schimbate.
- Popup-ul vechi „-5% valabil doar azi" și popup-ul după 45 de secunde de inactivitate sunt încă în `valmex.js`. Contrazic brief-ul Smart Conversion.
- Testimonialele de pe Acasă (Andrei, Maria, Vasile) și „15+ specialiști" / numele echipei din Despre: confirmă că sunt reale.

## Testat
Chromium headless, desktop 1280 px și telefon 390 px: fără erori JavaScript și fără scroll orizontal pe 15 pagini; Smart Conversion (dezactivat, fără consimțământ, înainte de 8 s, NU și reîncărcare, DA → ofertă, o dată pe sesiune, semnal calculator, ferestre deschise); calculatoare, filtre, modale, meniu, temă luminoasă/întunecată, `cms-public.js` (valori noi, valori invalide, fișier lipsă), toate legăturile interne.

## Testat cu simulare (fără GitHub și Google reale)
Panoul a fost testat pe un GitHub și un Apps Script SIMULATE: conectare, salvare preț/text/contact, import proiecte, adăugare/ștergere/ordonare proiect, conflict de versiune, eroare de conexiune, istoric, filtre și status cereri, export CSV, protecție la HTML injectat, aplicare SEO în fișierele reale ale paginilor. Tokenul nu apare în localStorage/sessionStorage.

## Netestat
Conectarea reală la GitHub și la Apps Script (CORS, permisiuni), încărcarea/ștergerea imaginilor în GitHub, conversia WebP pe telefon, prezentarea 3D (necesită Three.js de pe internet), trimiterea formularelor către Apps Script, pagini pe un telefon Android real.
