**Fix implementation:** [CanteenPowered/tf2-mvm-patches (fix-refill-energy-weps)](https://github.com/CanteenPowered/tf2-mvm-patches/tree/fix-refill-energy-weps)

**Fix author:** [`Hunter K. (HK)`](http://steamcommunity.com/profiles/76561199210592230)

## The issue

"Refill clips and ammo" canteens don't work with weapons that use energy instead of primary or secondary ammo, such as The Cow Mangler 5000 or The Pomson 6000.

## Suggested fix

A potential fix for this issue would be to call `CTFWeaponBase::WeaponRegenerate()` if the weapon uses energy.

The achievement "Shell Extension" would also need to be triggered if the weapon uses energy and has no stored energy.

In `CTFPowerupBottle::ReapplyProvision()`:
```diff
 	// Refill weapon clips
 	for ( int i = 0; i < MAX_WEAPONS; i++ )
 	{
-		CBaseCombatWeapon *pWeapon = pTFPlayer->GetWeapon(i);
+		CTFWeaponBase *pWeapon = dynamic_cast< CTFWeaponBase* >( pTFPlayer->GetWeapon(i) );
 		if ( !pWeapon )
 			continue;
 
 		// ACHIEVEMENT_TF_MVM_USE_AMMO_BOTTLE
 		if ( TFGameRules() && TFGameRules()->IsMannVsMachineMode() )
 		{
 			if ( ( pWeapon->UsesPrimaryAmmo() && !pWeapon->HasPrimaryAmmo() ) ||
-				( pWeapon->UsesSecondaryAmmo() && !pWeapon->HasSecondaryAmmo() ) )
+				( pWeapon->UsesSecondaryAmmo() && !pWeapon->HasSecondaryAmmo() ) ||
+				( pWeapon->IsEnergyWeapon() && !pWeapon->Energy_HasEnergy() ))
 			{
 				pTFPlayer->AwardAchievement( ACHIEVEMENT_TF_MVM_USE_AMMO_BOTTLE ); 
 			}
 		}
 
 		pWeapon->GiveDefaultAmmo();
 
+		if ( pWeapon->IsEnergyWeapon() )
+		{
+			pWeapon->WeaponRegenerate();
+		}
+
 		if ( iShareBottle && pHealTarget )
 		{
-			CBaseCombatWeapon *pPatientWeapon = pHealTarget->GetWeapon(i);
+			CTFWeaponBase *pPatientWeapon = dynamic_cast< CTFWeaponBase* >( pHealTarget->GetWeapon(i) );
 			if ( !pPatientWeapon )
 				continue;
 
 				pPatientWeapon->GiveDefaultAmmo();
+
+				if ( pPatientWeapon->IsEnergyWeapon() )
+				{
+					pPatientWeapon->WeaponRegenerate();
+				}
+
 				bBottleShared = true;
 			}
 		}
```
