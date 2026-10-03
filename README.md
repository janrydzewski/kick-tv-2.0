# Kick TV 2.0

Kick TV 2.0 plays Kick on a Samsung TV. Pick a live channel from the shelves, search for someone, or open a public archive, then use the remote. No Kick account is required.

The television app lives in `app/`. It is a Tizen package: HTML, CSS, and JavaScript, with Samsung AVPlay drawing the video. The page around the picture is the home shelves, search, the channel page, chat, and the playback controls. Live chat can sit on the right. While it is open, the picture scales into the remaining 16:9 area. Archives have a progress bar and remote seek, and they do not open chat.

The interface is Polish when the TV language is Polish, and English otherwise.

![Kick TV 2.0 icon](app/kick-icon-117.png)

The application id is `JrKickTv20.KickTV20`. In the Apps2Samsung list the name is **Kick TV 2.0**. The catalog entry named KickTV is a different application.

## What you can do

- Browse live streams, recently watched channels, and categories.
- Search by name. The Search button runs the query. A result opens the channel or the category.
- Watch a live stream, or play a public archive from the channel page.
- Change quality while something is playing. The last choice is remembered. The default is 480p.
- Open live chat with OK. OK again returns the picture to fullscreen.
- Chat can show Kick emotes and 7TV emotes when those services return them.
- Seek an archive by holding Left or Right. A short press skips about 10 seconds. A longer hold skips further.
- Leave a screen with Back. Back closes search, the channel page, and an open category row before it exits the app.

Private or deleted archives are not listed.

## Install with Apps2Samsung

You need Developer Mode on the TV and [Apps2Samsung](https://github.com/Apps2Samsung/Apps2Samsung/releases) on a computer or phone. The TV and that computer have to be on the same local network. Apps2Samsung downloads `KickTV20.wgt` from the GitHub Release, signs it for your set, and installs it.

Developer Mode has to stay on whenever you install or update the app.

The **Kick TV 2.0** row appears in Apps2Samsung after two things exist: a public GitHub Release of this repository that contains `KickTV20.wgt`, and a merged package file in [tizen-community-packages](https://github.com/Apps2Samsung/tizen-community-packages). Until that listing is live, use [Install from a WGT file](#install-from-a-wgt-file).

### 1. Turn on Developer Mode

1. On the TV, open **Apps**.
2. Open the number pad with the `123` button and enter `12345`.
3. Turn **Developer Mode** on.
4. If the TV asks for an IP address, enter the local IP of the computer or phone that will run Apps2Samsung.
5. Restart the TV. On most sets, hold Power until the Samsung logo appears.

### 2. Install from the catalog

1. Download Apps2Samsung for Windows, macOS, Linux, or Android from its [releases](https://github.com/Apps2Samsung/Apps2Samsung/releases).
2. Start it and select the TV. If the TV is missing, refresh the list or type its IP address.
3. In the **Release** list, select **Kick TV 2.0**.
4. Click **Download & Install**. Sign in to Samsung if Apps2Samsung asks.

The app shows up on the TV as **Kick TV 2.0**.

### 3. Put the app in the catalog

This repository already publishes the package. Pushing a tag `vX.Y.Z` that matches `app/config.xml` runs `.github/workflows/release.yml` and attaches `KickTV20.wgt` to a GitHub Release.

The catalog entry is `apps2samsung/package.json`. Before you open the pull request, set `repo` to your public GitHub repository, `owner/name`. Copy that file to the community repo as `packages/<owner>__<name>.json`. The file name uses a double underscore. Open a pull request against [Apps2Samsung/tizen-community-packages](https://github.com/Apps2Samsung/tizen-community-packages). Their bot checks the JSON. After it is merged, the next catalog build can download the Release asset.

The Release has to exist first. A catalog sync cannot fetch a package that was never published.

### Updating

Select **Kick TV 2.0** in Apps2Samsung and run **Download & Install** again. Recent channels and the saved quality normally stay on the TV, because the application id does not change.

If Tizen reports an author certificate mismatch, remove Kick TV 2.0 from the TV and install it again. Tizen will not overwrite an application id with a package signed by a different author certificate.

### If installation fails

**The TV never shows up in Apps2Samsung**

- Developer Mode is on, and the TV was fully restarted after you enabled it.
- The IP stored in Developer Mode is the machine running Apps2Samsung.
- Both devices are on the same LAN. A VPN on the computer can hide the TV.

**Kick TV 2.0 is not in the Release list**

The catalog entry is not merged yet, or this repository has no Release with `KickTV20.wgt`. Use the WGT file method below.

**Certificate or signature error**

Uninstall Kick TV 2.0, then install it again.

**The app opens, but a stream never starts**

- The channel is live, or you opened a public archive rather than an empty channel.
- The TV can reach `kick.com`, `files.kick.com`, `images.kick.com`, `7tv.io`, and `cdn.7tv.app`.
- Press Back until you are on the home shelves and open the channel again.

## Install from a WGT file

Use this when the catalog entry is not available yet.

```bash
./scripts/build.sh
```

That writes `dist/KickTV20.wgt`. You need `bash`, `node`, `xmllint`, `zip`, and `unzip`. You can also download `KickTV20.wgt` from a GitHub Release of this repository.

In Apps2Samsung, choose **Custom WGT File**, select `KickTV20.wgt`, and install it. Apps2Samsung still signs the file for the TV. The build output is not signed.

Tizen Studio can do the same job: create a Samsung TV certificate profile that includes the television's DUID, sign `dist/KickTV20.wgt`, and install it with Device Manager.

## Remote

### Home

| Key | Action |
| --- | --- |
| Up / Down | Move between the Search button and the shelves |
| Left / Right | Move between tiles on a shelf |
| OK | Open the focused tile, or open search from the Search button |
| Back | Close the category row, then exit the app |

### Search

| Key | Action |
| --- | --- |
| Right | Move from the text field to the Search button |
| Left or Up | Move from the Search button back to the text field |
| Down | Move into the results |
| OK | Run the search, or open the focused result |
| Back | Close search |

Type at least two characters. The Results heading appears when result cards are on screen.

### Channel page

| Key | Action |
| --- | --- |
| Arrows | Move between the live tile and archive tiles |
| OK | Play the focused item |
| Back | Return home |

### Playback

| Key | Action |
| --- | --- |
| OK | Show or hide live chat. The picture scales beside the chat. Archives ignore this key. |
| Up | Open the quality list. Up and Down move in the list, OK applies the choice, Back closes it. |
| Left / Right | On an archive, seek. Hold the key to skip further. |
| Play / Pause | Pause or resume when the TV sends those keys |
| Back | Leave the player. From a channel you opened yourself, Back returns to that channel page. |

The screensaver stays off while something is playing.

## Build

From the repository root:

```bash
./scripts/validate.sh
./scripts/build.sh
```

`validate.sh` checks `app/app.js`, `app/config.xml`, and the CSS braces. `build.sh` runs that check and zips the app into `dist/KickTV20.wgt`. The current version is the `version` attribute in `app/config.xml`. A tag pushed as `v` plus that version publishes the file on GitHub.

The package still has to be signed for the target TV. Apps2Samsung or Tizen Studio does that.

## Requirements

- A Samsung TV with Tizen 4.0 or newer.
- A network connection for playback, search, chat, and emote images.
- AVPlay on the TV. A desktop browser can show the layout, but it cannot play the stream.

## Good to know

This project is not affiliated with or endorsed by Kick, 7TV, Samsung, or Tizen. Those names, logos, and trademarks belong to their owners. Kick and 7TV can change their public endpoints, and search, listings, or chat can break until the app is updated.

## License

Source code is under the [MIT License](LICENSE).

Inspired by [Kick TV](https://github.com/corekill/kick-tv-tizen). Portions of this project include that code under the MIT license.
