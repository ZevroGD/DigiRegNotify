# Register+

An unofficial Android client for [Digitales Register](https://digitalesregister.it). It signs
in with your own account, checks in the background, and notifies you about new homework, marks,
messages and timetable changes.

## Install

1. On GitHub, open this repository's [Releases page](https://github.com/ZevroGD/DigiRegNotify/releases) and
   pick the **latest** release.
2. Under **Assets**, download the APK for your device to your phone.
3. Open the downloaded file. Android will ask you to allow **Install unknown apps** for the app
   you opened it with (Chrome, Files, …); allow it once, then tap **Install**.
4. Updates work the same way: install a newer APK over the old one. Your login and settings
   are kept, because every build is signed with the same key.

The release text lists what changed in that version. The version shows at the top of
**Settings → Developer menu → Status**.

## First start

1. Enter your Digitales Register username and password and choose how often to check
   (15 minutes to 24 hours, default 2 hours). Tap **Sign in**.
2. Allow **notifications** when Android asks.
3. On Android 12 and newer you are sent to a settings page to allow **exact alarms**. Allow it,
   otherwise checks still run but may arrive late.
4. Optionally tap **Run a check now** to confirm that login works.

## Keep background checks alive (important on Xiaomi and similar phones)

Some phone makers stop background apps overnight. If checks seem to stop, set this for
Register+ in the phone's app settings:

- **Autostart**: on
- **Battery saver / battery usage**: *No restrictions*
- In the recent-apps view, **lock** the app (long-press its card → Lock)

**Settings → Developer menu → Status** shows when the last check and the last *background* check ran. If the background
one is old, one of the settings above is usually the reason.

## What it does

- **Home**: upcoming homework and tests by day, tick them off (synced with the website),
  search, filter by category, unread messages. Pull down to refresh.
- **Attachments**: a 📎 on homework with a description, link or file; messages list theirs in the popup.
- **Edit and delete markers**: an ⓘ on edited homework shows before and after; deleted homework is
  marked on its due day.
- **History**: the past month of homework, marks and teacher notes.
- **Marks**: chart and per-subject averages; tap a subject for its exact marks. Tap the chart to open
  it full screen and drag along it to see each day's marks.
- **Timetable**: week view with lesson details.
- **Notifications**: new homework, new marks, new messages and changed timetable hours (with before and after). Each kind
  can be switched off in Settings.
- **Themes**: several colour palettes, a Custom theme (pick any two colours on a colour wheel or with a hex code), AMOLED black, and
  your own names and colours for subjects.

## Updates from inside the app

The app can find and install new releases for you, so you don't have to open GitHub. The
repository is public and built in, so there is nothing to set up.

Optional: a GitHub token raises GitHub's request limit, which only matters if checks report
that the limit is reached. In **Settings → Updates → GitHub access**, tap **Create token on GitHub**,
generate a token (it needs no permissions) and paste it into the field.

How it works:

- With **Check automatically** on, the app looks for a new release about twice a day (during its
  normal background check) and sends one notification per new version.
- Tapping the notification, or simply opening the app, shows a popup with the version, the release
  notes and a **Download** button (at most about twice a day, so it doesn't nag). **Later** closes it.
- **Check for update** in Settings does the same check on demand.
- When the download finishes, the APK is opened with the installer chosen under
  **Settings → Developer menu → Update installer**. The list shows apps that offer to take an APK;
  if yours isn't in it, choose *Enter package name…* and type its package name. If none is chosen
  (or the chosen app can't take the file) it uses the system installer, which first asks once to
  allow installing from Register+.
- Nothing is ever installed without you pressing the install button.

The token, if you add one, is kept encrypted on the phone. Revoke it on GitHub if you lose the
phone. Tokens expire (at most after a year); if checks fail with "rejected the token", make a
new one or remove it under **GitHub access**.

## If something goes wrong

Open **Settings → Developer menu**:

- **Status** shows whether you are logged in, the alarm is queued, notifications and exact
  alarms are allowed, and when the last checks ran.
- **Log** records requests, alarms, checks and notifications (no passwords or cookies). It
  keeps the newest ~200 KB and can be switched off.
- **Crash reports** keep the last 10 crashes with their full error text.
- **Share full report** sends status, log and the latest crash as text.

## For the maintainer: building and releasing

Builds run on [GitHub Actions](https://github.com/ZevroGD/DigiRegNotify/actions), no Android Studio is needed.

1. Replace [DigiRegNotify.zip](https://github.com/ZevroGD/DigiRegNotify/blob/main/DigiRegNotify.zip) in the repository with the new zip and commit it to `main`.
   Only the zip starts a build; editing the README or the workflow does not.
2. The [Build APK workflow](https://github.com/ZevroGD/DigiRegNotify/actions/workflows/build.yml) builds the app. The APK is attached to the run under **Actions**.
3. To publish a **release** from that same run, put `[release]` in the commit message when you
   upload the zip (for example `1.3 [release]`). It creates a release `v<version>` with the APK
   and the text of `RELEASE_NOTES.md` (which is inside the zip). Without `[release]` you get a
   plain build only.
   (Alternative: **Actions → Build APK → Run workflow** with **Publish as a GitHub release**
   ticked. That starts a second build, so it uses up another build number.)

**Version numbers**: set in `gradle.properties` inside the zip.

- `appVersionBase` is the first two numbers (`1.3`).
- The last number is the GitHub run number minus `appBuildOffset`, so a new series starts at
  `x.y.0`. When you change `appVersionBase`, set `appBuildOffset` to the run number the next
  build will get (the last run's `#N`, plus one).

The workflow file in the repository ([.github/workflows/build.yml](https://github.com/ZevroGD/DigiRegNotify/blob/main/.github/workflows/build.yml)) is the one GitHub runs; the copy
inside the zip is only a reference.

## Notes and limits

- The Custom theme: Android themes can't take colours chosen while the app runs, so the app has 1,728
  prepared colours (12 levels per red, green and blue channel) and uses the one closest to what you
  pick, never more than about 20 steps away. The picker keeps and previews your exact pick; the
  snapping happens only inside the theme.
- If the school changes its website or API, parts of the app can stop working until the
  requests are captured again.
- It is a debug build, signed with the project's fixed key so that updates install over each other.
