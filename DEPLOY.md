# dunitru.ro — ghid de deploy și recuperare

Documentația „cum funcționează totul" pentru site-ul **dunitru.ro**.
Dacă pierzi fișierele locale, începe de aici — **nu se pierde nimic, tot codul
e pe GitHub**.

Sistemul e identic ca la `marcel-parcel.ro` (GitHub → cPanel Git Version Control
→ Deploy), doar mai simplu: site static, fără PHP, fără bază de date.

---

## 1. Ce este și unde trăiește

- **Site:** pagină statică HTML/CSS („în construcție"), fără dependențe.
- **Cod sursă (sursa de adevăr):** GitHub `cristi143/dunitru-ro`.
- **Producție:** ClausWeb (shared hosting), cPanel cu *Git™ Version Control*.
  - cont: `marcelpa` (dunitru.ro e **Addon Domain** în acest cont)
  - document root dunitru.ro: `/home/marcelpa/dunitru.ro/`
  - clona Git a serverului: `/home/marcelpa/repositories/dunitru-ro`
  - **fără** SSH shell pe server

---

## 2. Cum faci o modificare (flux normal)

1. Ceri modificarea în Claude (sau editezi direct `public/index.html`).
2. Se face commit și push pe `main`.
3. **cPanel → Git™ Version Control → `dunitru-ro` → Manage → Pull or Deploy:**
   - **Update from Remote** (trage ultimul commit din GitHub)
   - **Deploy HEAD Commit** (rulează `.cpanel.yml`)
4. Verifici https://dunitru.ro (Ctrl+F5 pentru a ocoli cache-ul browserului).

Log-ul deploy-ului: `/home/marcelpa/deploy-dunitru.log` (vizibil din File Manager).

> Important: cPanel oprește deploy-ul la prima comandă cu exit ≠ 0.

---

## 3. Ce face deploy-ul (`.cpanel.yml`)

1. Verifică de siguranță că document root-ul **nu** conține `index.php`
   (protecție ca nu cumva să golească din greșeală `public_html/`, unde stă
   aplicația Laravel Marcel Parcel).
2. Golește `/home/marcelpa/dunitru.ro/` — astfel ștergerile din repo se propagă.
3. Copiază tot conținutul lui `public/` acolo (inclusiv `.htaccess`).
4. Scrie în log lista fișierelor publicate.

Tot ce trebuie să ajungă pe web se pune în **`public/`**. Restul fișierelor din
repo (`README.md`, `DEPLOY.md`, `.cpanel.yml`, `CLAUDE.md`) rămân doar în git.

---

## 4. Structura repo-ului

```
dunitru-ro/
├── .cpanel.yml        # rețeta de deploy rulată de cPanel
├── CLAUDE.md          # context de lucru pentru Claude
├── DEPLOY.md          # acest fișier
├── README.md
└── public/            # ← TOT ce ajunge pe dunitru.ro
    ├── index.html
    ├── style.css
    ├── favicon.svg
    ├── robots.txt
    ├── sitemap.xml
    └── .htaccess
```

---

## 5. Note importante

- **`.htaccess` conține `RewriteEngine Off`.** Motivul: dacă document root-ul
  dunitru.ro ajunge *sub* `public_html/` (ex. `public_html/dunitru.ro`), regulile
  de rewrite ale Laravel-ului din `public_html/.htaccess` s-ar moșteni și ar
  strica site-ul. Linia asta le neutralizează. Nu o șterge.
- **Dacă schimbi document root-ul** din cPanel, actualizează calea în `.cpanel.yml`
  (apare în mai multe linii) și în acest fișier.
- **Domeniul e Addon Domain** în contul `marcelpa`. Cele două site-uri împart
  contul (spațiu pe disc, trafic), dar au document root-uri separate și repo-uri
  Git separate — deploy-urile nu se influențează reciproc.
- **Certificat SSL:** ClausWeb emite de obicei automat AutoSSL pentru domeniile
  din cont. Dacă https nu merge după câteva ore, din cPanel → *SSL/TLS Status*
  → bifezi dunitru.ro → **Run AutoSSL**.

---

## 6. Recuperare după pierderea fișierelor locale

```bash
git clone git@github.com:cristi143/dunitru-ro.git
```
Atât. Nu e nimic de instalat, de configurat sau de compilat — e HTML static.
Producția rulează neatinsă între timp.
