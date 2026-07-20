# Setup guide

This is your version of Andrew6rant's animated GitHub profile README — a
"neofetch"-style card that auto-updates daily with your live GitHub stats
(age/uptime, repos, stars, commits, followers, and total lines of code).

## 1. Create the special repo

GitHub only shows a profile README if the repo name **exactly matches your
username**:

1. Create a new **public** repo named exactly `uchkunovdaniel`.
2. Upload all the files in this package to the root of that repo
   (keep the `.github/workflows/build.yaml` path intact).

## 2. Create a Personal Access Token

The workflow needs a token to query the GitHub GraphQL API on your behalf.

1. Go to GitHub → Settings → Developer settings → Personal access tokens →
   Fine-grained tokens → **Generate new token**.
2. Give it access to **all repositories** (or at least the ones you want
   counted toward lines-of-code/commits).
3. Under **Account permissions**, grant read access to: Followers, Starring,
   Watching.
4. Under **Repository permissions**, grant read access to: Commit statuses,
   Issues, Metadata, Pull requests — and **read and write** access to
   **Contents** (write is needed so the Action can push the updated SVG
   back to the repo).
5. Copy the generated token — you won't see it again.

## 3. Add repo secrets

In the new `uchkunovdaniel` repo → Settings → Secrets and variables →
Actions → **New repository secret**, add two secrets:

| Name | Value |
|---|---|
| `ACCESS_TOKEN` | the token from step 2 |
| `USER_NAME` | `uchkunovdaniel` |

## 4. Personalize the card

Open `dark_mode.svg` and `light_mode.svg` and edit the placeholder text —
search for these values and swap in your own:

- `Freelance / Consulting` → your role/company
- `VS Code` → your IDE(s)
- `TypeScript, Python, JavaScript` → your languages
- `your.email@example.com`, `your-linkedin-handle`, `yourwebsite.dev` →
  your real contact info
- `Plovdiv, Bulgaria` → your location

Also open `today.py` and replace the placeholder birthday
`datetime.datetime(2000, 1, 1)` near the bottom with your real birthday, if
you want the "Uptime" field to be accurate. (Leave it out entirely — and
remove the `age_data`/`Uptime` line from the SVGs — if you'd rather not show
your age.)

Do **not** touch anything with an `id="..."` attribute in the SVGs
(`age_data`, `repo_data`, `star_data`, etc.) — those are the placeholders the
script fills in automatically. Everything else is safe to edit freely.

## 5. Run it

- Push a commit, or wait for the daily 4am UTC cron job, and the Action will
  run `today.py`, fetch your stats, rewrite both SVGs, and commit the
  changes back to the repo.
- You can also trigger it manually from the Actions tab once you push
  (Actions → README build → Run workflow — you may need to add
  `workflow_dispatch:` under `on:` in `build.yaml` first if you want a
  manual button).

## 6. If you hit a 403 on push

If the Action fails at the `git push` step with something like
`Permission to .../....git denied to github-actions[bot]`, it's because the
default `GITHUB_TOKEN` GitHub gives the Action is read-only. This template
already works around it by checking out the repo with your `ACCESS_TOKEN`
and by setting `permissions: contents: write` in `build.yaml`, but you also
need your PAT itself to have **Contents: Read and write** (not just read) —
see step 2 above.

