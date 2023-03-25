![banner](res/banner.png)

This is a collection of community-created Mann vs. Machine bug fixes. These are **strictly** bug fixes, not balance changes. All fixes are tested in the latest version of TF2, or in [Team Comtress 2](https://github.com/CanteenPowered/tf2-mvm-patches) if the fix requires a code change.

## Fixes

* [Fix bots failing to use items](fixes/tfbot-use-item.md) - Fixes bots sometimes failing to use lunchbox/banner items
* [Fix spybots using human voice lines in MvM](fixes/tfbot-spy-tease.md) - Fixes spies using human voice lines while hiding
* [Fix bomb reset announcer lines triggering in regular CTF](fixes/tfbot-bomb-reset.md) - Fixes "you've driven the bomb carrier back" triggering in regular CTF with bots
* [Fix penetrating arrow collision](fixes/penetrating-arrow-collision.md) - Fixes penetrating projectiles colliding with the owning player when ubercharged
* [Fix refill canteens not refilling energy weapons](fixes/refill-energy-weps.md) - Fixes "refill clips and ammo" canteens not working with energy weapons

## Contributing

If you have an idea for a fix, please leave a comment on the [Fix Requests](https://github.com/CanteenPowered/tf2-community-mvm-fixes/issues/1) issue.

Fix implementations are also welcome. **Please don't submit any fixes without the permission of the original author.** If your fix requires code modification, please write a working fix in [Team Comtress 2](https://github.com/CanteenPowered/tf2-mvm-patches) before submitting. It's also a good idea to reverse-engineer the latest TF2 client/server binaries to make sure your fix will still work in modern TF2.
