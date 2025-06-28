# Secretos — Zero‑Backend Password Vault

**Secretos** lets you keep an encrypted list of secrets on GitHub without running any server. All crypto happens in your browser. The only thing you must protect is your **pass‑phrase**.

---

## What you need

1. A **public repo** called `secretos` (this one).
   It hosts the web page and **`config.enc`** — an encrypted copy of your Personal‑Access‑Token (PAT).
2. A **private repo** called `vault`.
   It stores one file, **`vault.json`**, containing the encrypted key/value list.
3. One **fine‑grained PAT** with **Contents Read & Write** on both repos.

---

## First‑time setup (≈ 3 min)

1. **Fork** this repo → your account. Enable **Pages** (`Settings → Pages → main / root`).
   Your site appears at `https://<you>.github.io/secretos/`.
2. **Create** a *private* repo named **`vault`** (leave it empty).
3. **Generate** a fine‑grained PAT → grant **Contents RW** on `secretos` and `vault`.
4. Open `https://<you>.github.io/secretos/make-config.html` in your browser.
   Paste the PAT and choose a long pass‑phrase → click **Generate & Commit**.
   The page encrypts the PAT and commits **`config.enc`** to `secretos`.
5. Visit `https://<you>.github.io/secretos/`, enter the pass‑phrase, and start adding secrets.

---

## Everyday use

* **Search** by typing in the filter box.
* **＋ Add** to create a new secret; click any row to edit.
* **Save** writes a fresh encrypted blob to `vault.json` through the GitHub API.

---

## Trust model (why this is safe)

* `config.enc` & `vault.json` are **AES‑256‑GCM** cipher‑text. Without the pass‑phrase they reveal nothing.
* Your pass‑phrase never leaves the browser. PAT is stored only inside `config.enc`.
* The PAT can write **only** to `secretos` and `vault`; it has no access elsewhere.
* Every commit keeps history—delete or revert via GitHub UI if needed.

---

## Rotate your token / pass‑phrase

1. Generate a new fine‑grained PAT.
2. Re‑run **make-config.html** with the new token (optionally a new pass‑phrase).
3. Click **Generate & Commit**—`config.enc` is replaced.
4. Revoke the old token.

---

## Locked out?

*Lose the pass‑phrase → data is unrecoverable.* Keep an offline backup of `vault.json` if the secrets are critical.

---

MIT License • Contributions welcome.
