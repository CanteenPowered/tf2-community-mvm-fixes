**Relevant GitHub issue:** https://github.com/ValveSoftware/Source-1-Games/issues/3984

**Fix implementation:** [CanteenPowered/tf2-mvm-patches (fix-arrow-collision)](https://github.com/CanteenPowered/tf2-mvm-patches/tree/fix-arrow-collision)

**Fix author:** [`Hunter K. (HK)`](http://steamcommunity.com/profiles/76561199210592230)

## The issue

Arrow-based entities with the "projectile penetration" upgrade will immediately collide with their owner if the player is ubercharged.

https://user-images.githubusercontent.com/91440203/207964367-2a6a45b9-c8c3-4fcc-bef0-2553141c8a99.mp4

## Suggested fix

A potential fix for this issue would be to check if the target is the owning player when testing collision.

In `CTFProjectile_Arrow::StrikeTarget()`:
```diff
 	// Block and break on invulnerable players
 	CTFPlayer *pTFPlayerOther = ToTFPlayer( pOther );
 	if ( pTFPlayerOther && pTFPlayerOther->m_Shared.IsInvulnerable() )
-		return false;
+	{
+		// Don't break on our owner
+		CBaseEntity* pOwner = GetOwnerEntity();
+		if ( !pOwner || pOwner->entindex() != pOther->entindex() || pOwner->GetTeamNumber() != GetTeamNumber() )
+			return false;
+	}
```
