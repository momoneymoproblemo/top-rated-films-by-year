# Top Rated Films by Year — unofficial Stremio add-on

A free catalogue-only Stremio add-on, rebuilt weekly using IMDb's published non-commercial datasets. It does not serve any streams. The homepage is designed in an IMDb-inspired dark/yellow style; the project is not endorsed by IMDb or Stremio.

## Updated features

- 1920 through the current year (2026 at the time of this release), newest first in **both** the site dropdown and Stremio year dropdown.
- **Required** year in Stremio: the unwanted "None" choice is removed. URLs without a selected year still receive the current year's list to retain backward compatibility.
- Cinema-inspired homepage with real ranked poster cards and a working search for films/years. All movie listings are produced from IMDb, not hardcoded or invented.
- Rotating cinematic hero image: cycles through available film backdrops from the *selected year's list*, roughly every 8.5 seconds, with a fallback image if artwork is unavailable.
- One-click **Install Add-on for Stremio** button and separate **Copy Stremio Link** button.
- Retains original filtering: minimum vote counts, weighted score, 60-minute runtime floor, 6.0 rating floor, up to 250 titles per year, documentaries included, concerts excluded, and Indian-language films from 2015 onwards excluded based on Wikidata with a cached fallback.
- Same add-on ID `community.topratedbyyear` and same catalog ID `top-by-year` for compatibility with previous installations.

## Publish to GitHub (important)

**1.** Extract the ZIP on your computer. Upload **all the contents** of the extracted folder to the *root* of your GitHub repository. Keep the folder layout exactly as it is, including the hidden-looking `.github/workflows/update.yml` file. Do not upload the ZIP as a single file, and do not nest everything under another directory.

The repo must contain these paths:

```
.github/workflows/update.yml    <-- required automated updater, inside .github/workflows
build.js                        <-- weekly IMDb/Wikidata list builder
data/india.json                 <-- cached India/language filtering data
README.md
docs/index.html                 <-- public website
docs/assets/hero-fallback.webp
docs/logo.png
docs/background.png
docs/.nojekyll
```

**2.** Open the repository on GitHub, then **Settings → Pages → Build and deployment → Source → GitHub Actions**. (Do not select "Deploy from a branch": the workflow publishes its own Pages build after rebuilding data.)

**3.** Open **Actions** and find **Rebuild and publish film lists**. On the first upload to `main`, the workflow should start automatically. If it does not, select **Run workflow**. Wait for **both** `rebuild` and `deploy` jobs to finish successfully. The first run downloads fresh IMDb datasets and may take several minutes.

**4.** Open `https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/`. Your movie selections, posters, hero art, copy-link button and manifest should now be available. The copy and install buttons automatically use the site's real URL.

**5.** Changes to the main code or site, plus an automatic weekly schedule, will rebuild and republish the lists. You can also run it manually from the Actions tab.

### Troubleshooting

- **Workflow missing?** Confirm the file is located at `.github/workflows/update.yml` (not `update.yml` in the root). In GitHub, you can use **Add file → Create new file** and enter the full path `.github/workflows/update.yml` if a computer file manager hides the folder.
- **The website is visible but no films appear?** The automatic build has not completed or failed. Visit **Actions** and inspect the `rebuild` job; the site deliberately displays an informative message until `summary.json` and film JSON files are generated.
- **Deploy job fails?** Check Settings → Pages → Source is **GitHub Actions** and allow repository Actions to run. `pages: write` and `id-token: write` permissions are already in the workflow.
- **Keep the old repo URL?** Upload into the existing `top-rated-by-year` repository. When deployed on a different repository, the build automatically constructs the manifest asset URLs using `GITHUB_REPOSITORY`.
- **Using a custom domain?** Set `PUBLIC_BASE` to the full website origin/base path in the build step's environment.

## How the rankings are calculated

The IMDb weighted formula is `v/(v+m) × R + m/(v+m) × C`. The tuning constants and India/Wikidata language filter are **unchanged** from the original source, apart from starting at 1920 instead of 1950. From 2000 onwards, films need at least 20,000 votes. Older years use a per-year scaled minimum (minimum 1,000 votes). Films shorter than 60 minutes, concert documentaries, and adult titles are excluded. Documentaries otherwise remain eligible. Exclusions for Indian-language films apply **only from 2015 onwards**.

IMDb provides the datasets for non-commercial use; this add-on is not officially associated with IMDb or Stremio. Film artwork is delivered from the existing Stremio metadata image host and might occasionally be unavailable. No image API keys are necessary.
