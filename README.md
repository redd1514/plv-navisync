
  # Build PLV NaviSync App

  This is a code bundle for Build PLV NaviSync App. The original project is available at https://www.figma.com/design/JKsrOJBLNqkdNSv3u8702j/Build-PLV-NaviSync-App.

  ## Running the code

  Run `pnpm install` to install the dependencies.

  Run `pnpm dev` to start the development server.

## Local Developer Diagnostics unlock

Developer Diagnostics remains Admin/Super Admin-only in production. In local development, Student/Guest map users can open the unlock dialog with **Ctrl+Shift+D** (Windows/Linux) or **Cmd+Shift+D** (macOS). The unlock is provided only by a loopback-only Vite development middleware and does not create an application account or grant database permissions.

To configure or rotate local access, run `node scripts/configureLocalDiagnostics.mjs` in an interactive terminal. Enter the developer ID and password when prompted; password entry is hidden. The script stores only a scrypt password hash in the gitignored `.env.diagnostics.local` file. Restart `pnpm dev` after configuration. The temporary demo password should not be used for a public deployment. No production Developer Unlock endpoint is implemented; continue using Admin/Super Admin access in production until a trusted production authentication service is deliberately configured.

## Theming note (dynamic branding + dark mode)

- **Light mode** — the brand colors (`--app-primary`, `--app-accent`, `--app-background`) come from Firestore as raw values and are applied verbatim.
- **Dark mode** — the primary color is automatically lightened (hue-preserving, via `src/lib/color.ts`) so brand-colored text/icons always hold **≥ 4.5:1** contrast against dark backgrounds. Colors that already pass are left untouched.
- **Admin settings** still shows the raw, original color — only the live CSS variable gets the dark-mode adjustment.

## Upstream fork sync automation

This repository includes `.github/workflows/sync-upstream.yml` to sync `main` from `nitsuwa/plv-navisync` once per day (and on manual `workflow_dispatch`).

One-time setup: ensure repository **Settings → Actions → General → Workflow permissions** allows `Read and write permissions` for `GITHUB_TOKEN`, otherwise sync updates will be denied.

The workflow uses GitHub's built-in fork synchronization API (`merge-upstream`) and will fail clearly when synchronization is blocked (for example, branch protection restrictions or diverged history). It does not force-push or discard fork-only commits.
