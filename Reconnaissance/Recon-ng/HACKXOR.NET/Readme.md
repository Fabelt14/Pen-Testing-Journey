# 🔎 Recon-ng Lab: Host & File Enumeration on **hackxor.net**

## 📌 Introduction

In this lab I used **Recon-ng** (CLI) and its marketplace modules to do passive OSINT on **hackxor.net**.
The goal was to learn how modules gather hosts and find “interesting” files (like `robots.txt`), store results in a workspace, and export artifacts for reporting.

---

## ⚙️ Tools & Setup

* **Tool:** Recon-ng (Kali)
* **Workspace:** `test`
* **Modules used:**

  * `recon/domains-hosts/hackertarget`
  * `recon/domains-hosts/bing_domain_web`
  * `discovery/info_disclosure/interesting_files`
* **Target:** `hackxor.net`

---

## 🛠️ Steps Performed (commands I ran)

Below are the exact commands I used in this lab (Recon-ng + Recon-web). Copy-paste and run inside a Recon-ng session or follow the steps in the repo.

```text
# Recon-ng interactive / discovery
help                              # show recon-ng help
marketplace search <term>         # search available marketplace modules
marketplace install <module>      # install a module from the marketplace
marketplace info <module>         # show details about a marketplace module
shell clear                       # clear the recon-ng console screen

# Workspace management
workspaces list                   # list available workspaces
workspaces create test            # create a workspace named 'test'
workspaces select test            # select/load the 'test' workspace

# Module workflow
modules load recon/.../module     # load a module (e.g., recon/domains-hosts/hackertarget)
options                           # show current module options
options set SOURCE hackxor.net    # set module input/source (your target)
run                               # run the loaded module
show hosts                        # list hosts discovered in the workspace DB

# Extra local checks (after running modules)
cd ~/.recon-ng/workspaces/test/
ls                                # view saved files (CSV, downloaded files)
cat http_hackxor.net_robots.txt   # view downloaded robots.txt content

```
## 📊 Results

### Hosts discovered (from `hackertarget` + `bing_domain_web`)

* `research1.hackxor.net` → `138.68.117.124`
* `dreaded.hackxor.net`   → `138.68.117.124`
* `hkrb.hackxor.net`      → `138.68.117.124`
* `hmrc.hackxor.net`      → `138.68.117.124`
* `intranet.hackxor.net`  → `10.60.10.18`  *(internal address)*
* `transparency.hackxor.net` → `138.68.117.124`

**Total hosts found:** **7**

(You may see `research1.hackxor.net` listed twice in the raw output — recon-ng stores rows per module run.)

---

### Interesting files (from `discovery/info_disclosure/interesting_files`)

* Found: `http://hackxor.net/robots.txt` → **200 OK**
* Downloaded file saved to workspace:

  * `~/.recon-ng/workspaces/test/http_hackxor.net_robots.txt`

**robots.txt content:**

```text
User-agent: *
Disallow: /settings
Disallow: /pleasebanme
```

Other paths checked returned 404 (e.g., `sitemap.xml`, `phpinfo.php`, `admin-console/`).

---

### Dashboard summary (quick)

* Modules run: `hackertarget` (2 runs), `bing_domain_web` (1 run), `interesting_files` (1 run)
* Data gathered: Hosts = 7, Interesting Files = 1

---

## 💡 Analysis (what this means)

* Recon-ng combined multiple modules to build an inventory of subdomains and IPs. This gives a clear **attack surface map**.
* The internal address (`10.60.10.18`) is interesting — if seen in a real engagement, it hints at internal resources exposed in public records (worth investigating carefully and only with permission).
* `robots.txt` reveals *hidden* paths (`/settings`, `/pleasebanme`). Robots only tell crawlers not to index — they do **not** protect the directories. Attackers use `robots.txt` as a quick source of candidate URLs for further manual checks.
* The module’s 404 checks are useful: a 200 on one of those common admin paths (e.g., `phpinfo.php`) would be a high-value find. In this run, they were not present.

---

## ✅ Conclusion

**What I achieved**

* Mapped 7 hosts for `hackxor.net`.
* Downloaded `robots.txt` which exposed paths worth reviewing.
* Practiced workspace-based, repeatable recon workflows in Recon-ng.

**If this were a scoped pen test (authorized):**

1. Manually test `/settings` and `/pleasebanme` for input validation, auth checks, and exposed data (non-destructive testing only).
2. Investigate `intranet.hackxor.net` for internal exposure paths and whether it’s reachable from public hosts.
3. Save all exports (CSV) and screenshots, and produce a findings report with remediation steps.
