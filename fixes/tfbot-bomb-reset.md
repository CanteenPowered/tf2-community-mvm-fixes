**Relevant GitHub issue:** https://github.com/ValveSoftware/Source-1-Games/issues/715

**Fix implementation:** [CanteenPowered/tf2-mvm-patches (fix-tfbot-bomb-reset)](https://github.com/CanteenPowered/tf2-mvm-patches/tree/fix-tfbot-bomb-reset)

**Fix author:** [`Hunter K. (HK)`](http://steamcommunity.com/profiles/76561199210592230)

## The issue

The Mann vs. Machine announcer voice line for knocking the bomb carrier back can play in regular CTF when a TFBot carrying the flag re-routes its path to the capture zone.

This can be reproduced by airblasting a bot carrying the bomb into the moat on 2fort.

## Suggested fix

A potential fix would be to add a check to see if Mann vs. Machine mode is active before checking if the bomb carrier has been knocked back.

In `CTFBotDeliverFlag::Update()`:
```diff
 	m_flTotalTravelDistance = NavAreaTravelDistance( me->GetLastKnownArea(), TheNavMesh->GetNavArea( zone->WorldSpaceCenter() ), cost );
 
-	if ( flOldTravelDistance != -1.0f && m_flTotalTravelDistance - flOldTravelDistance > 2000.0f )
+	if ( TFGameRules()->IsMannVsMachineMode() && flOldTravelDistance != -1.0f && m_flTotalTravelDistance - flOldTravelDistance > 2000.0f )
 	{
 	  TFGameRules()->BroadcastSound( 255, "Announcer.MVM_Bomb_Reset" );
```
