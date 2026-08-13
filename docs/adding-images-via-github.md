# Adding images through the GitHub website (no technical tools needed)

This guide is for anyone who wants to add or replace a jumbotron or favicon
in this repository using **only the web browser** — no installations, no
command line, no "cloning". If a word like *branch* or *pull request* is new
to you, don't worry: it is explained where it first appears, and the steps
tell you exactly which button to click.

> **The one rule that matters most:** changes on the `main` branch are live
> on the actual websites within minutes. That is why every step below saves
> your work as a **proposal** (a *pull request*) instead — a colleague checks
> it, and only after they press the merge button does anything go live.
> As long as you follow the steps, you cannot break a website.

## 1. Prepare your files

Have the files ready on your computer, with **exactly** these names:

| File           | What it is                                | Requirements |
| -------------- | ----------------------------------------- | ------------ |
| `jumbotron.webp` (or `.jpg` / `.png` / `.svg`) | The big banner photo at the top of the site | At least 1920 px wide, ideally 2560 px; aim for 500 KB or less |
| `favicon.ico`  | The little icon in the browser tab        | Must be `.ico` — see below if you only have another format |

You will also need to know, for each file, **who supplied it and when** —
for example "supplied by the municipality of Epe on 3 June 2026" or "taken
from the home page of epe.nl on 3 June 2026 as a placeholder". This gets
written down in a small text file called `origin.md` (step 4). We never use
stock photos or images of unknown origin.

**Only have a large JPG or PNG for the banner?** That is fine — upload it as
`jumbotron.jpg` or `jumbotron.png`. If you want to make it smaller yourself,
the free website [squoosh.app](https://squoosh.app) runs entirely in your
browser: drag your photo in, choose **WebP** on the right, and save the
result as `jumbotron.webp`.

**Only have a logo (SVG/PNG) instead of a `favicon.ico`?** Use
[realfavicongenerator.net](https://realfavicongenerator.net) in your
browser: upload the logo, download the package, and take the `favicon.ico`
from it. Keep the original logo file too — it goes in the `source` folder.

## 2. Find (or create) the organisation's folder

Open <https://github.com/ConductionNL/woo-website-assets> and click through
the folders: first the category — `municipalities`, `water-authorities`,
`partnerships` or `other` — then look for the organisation.

Folder names are always the organisation's name in **lowercase**, with
**hyphens instead of spaces** and **no accents**: Gooise Meren becomes
`gooise-meren`, Súdwest-Fryslân becomes `sudwest-fryslan`.

- **The folder is already there** (this is almost always the case — a folder
  exists for every organisation, even before it has images): continue with
  step 3.
- **The folder does not exist yet:** see
  [Creating a brand-new folder](#creating-a-brand-new-folder) at the bottom,
  then come back to step 3.

## 3. Upload the images

1. Click the organisation's folder to open it (for example
   `municipalities/epe`).
2. Click the **Add file** button (top right, next to the green button) and
   choose **Upload files**.
3. Drag your `jumbotron.webp` and `favicon.ico` into the page (or click
   *choose your files*).

   ⚠️ Make sure you are uploading into the organisation's folder itself —
   the page title above the upload area should read something like
   `woo-website-assets / municipalities / epe /`.
4. Scroll down to the box titled **Commit changes**. In the first (short)
   text field, briefly describe what you did, for example:
   `Add jumbotron and favicon for Epe`.
5. **Important:** select **"Create a new branch for this commit and start a
   pull request"** — *not* "Commit directly to the main branch". A *branch*
   is simply a named working copy where your changes wait safely until they
   are approved. GitHub suggests a branch name; you can leave it as is.
6. Click the green **Propose changes** button.
7. GitHub now shows a page titled **Open a pull request** — this is your
   change proposal. Click the green **Create pull request** button. Done:
   your images are uploaded and waiting for review. **Leave this browser tab
   open** — you'll need the branch name in the next step.

## 4. Record where the images came from

Every organisation folder contains a file `source/origin.md` where we note
who supplied each image and when. Add your images to it, **on the same
branch** you just created:

1. Go back to the front page of the repository
   (<https://github.com/ConductionNL/woo-website-assets>).
2. At the top left, above the file list, there is a dropdown button that
   says **main**. Click it and select **your** branch — the one from step 3
   (its name is also shown at the top of your pull request page, right after
   the word "from").
3. Now navigate to the organisation's folder again, open the `source`
   folder, and click `origin.md`.
4. Click the **pencil icon** (✏️, top right of the file view) to edit.
5. Give each image its own heading and write one bullet per file, labelled
   by its role: the **Original** is the supplied file in the `source`
   folder (say who supplied it, and when); the **Served file** is the file
   in the organisation's folder itself — the one the website uses. Two
   examples to copy and adapt:

   ```markdown
   # Origin

   ## Favicon

   - **Served file** (`favicon.ico`) — generated 2026-06-03 from the logo
     on [epe.nl](https://www.epe.nl/).

   ## Jumbotron

   - **Original** (`source/jumbotron.jpg`) — supplied 2026-06-03 by the
     municipality of Epe.
   - **Served file** (`jumbotron.webp`) — converted by Conduction from the
     original.
   ```

   or, when the banner is a temporary placeholder from the organisation's
   own website:

   ```markdown
   ## Jumbotron

   - **Served file** (`jumbotron.jpg`) — placeholder taken 2026-06-03 from
     [epe.nl](https://www.epe.nl/), pending an image supplied by the
     organisation.
   ```

6. Click the green **Commit changes...** button. In the window that opens,
   choose **"Commit directly to the `<your-branch-name>` branch"** (this is
   safe — it is your own proposal branch, not `main`) and click **Commit
   changes**. Your pull request updates automatically.

## 5. Ask for a review

Open your pull request (the tab from step 3, or find it under the **Pull
requests** tab at the top of the repository). Check under **Files changed**
that you see your images and the updated `origin.md`. Then ask a colleague
at Conduction to review and merge it. After the merge, the images are live
within about five minutes — for a *new* organisation nothing changes on any
website yet, because no website points to the new files until its
configuration is updated.

## Creating a brand-new folder

GitHub cannot create an empty folder — a folder only exists once there is a
file in it. So we create the folder and its `origin.md` in one go:

1. Open the category folder (for example `municipalities`).
2. Click **Add file → Create new file**.
3. In the file-name field, type the organisation's folder name, then a `/`,
   then `source`, then a `/`, then `origin.md` — like this:

   ```
   gooise-meren/source/origin.md
   ```

   Each time you type a `/`, GitHub turns what you typed into a folder.
4. In the big text area, write the origin lines as shown in step 4 above
   (you can also leave it empty for now and fill it in during step 4).
5. Click **Commit changes...**, choose **"Create a new branch for this
   commit and start a pull request"**, then **Propose changes** and
   **Create pull request**.
6. Continue with step 3 (Upload the images) — but remember to first switch
   from **main** to **your branch** with the dropdown, as described in
   step 4.2, so everything lands in the same proposal. When you upload while
   on your own branch, the Commit-changes box offers **"Commit directly to
   the `<your-branch-name>` branch"** — that is the right choice here.

## Replacing images of an existing website — extra care

Uploading a file with the same name as an existing one **overwrites** it,
and once merged the live website changes within minutes. That is fine — it
is how images get updated — but for a site that is already live, always have
someone look at your pull request who can judge the change, and never
**delete or rename** a folder or file: a live website would instantly lose
its images.
