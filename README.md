# nog-cli

List [rdaily.live](https://rdaily.live) sports matches, fuzzy-pick one, and
resolve/open (or play) its broadcast stream — all from the terminal.

`nog-cli` is a single self-contained Bash script. It fetches the Flashscore day
feed, lets you fuzzy-pick a live/upcoming match, resolves the broadcast token to
a direct HLS (`.m3u8`) stream, and plays it. The streams are PNG-obfuscated
(each MPEG-TS segment is wrapped in a fake PNG), so playback de-wraps segments on
the fly and pipes clean MPEG-TS into your player.

## Requirements

- `bash`, `curl`
- `python3` (used for on-the-fly stream de-obfuscation when playing)
- A player: `mpv` or `vlc` (on WSL, Windows VLC is used automatically for GPU
  decoding when available)
- Optional menu program for picking: `fzf`, `rofi`, or `dmenu` (falls back to a
  built-in numbered menu)

## Install

```sh
git clone git@github.com:Petarper/nog-cli.git
cd nog-cli
chmod +x nog-cli
# optional: put it on your PATH
ln -s "$PWD/nog-cli" ~/.local/bin/nog-cli
```

## Usage

```sh
nog-cli                           # today's live/within-1hr football -> plays it
nog-cli nasaf                     # fuzzy 'nasaf', play it
nog-cli -u nasaf                  # print the .m3u8 URL instead of playing
nog-cli -m basketball football f1 # several sports at once
nog-cli -s tennis -d 1 -a         # all of tomorrow's tennis
```

By default `nog-cli` plays the chosen match. Use `-u`/`--url` to just print the
direct stream URL, or `-o`/`--open` to open the player page in your browser.

### Options

| Option | Description |
| --- | --- |
| `-m, --sports NAMES` | Sport(s) to list, space-separated (default: `football`) |
| `-d, --day N` | Day offset from today (`0`=today, `1`=tomorrow, `-1`=yesterday) |
| `-w, --window SECS` | Only show matches live or starting within SECS (default: 3600) |
| `-a, --all` | Show all matches for the day (same as `-w 0`) |
| `-F, --no-fuzzy` | Use plain substring matching instead of fuzzy |
| `-p, --play` | Resolve to the direct HLS stream and play it (**default**) |
| `-u, --url` | Don't play; just print the direct `.m3u8` URL |
| `--player P` | Force player backend: `mpv`, `vlc`, or a stdin-reading command |
| `-o, --open` | Open the player page in `$BROWSER` / `xdg-open` |
| `-l, --list` | Just list matches, don't resolve a stream |
| `-v, --version` | Show version |
| `-h, --help` | Show help |

Known sports: `football tennis basketball hockey nfl handball futsal volleyball
darts snooker boxing mma motorsport` (alias `f1`) `winter-sports`.

### Environment variables

| Var | Purpose |
| --- | --- |
| `MENU` | Force a menu program (`fzf`, `rofi`, `dmenu`, or any command) |
| `BROWSER` | Browser command used by `--open` |
| `NOG_PLAYER` | Default backend (`mpv` \| `vlc` \| custom command) |
| `NOG_MPV_ARGS` | Extra args appended to the `mpv` command |
| `NOG_VLC_ARGS` | Extra args appended to the `vlc` command |

## License

[MIT](LICENSE)
