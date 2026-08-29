# dunitru.ro

Site static pentru **dunitru.ro**. Momentan o pagină „în construcție".

- Tot ce ajunge pe web e în [`public/`](public/).
- Deploy: push pe `main`, apoi în cPanel *Git™ Version Control* → `dunitru-ro`
  → **Update from Remote** → **Deploy HEAD Commit**.
- Detalii complete despre hosting și deploy: **[DEPLOY.md](DEPLOY.md)**.

## Dezvoltare locală

Nu e nevoie de build. Deschizi `public/index.html` în browser, sau:

```bash
cd public && python3 -m http.server 8000
# http://localhost:8000
```
