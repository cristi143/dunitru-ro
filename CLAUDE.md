# dunitru.ro — context de lucru

Site static (HTML/CSS) găzduit pe ClausWeb, cu deploy prin cPanel Git Version
Control. Momentan: pagină „în construcție".

## La începutul fiecărei sesiuni
Citește **`DEPLOY.md`** — acolo e tot despre hosting, deploy și structura repo-ului.

## Reguli de lucru
- Tot ce ajunge pe web stă în **`public/`**. Nimic altceva nu se copiază pe server.
- Se lucrează direct pe `main` (site mic, un singur om) — commit + push pe `main`.
  Înainte de push, **arată-i utilizatorului ce s-a modificat** ca să confirme.
- Deploy-ul îl face utilizatorul manual din cPanel (Update from Remote →
  Deploy HEAD Commit).
- Mesajele de commit au diacritice → se folosește fișier cu `-F`, nu `-m`.
- Se comite cu identitatea dată explicit dacă repo-ul n-are `user.name`/`user.email`:
  `git -c user.name="Cristi Iorga" -c user.email="cristi.s.iorga@gmail.com"`
- În commit nu se pune identificatorul de model.
- **Nu șterge `RewriteEngine Off` din `public/.htaccess`** — vezi DEPLOY.md §5.
- Fără build step, fără npm, fără framework. Dacă e nevoie de ceva mai mult decât
  HTML+CSS, se discută întâi.

## Context vecin
Site-ul e Addon Domain în același cont cPanel (`marcelpa`) cu aplicația
**marcel-parcel.ro** (`../../02_Marcel/marcel-parcel/`). Sunt proiecte complet
separate — nu se modifică nimic acolo din sesiunile de dunitru.
