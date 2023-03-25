**Fix implementation:** [CanteenPowered/tf2-mvm-patches (fix-tfbot-use-item)](https://github.com/CanteenPowered/tf2-mvm-patches/tree/fix-tfbot-use-item)

**Fix author:** [`Hunter K. (HK)`](http://steamcommunity.com/profiles/76561199210592230)

## The issue

The NextBot action CTFBotUseItem will often finish early without actually using any items. This is because the action doesn't wait for the weapon to fully deploy before running +attack and yielding.

This is most notable on wave 2 of the MvM mission Bavarian Botbash, where the squad of three giant soldiers will often stall the game because at least one will be unable to activate its banner.

## Suggested fix

A potential fix for this issue would be to check if the weapon has finished deploying before attempting to use it.

In `CTFBotUseItem::Update()`:
```diff
 if ( m_cooldownTimer.IsElapsed() )
 {
-	// use it
-	me->PressFireButton();
-	m_cooldownTimer.Invalidate();
+	// Wait for the weapon to finish deploying before trying to use it.
+	if ( me->GetNextAttack() <= gpGlobals->curtime )
+	{
+		// use it
+		me->PressFireButton();
+		m_cooldownTimer.Invalidate();
+	}
 }
```
