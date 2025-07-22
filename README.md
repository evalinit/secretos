# Los Secretos

A zero‑backend password vault that runs **entirely in your browser**.

* **Secrets** live in `vault.json` inside a private repo you own.  
* **This page** (`index.html`) sits in a public repo and runs on GitHub Pages.  
* Your **Personal Access Token (PAT)** never leaves the device—it’s AES‑encrypted with your pass‑phrase and stored in browser storage.

---

## 1 · Repo layout

| Repo | Visibility | What’s in it |
|------|------------|-------------|
| **`secretos`** | Public | `index.html` (this app) |
| **`vault`** | **Private** | `vault.json` (encrypted secrets) |

---

## 2 · First‑time setup (per browser)

1. **Fork** `secretos` and enable GitHub Pages → `/secretos/`.  
2. **Create** a private repo called **`vault`** (empty main branch).  
3. **Generate** a *fine‑grained* PAT with:<br> • Repos → **`vault`**<br> • Permissions → **Contents → Read & Write**  
   > *Tip: set an expiry (30‑60 days) for easy revocation.*  
4. Open your Pages URL. The **Setup** form appears.  
5. Paste the PAT, choose a long pass‑phrase, decide whether to check **remember**.  
   *Your browser encrypts the PAT and stores it:  
   `localStorage['secretos-config']`.*

Next reload: the app auto‑unlocks using the encrypted PAT + your pass‑phrase.

---

## 3 · Daily use

* **Add** a secret – suggestion is a 24‑char strong password.  
* **Delete** – asks for confirmation.  
* **🔍 Filter** – live search.  
* **📋 Copy** – copies plaintext value.  
* All edits **auto‑save** back to `vault.json` (PBKDF2‑250k + AES‑GCM).

---

## 4 · Top‑right controls

| Control | What it does |
|---------|--------------|
| **remember** (checkbox) | • ON → pass‑phrase cached in `localStorage` (auto‑unlock every visit).<br>• OFF → cached only in `sessionStorage` (clears on browser quit). |
| **Log out** | Removes cached pass‑phrase (both storages). Next page load shows Unlock screen. |
| **Uninstall** | Wipes encrypted PAT **and** pass‑phrase caches. Next page load shows Setup screen. |

---

## 5 · Token rotation

If you revoke the PAT on GitHub:

1. Click **Uninstall**.  
2. Reload → Setup form.  
3. Paste new PAT, same (or new) pass‑phrase → vault re‑encrypted automatically.

---

## 6 · Security model

* **Offline repo clone:** attacker sees only encrypted `vault.json`. PAT is not in the repo.  
* **Browser storage:** PAT ciphertext + PBKDF2‑salt. Brute‑forcing it is equivalent to brute‑forcing your pass‑phrase.  
* **Extensions / XSS:** anything that can execute JS while the vault is unlocked can read secrets and PAT. Keep a clean profile.  
* **Device theft:** if **remember** was ON, the thief still needs the pass‑phrase; if OFF, storage is empty after a browser quit.  
* **TLS:** all GitHub API traffic is HTTPS.

---

## 7 · Backup & restore

* Download `vault.json` periodically for offline backup.  
* To restore: push it back to the `vault` repo—same pass‑phrase decrypts it.

---

MIT License – hack away and stay safe.

ChatGPT wrote everything in this repo except this sentence. Sorry.
