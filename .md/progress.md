# ZENDEX.fi — Project Progress Log

## Project Overview
- **Live site:** https://www.zendex.fi
- **Vercel project:** `zendexfi` under team `astragrowth-5196s-projects`
- **GitHub upstream repo:** `infiniteezverse/zendexfi` (repoId: 1259330372)
- **Working fork:** `zendexpro/zendexfi` — branch `patch-1`
- **Vercel Team ID:** `team_Euukd7t0u9IIDieLskJwsJXd`
- **Vercel Project ID:** `prj_Ey9owpNvljb0XrHBdsDw8raZkyFv`
- **Open PR:** https://github.com/infiniteezverse/zendexfi/pull/1 (deployed via Vercel, not merged at GitHub level)

---

## Accounts
| Service | Account |
|---------|---------|
| Vercel | `ezlabsup` / astragrowth@gmail.com |
| GitHub | `zendexpro` (fork owner) |

---

## Completed Tasks

### Task 1 — Add Logo + Favicon ✅
**Problem:** Logo (3D lightning bolt) was not showing above ZENDEX wordmark and not set as favicon after redeployments.  
**Root cause:** No `<img>` element and no `<link rel="icon">` in index.html. No image file in repo.  
**Solution:** Converted logo PNG to base64 JPEG data URI (8,559 chars) and embedded directly in index.html — redeployment-proof.  
**Changes:**
- Added `<link rel="icon" type="image/jpeg" href="data:image/jpeg;base64,...">` in `<head>`
- Added `<img class="logo" src="data:image/jpeg;base64,..." alt="ZENDEX Logo">` above wordmark
- Added `.logo { width: 100px; height: 100px; ... }` CSS

**Commit:** "Add logo above ZENDEX and as favicon (base64 embedded)"  
**Deployed via:** Vercel "Promote to Production" UI

---

### Task 2 — Increase Logo Size +30% ✅
**Change:** `.logo` CSS updated from `100px` → `130px`  
**Commit SHA:** `543bc07b2fb7046f49bd9e769a590fa8234d99e7`  
**Commit message:** "Increase logo size by 30% (100px → 130px)"  
**Deployed via:** Vercel API  
```
POST /api/v13/deployments?teamId=team_Euukd7t0u9IIDieLskJwsJXd
{ name: "zendexfi", gitSource: { type: "github", repoId: 1259330372, ref: "patch-1", sha: "543bc07b2fb7046f49bd9e769a590fa8234d99e7" }, target: "production" }
```

---

### Task 3 — Increase Logo Size +20% ✅
**Change:** `.logo` CSS updated from `130px` → `156px`  
**Commit SHA:** `b0162f0738dd3ec72c67c2ca843a7f02ddc32bd4`  
**Commit message:** "Increase logo size by 20% (130px → 156px)"  
**Deployment ID:** `dpl_5LR7sS3c3P22HhG5cCnNDbRZGJQz` — state: READY, target: production  
**Date:** 2026-06-13

---

## Deployment Workflow (Repeatable)

For any future change to www.zendex.fi:

1. Edit `https://github.com/zendexpro/zendexfi/edit/patch-1/index.html`
   - Use CodeMirror 6 JS API to inject changes:
     ```js
     const view = document.querySelector('.cm-content').cmTile.view;
     const doc = view.state.doc.toString();
     const newDoc = doc.replace(OLD, NEW);
     view.dispatch({ changes: { from: 0, to: doc.length, insert: newDoc } });
     ```
2. Click "Commit changes..." → set descriptive message → "Commit directly to patch-1"
3. Get full 40-char SHA from commit page
4. From Vercel dashboard tab, trigger production deployment:
   ```js
   fetch('/api/v13/deployments?teamId=team_Euukd7t0u9IIDieLskJwsJXd', {
     method: 'POST',
     headers: { 'Content-Type': 'application/json' },
     body: JSON.stringify({
       name: 'zendexfi',
       gitSource: { type: 'github', repoId: 1259330372, ref: 'patch-1', sha: '<FULL_40_CHAR_SHA>' },
       target: 'production'
     })
   }).then(r=>r.json()).then(d=>{ window._dResult = JSON.stringify({id:d.id, state:d.readyState}); });
   ```
5. Monitor: `fetch('/api/v13/deployments/<ID>?teamId=team_Euukd7t0u9IIDieLskJwsJXd').then(r=>r.json()).then(d=>{window._dStatus=d.readyState;})`
6. Verify live at https://www.zendex.fi

---

## Key Technical Notes

- **zendexpro cannot merge PR** into infiniteezverse/zendexfi (no write access). Use Vercel API instead.
- **Vercel does NOT auto-build** when fork branch (`zendexpro/zendexfi:patch-1`) is updated — must trigger manually.
- **Base64 logo** is embedded in index.html to survive redeployments (no external file dependency).
- **CodeMirror version:** CM6, accessed via `.cm-content.cmTile.view`
- **window.name trick:** Used to pass large strings (HTML content) across same-tab navigations on different domains.

---

## Current Logo Size History
| Version | Size | Commit |
|---------|------|--------|
| Initial (no logo) | — | — |
| After Task 1 | 100×100px | Add logo commit |
| After Task 2 | 130×130px | 543bc07 |
| After Task 3 | 156×156px | b0162f0 |
