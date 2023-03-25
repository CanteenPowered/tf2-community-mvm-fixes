**Relevant GitHub issue:** https://github.com/ValveSoftware/Source-1-Games/issues/4794

**Fix implementation:** [CanteenPowered/tf2-mvm-patches (fix-tfbot-spy-tease)](https://github.com/CanteenPowered/tf2-mvm-patches/tree/fix-tfbot-spy-tease)

**Fix author:** [`Hunter K. (HK)`](http://steamcommunity.com/profiles/76561199210592230)

## The issue

Spybots in the NextBot action `CTFBotSpyHide` will always use human voice lines, even in MvM.

## Suggested fix

A potential fix for this issue would be to change `CTFBotSpyHide` to use the MvM soundscript if Mann vs. Machine mode is active.

In `CTFBotSpyHide::Update()`:
```diff
 if ( m_talkTimer.IsElapsed() )
 {
 	m_talkTimer.Start( RandomFloat( 5.0f, 10.0f ) );
-	me->EmitSound( "Spy.TeaseVictim" );
+	if ( TFGameRules() && TFGameRules()->IsMannVsMachineMode() )
+	{
+		me->EmitSound( "Spy.MVM_TeaseVictim" );
+	}
+	else
+	{
+		me->EmitSound( "Spy.TeaseVictim" );
+	}
 }
```

and in `CTFPlayer::PrecacheMvM()`:
```diff
 	PrecacheScriptSound( "Weapon_Upgrade.ExplosiveHeadshot" );
 	PrecacheScriptSound( "Spy.MVM_Chuckle" );
+	PrecacheScriptSound( "Spy.MVM_TeaseVictim" );
 	PrecacheScriptSound( "MVM.Robot_Engineer_Spawn" );
 	PrecacheScriptSound( "MVM.Robot_Teleporter_Deliver" );
```

The soundscript Spy.MVM_TeaseVictim also references a few audio files by the wrong name, so that would need to be fixed as well.

In `scripts/game_sounds_vo_mvm_handmade.txt:`

```diff
"Spy.MVM_TeaseVictim"
{
	"channel"		"CHAN_VOICE"
	"volume"		"1"
	"pitch"			"PITCH_NORM"
	"soundlevel"	"SNDLVL_95dB"
	"rndwave"
	{
		"wave"		"vo/mvm/norm/spy_mvm_cloakedspy03.mp3"
		"wave"		"vo/mvm/norm/spy_mvm_cloakedspy04.mp3"
		"wave"		"vo/mvm/norm/spy_mvm_Revenge01.mp3"
		"wave"		"vo/mvm/norm/spy_mvm_Revenge02.mp3"
		"wave"		"vo/mvm/norm/spy_mvm_Revenge03.mp3"
-		"wave"		"vo/mvm/norm/spy_mvm_specialcomplete04.mp3"
-		"wave"		"vo/mvm/norm/spy_mvm_specialcomplete10.mp3"
-		"wave"		"vo/mvm/norm/spy_mvm_specialcomplete12.mp3"
+		"wave"		"vo/mvm/norm/spy_mvm_specialcompleted04.mp3"
+		"wave"		"vo/mvm/norm/spy_mvm_specialcompleted10.mp3"
+		"wave"		"vo/mvm/norm/spy_mvm_specialcompleted12.mp3"
	}
}
```
