# Livecycle Migration Guide

## Scope
This guideline designed to help migrate livecycle landing to external 3rd party providers due to the sunsetting of the platform, which means:
- Livecycle AI platfrom will no longer be available for editing and publishing new pages after 31/08/2025.
- Livecycle Hosting services will continue to run up to 31/10/2025 so existing landing pages will still work.
- Livecycle Data Generation Apis and Traffic splitting services will shut down on 30/09/2025.
- Livecycle support via email and slack will be still available during all that time (up to 30/09/2025)

A Github account is neccssary for this action, it can be a free github account.

Use this [form](https://get-livecycle.notion.site/2530b1258d8780119173c8f2187d6a01?pvs=105) to fill out the account details.

This guideline assume that you've already transfered the Livecycle Github migration repositories for your domains to your own Github account.

## Migrate deployment/hosting pipeline
This repository contains your static website (self‑contained landing page directories with `index.html` and static assets). 
It is intended to be deployed using a third‑party static hosting provider of your choice*, such as **GitHub Pages** or **Cloudflare Pages**.  
The instructions below help you migrate from our hosting solution to your own account.
Migration is quick and it should take around 10-15m to setup to complete deployment/hosting pipeline. (domain propgation can take up to 24h)

#### Remarks
- Cloudflare and Github pages are used as example do they are simplicty,
production robustness and free cost*, Livecycle pages can be hosted by any other hosting provider that support static file hosting.
- Github pages on free tier does require the repo to be public

## Option 1 – Deploying to GitHub Pages (Simplest, fastest)

### 1. Enable GitHub Pages (10m)

1. Go to your repository’s **Settings** → **Pages**.
2. Under **Source**, choose the branch (usually `main`) and select **/(root)** as the folder. Click **Save**.
3. GitHub will build and serve your site at:
https://<github‑username>.github.io/<respostiry-name|domain>

### 2. Configure a custom domain 

1. In the same **Pages** settings, locate the **Custom domain** field.
2. Enter your domain (for example, `my-website.com`) or subdomain (for example, `lp.my-website.com`) and click **Save**. GitHub will add a `CNAME` file to your repository.
3. Update the DNS at your domain provider:

- **Subdomain** (`lp.my-website.com`): create a **CNAME** record pointing your subdomain to `<github‑username>.github.io` (without the repository name):

  ```text
  # Example CNAME record for GitHub Pages subdomain
  Type   Name   Value
  CNAME  www    <github‑username>.github.io
  ```

- **Apex domain** (`my-website.com`): create **A** records pointing to the GitHub Pages IP addresses. Use four separate records – one for each IP:

  ```text
  # Example A records for GitHub Pages apex domain
  Type  Name  Value
  A     @     185.199.108.153
  A     @     185.199.109.153
  A     @     185.199.110.153
  A     @     185.199.111.153
  ```

4. DNS changes can take up to 24 hours to propagate. Once complete, your custom domain will show your GitHub Pages site. You can verify the records using `dig example.com +noall +answer -t A` for apex domains or `dig www.example.com +nostats +nocomments +nocmd` for subdomains:contentReference[oaicite:1]{index=1}.
   
* Read more [here]https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

### 3. Enforce HTTPS (recommended)

After your custom domain resolves, return to **Settings** → **Pages** and select **Enforce HTTPS**. This ensures traffic is encrypted.

## Option 2 – Deploying to Cloudflare Pages

Cloudflare offer a simple and free deployement option for static websites (cloudflare pages), you'll need to create a cloudflare account if you don't have one already.

### 1. Create a Pages project

1. Log in to [Cloudflare](https://dash.cloudflare.com/) and navigate to **Workers & Pages** → **Pages**.
2. Click **Create a project** and choose **Connect to Git** to connect this repository, or choose **Direct Upload** to upload the static files manually.
3. Set **Framework preset** to **None** (for static sites) and leave the build command empty. Set **Output directory** to `/`.

### 2. Add a custom domain

1. In your Pages project, go to **Custom domains** and click **Add**.
2. Enter your domain or subdomain and follow the prompts.

### 3. Configure DNS

- **Subdomain**: if you do not want to change nameservers, add a **CNAME** record at your DNS provider pointing your subdomain to your Pages URL (`<project-name>.pages.dev`):
```text
# Example CNAME record for Cloudflare Pages subdomain
Type   Name   Value
CNAME  shop   <project-name>.pages.dev
```
- **Apex domain**: your domain must use Cloudflare nameservers. Update your nameserver records at your domain registrar to the nameservers provided by Cloudflare. Once the domain is active in Cloudflare, Cloudflare will automatically create the necessary CNAME records for your Pages site.


## Option 3 - Any other static hosting provider
In most other providers (Netlify, Vercel, AWS Amplify, etc.), the process is similar: connect the repository, define the output directory as the repository root, and configure DNS according to their documentation.
You contact Livecycle team via slack if any help is needed.

## Option 4 - Adjust the pages for external custom cms/platform (wix, instapages, wordpress, etc...)
Livecycle pages are simple static artifacts of pages+assets, most cms platforms use their own custom format which means editing the code of the files is needed for migration.
In each platform it can be different and it require assitance for the platform administrator to check what's the best approach.

## Updating the landing pages
Since livecycle platform is not active anymore, updating the website can be done with a regular text/code editor
or using AI agents like OpenAI Codex, Google Jules, etc...


## FAQ

Q: Can Livecycle pages run in different platforms than Livecycle?  
Yes! Livecycle pages are regular/standard html pages and images that can be serve from regular web servers and CDN/hosting providers.  

Q: Will the files work in wix/instapages/wordpress and other CMS/Web platforms?  
A: The pages can work on any hosting provider or platform that support static files.  
For example, cloudflare pages, netlify, vercel, github pages can work easily, platforms that doesn't support static files serving will require manual adjustment by the platform administrators if possible.  

Q: When will my pages stop working on livecycle?  
A: On 31/10/25, before that you should migrate your pages to a different hosting provider and adjust the dns accordingly. Use this guide for migration and feel free to reach us on slack and via email. 

Q: What about pages that leverage runtime AI generation or traffic spliting?  
A: These features will shut down on 30/09/25, any landing page that still need these features should switch to am alternative solution.  
