# statuspageX docs

## Version: 1.0.1

> simple, reliable, affordable self-hosted status page.

Welcome to your new status page! This guide will walk you through deploying your StatuspageX site to Netlify using GitHub, creating and editing incident/maintenance content, and customizing your status page.

---

## 1. Deploying to Netlify with GitHub

### **A. Prerequisites**
- A GitHub account
- A Netlify account (https://netlify.com)

### **B. Initialise the Repository**
1. **Initialise** a new GitHub repository to your own GitHub account, and add the contents of this website to your repository to bootstrap your status page.
Before you deploy for the first time, ensure you edit and set all the config you need. You cna also run it locally to verify all the content first, before pushing to GitHub.

### **C. Connect to Netlify**
1. Log in to Netlify and click **"Add new site" > "Import an existing project"**.
2. Choose **GitHub** and authorize Netlify if prompted.
3. Select your repository.
4. Set the build command to:
   ```
   hugo
   ```
5. Set the publish directory to:
   ```
   public
   ```
6. (Optional) Set the environment variable for Hugo version (recommended):
   - `HUGO_VERSION = 0.123.7` (or the version in use)
7. Click **Deploy Site**.

### **D. Custom Domain (Optional)**
- In Netlify, go to **Domain management** to add your custom domain.

---

## 2. Creating Content (Incidents, Maintenance, Informational)

All content lives in the `content/issues/` directory as Markdown (`.md`) files. Each file represents an incident, maintenance, or informational post.

### **A. Front Matter Fields (YAML)**
Each file starts with a block like this:
```yaml
---
title: "Short, clear title"
date: 2025-07-18T10:00:00Z  # Start time (UTC, ISO8601)
resolved: false             # true if resolved, false if ongoing
resolvedWhen: 2025-07-18T12:00:00Z  # (optional) When resolved
severity: down              # down | disrupted | notice
affected:
  - CDN                     # List of affected systems (must match config)
customerImpactDuration: 120 # (optional) Minutes customers were affected
summary: "One-line summary for lists and SEO."
tags:
  - outage                  # Any tags you want
  - cdn
section: issue              # Required for the system to recognize this as an incident
---
```

### **B. Supported Field Values**
- **title**: Any string
- **date**: ISO8601 (e.g., 2025-07-18T10:00:00Z)
- **resolved**: `true` or `false`
- **resolvedWhen**: ISO8601 (when resolved)
- **severity**:
  - `down` (major outage)
  - `disrupted` (partial outage or degraded)
  - `notice` (informational/maintenance)
- **affected**: List of system names (must match names in `config.yml`)
- **customerImpactDuration**: Integer (minutes)
- **summary**: One-line summary
- **tags**: List of tags
- **section**: Must be `issue`

### **C. Example: Major Outage**
```yaml
---
title: "CDN Unavailable"
date: 2025-07-18T10:00:00Z
resolved: false
severity: down
affected:
  - CDN
summary: "Increased latency and asset loading failures for users in Europe."
tags:
  - outage
  - cdn
section: issue
---

Our CDN edge nodes are Unavailable, impacting all EU sites.

**Customer Impact:**
- CDN edge nodes are Unavailable, impacting all EU sites.

**Root Cause:**
- Hardware failure in CDN provider.

**Resolution:**
- Ongoing investigation.
```

### **D. Example: Scheduled Maintenance**
```yaml
---
title: "Scheduled Maintenance: Database Cluster"
date: 2025-08-29T01:00:00Z
resolved: false
severity: notice
affected:
  - Database Cluster
summary: "Scheduled maintenance will be performed on the Database Cluster. Downtime will occur."
tags:
  - maintenance
  - database
section: issue
---

Scheduled maintenance will be performed on the Database Cluster from 01:00 to 02:00 UTC. Downtime will occur.

**Customer Impact:**
- Downtime will occur.
```

### **E. Example: Informational**
```yaml
---
title: "Monitoring Dashboard Update"
date: 2025-07-06T12:00:00Z
resolved: true
severity: notice
affected:
  - Monitoring Dashboard
summary: "Monitoring dashboard received a UI update. No impact to monitoring or alerting."
tags:
  - informational
  - monitoring
section: issue
---

A new UI update was deployed to the Monitoring Dashboard. No impact to monitoring or alerting functionality.

**Customer Impact:**
None.
```

---

## 3. Editing Content

- **To edit an incident:**
  1. Open the relevant `.md` file in `content/issues/`.
  2. Change any field (e.g., set `resolved: true` and add `resolvedWhen` when resolved).
  3. Commit and push your changes to GitHub.
  4. Netlify will auto-deploy your changes.

- **To add a new incident:**
  1. Copy an existing file or use the template above.
  2. Save it as a new `.md` file in `content/issues/`.
  3. Commit and push.

- **To delete an incident:**
  1. Delete the file from `content/issues/`.
  2. Commit and push.

---

## 4. Customizing Systems and Categories

- Edit `config.yml` under `params.systems` to add, remove, or rename systems.
- Edit `params.categories` to organize systems into categories.
- System names in `affected:` must match exactly (case-sensitive).

---

## 5. Advanced: Local Development

- Install [Hugo](https://gohugo.io/getting-started/installing/)
- Run locally:
  ```
  hugo server --disableFastRender
  ```
- Visit [http://localhost:1313](http://localhost:1313)

---

## 6. Troubleshooting

- **Incident not showing?**
  - Make sure `section: issue` is in the front matter.
  - Check for YAML errors (use spaces, not tabs).
  - System name in `affected:` must match config.
- **Build fails with division by zero?**
  - This is fixed in this version. If you see it, update your theme.
- **Netlify not updating?**
  - Push to GitHub and check Netlify deploy logs.
  - Clear your browser cache or use a private window.

---

## 7. Support

If you have any issues, email matt@statuspagex.com

---

Enjoy your new status page! 