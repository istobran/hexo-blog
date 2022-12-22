---
title: 三角洲特种部队 WAC 脚本帮助
tags: [三角洲特种部队, 联合行动, 地图制作]
date: 2022-12-22 11:58:29
urlname: novalogic-wac-script-api-reference
categories: 游戏
---

以前网易博客的旧文重发，本文收集了多篇来自多家论坛上的 WAC 脚本说明
主要适用于联合行动 Joint Operations 和三角洲特种部队：极限版 Delta Force:Xtreme 之后的版本
三角洲特种部队：黑鹰坠落 Delta force: Black Hawk Down 及之前版本会有部分功能不可用，需自行排查

<!--more-->
```html
///////////////////WAC HELP 1（MED）///////////////////

;wac file - nestable IF/THEN/ELSE/ENDIF boolean logic
;
;WAC post directory w:\vp\program\wac - commands listed in GAME.WAC
;WAC debug screen under shift-F12, numlock arrows to select and scroll
;
;GLOBAL WAC is GAME.WAC (executed first)
;
;MISSION WAC is misname.WAC (executed second)
;
;Left to Right order of operations
;
;---WAC LANGUAGE COMMANDS
;nestable flow control
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; IF THEN ELSE ENDIF
;boolean logic
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; AND OR XOR
;function modifier
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; NOT
;comments
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ; /
;
;Syntax - parens optional, line returns and tabbing optional
;
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if trigger1(params) and trigger2(params) then
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; event1(params)
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; event2(params)
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; endif
;
;Example
;
;&nbsp;&nbsp; I want to open doors in group 12 the first time I enter area 1501
;
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if location(1501) and never() then
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; opendoors(12)
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; endif
;

;---VARIABLES &amp; IMMEDIATES (a variable can always be used as an immediate)
;"STRING"&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate string value
;#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate decimal number
;anim_move&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate equate from ADM file
;ammo_name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate ammo name (ex. ammo2tgt ammo_rocket 1)
;fx_fxname&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate effect name (ex. fxrain fx_effect_lightning)
;effect_name&nbsp;&nbsp;&nbsp; immediate effect name (ex. fxrain effect_lightning)
;SS_SoundSet&nbsp;&nbsp;&nbsp; immediate soundset name
;sSoundSet&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate soundset name (alternate syntax)
;face_name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate face name (ex. ssnface 45 face_happy)
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_normal
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_happy
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_sad
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_smirk
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_angry
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_surprise
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_disgust
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_fear
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_aggressive
;
;V#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; V0 to v511 game variables, cleared at start of mission
;G#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; G0 to vG11 global variables, not cleared during link
;M#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Music Script Variable
;result&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; current return/accumulator value (mostly for debug)
;ticks&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; number of seconds into game
;GameOver&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true is game is over
;Win&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if blue team won
;Lose&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if red team won
;wind&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; used by SWING, FLICKER, and particle wind2
;health&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; player's health/hp value
;mana&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; player's mana
;neartype&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the type of the nearest enemy (from items.def dialog)
;neardist&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the distance to the nearest organic
;nearmove&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the anim move of the nearest organic (setable)
;nearid&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the unique dcb/id of the nearest organic
;neartid&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the id of the organic's target or 0 if no target
;nearblind
;nearflying
;nearguard
;nearSSN
;nearWP
;nearGroup
;nearHP

;deadDist&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the nearest corpse info
;DdeadType
;ddeadSSN
;DdeadMove
;DdeadGroup

;bbadDist&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the nearest badguy info (team 2)
;bbadType
;bbadSSN
;bbadMove
;bbadGroup




;---TRIGGERS (# param can be number or variable)
;never()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if current IF has never fired, for one time only events
;elapse(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if past # seconds since current IF activated
;
;-IF Link (these commands fire once only for every fire of linked IF)
;previous&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if previous (same nesting level) IF fired
;chain(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if previous (same nesting level) IF fired # seconds ago
;link(#1,#2)&nbsp;&nbsp;&nbsp; true if IF #1 away from current IF fired #2 seconds ago
;
;-Game Time (seconds of WAC script running)
;past(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if past # seconds into game, keeps firing after #
;before(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if before # seconds into game.&nbsp; keeps firing before #
;ontick(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if ontick #, only fires once
;
;random(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; randomly true 1 in # times, sets RND for additional tests
;location(#)&nbsp;&nbsp;&nbsp; true if you are at that music location
;area(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if within MED area without checking vertical
;area3D(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if within MED 3D area
;outside()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if you are not in a blink box
;waveready()&nbsp;&nbsp;&nbsp; true if no talking going on
;groupdead(#)&nbsp;&nbsp; true if entire group is dead
;groupalive(#)&nbsp; true if anyone in group is alive
;ssnwounded(#)&nbsp; true if ssn is wounded
;ssndead(#)&nbsp;&nbsp;&nbsp;&nbsp; true if ssn is dead
;ssnalive(#)&nbsp;&nbsp;&nbsp; true if ssn is alive
;ssnride(#)&nbsp;&nbsp;&nbsp;&nbsp; true if any organic is standing on SSN
;ssnonssn&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if SSN is standing on SSN
;ssnnearssn&nbsp;&nbsp;&nbsp;&nbsp; true if SSN is near SSN within dist
;ssnlosssn&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if SSN is near SSN within dist and has LOS
;ssnseesssn&nbsp;&nbsp;&nbsp;&nbsp; true if SSN is near SSN within dist and can see it
;meride(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if player is standing on SSN
;meattached(#)&nbsp; true if player is attached to SSN
;medrive(#)&nbsp;&nbsp;&nbsp;&nbsp; true if player is driving SSN
;meongun(#)&nbsp;&nbsp;&nbsp;&nbsp; true if player is on emplaced weapon on SSN
;ssnloc(#,#)&nbsp;&nbsp;&nbsp; true if vehicle or person is in music location
;ssnarea(#,#)&nbsp;&nbsp; true if vehicle or person is in 2D med area #
;ssnarea3D(#,#) true if vehicle or person is in 3D med area #
;dooropen(#)&nbsp;&nbsp;&nbsp; true if group # has door open
;event(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if med event has triggered
;squadevent(#)&nbsp; true if squad event is in que, sets squadSSN to talker
;playerfired(#) true if player has pressed fire and has slot # selected
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1=knife, 2=small arm, 3=main weapon, ect..
;
;---VARIABLE COMPARE
;eq(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #==#
;ne(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #!=#
;lt(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #&lt;#
;gt(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #&gt;#
;le(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #&lt;=#
;ge(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #&gt;=#
;true(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #!=0
;false(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #==0
;
;---VARIABLE MODIFY
;set(var,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set var to #
;add(var,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; add # to var
;sub(var,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; subtract # from var, clamp at 0
;inc(var)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; add 1 to var
;dec(var)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; subtract 1 from var, clamp at 0

;---EVENTS (# param can be number or variable)
;squadclear&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; clears squadevent selected by squadevent(#)
;forceanim(anim)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; forces all organics into anim slot (debug only)
;report("text")&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pop-up debug report window
;report#("text",#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pop-up debug report window with number
;text("text")&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output text to chat - right side
;text#("text",#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output text to chat w/# - right side
;consol("text")&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output text to consol - left side
;consol#("text",#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output text to consol w/# - left side
;
;flash&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; produce a flash of lightning &amp; thunder
;farflash&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; produce a far away flash of lightning &amp; thunder
;quake(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; earthquake for # 10th of a seconds
;
;colorfade(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; seconds for color to transition (zero is normal)
;sun(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets sun rgb&nbsp;&nbsp;&nbsp; ENV override
;sky(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets sky rgb
;ground(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets ground rgb
;ceiling(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets ceiling rgb
;floor(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets floor rgb (inside ground)
;lightning(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the color of the lightning
;cloud(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the cloud color
;gain(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the brightness of the whole scene
;
;fogcolor(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set fogcolor to R,G,B, changes at color fade rate
;fog(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; same as fogcolor
;skyfogcolor(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp; set skyfogcolor to R,G,B
;skyfog(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; same as skyfogcolor
;fogtype(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set fog type 0=fog, 1=haze, 2=haze wall, 3=fog wall
;fogdist(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets fogdist to # meters
;movefog(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; move fogdist to # meters over # seconds
;skyspeed(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the sky movement speed
;skyheight(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the height of the sky
;
;sound(sSSNAME, dist, head)&nbsp; plays soundset at distance(meters) and heading(bangle)
;
;nearwave("wave.wav", dist) plays wave file from the mouth of the nearest enemy with max dist to be heard
;nearanim(anim_move)&nbsp;&nbsp;&nbsp; sets the nearest enemy to ADM move slot
;
;SSNwave(ssn, "wave.wav", dist) plays wave file from the mouth of the ssn with max dist to be heard
;wave("wave.wav", dist) plays wave file from mouth of player
;SSNanim(ssn, anim_move)&nbsp;&nbsp;&nbsp; sets the ssn to ADM move slot
;SSNradio(ssn, "wave.wav")&nbsp; sets the ssn to talk on radio
;
;SSNmax(ssn, maxengage) set max engage distance
;SSNmin(ssn, minengage) set min engage distance
;SSNatt(ssn, maxattack) set max attack distance
;SSNSpawn(ssn, spawn count) set the spawn count, 0=don't respawn
;GroupMax(group, maxengage) set max engage distance
;GroupMin(group, minengage) set min engage distance
;GroupAtt(group, maxattack) set max attack distance
;GroupSpawn(group, spawn count) set the spawn count, 0=don't respawn
;remove(grp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; remove group # without a trace
;kill(grp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; kill group #
;removeSSN(ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; remove SSN # without a trace
;killSSN(ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; kill SSN #
;teleport(grp,tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; teleport group # to target #
;telessn(ssn, tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; teleport SSN # to target #
;targetfx(tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create med particle fx at target #
;sound2tgt(ss,tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create ssoundset at target # (ex. sound2tgt sSoundSet 1)
;ss2ssn(ss,ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create ssoundset at center of ssn
;ammo2tgt(ammo,tgt)&nbsp;&nbsp;&nbsp;&nbsp; create ammo # at target # (ex. ammo2tgt ammo_rocket 1)
;fx2tgt(fx,tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create fx # at target # (ex. fx2tgt effect_lightning 1)
;opendoors(group)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; open doors in group #
;closedoors(group)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; close doors in group #
;SSNtoWP(ssn, wp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; redirect SSN to WP list
;GtoWP(group, wp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; redirect Group to WP list
;ammorain(ammo)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rain down ammo # somewhere near player
;ammoarea(ammo, area)&nbsp;&nbsp; rain down ammo # somewhere inside area #
;fxrain(fx)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rain down effect # somewhere near player
;ssncspd(ssn,speed)&nbsp;&nbsp;&nbsp;&nbsp; set ssn to combat speed of #
;ssnpspd(ssn,speed)&nbsp;&nbsp;&nbsp;&nbsp; set ssn to patrol speed of #
;ssnuse(ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; snap ssn to emplaced weapon if within 3 meters
;ssnrelease(ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; remove ssn from emplaced weapon
;ssn2ssn(ssn, ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; have ssn goto ssn and try to snap to emplaced
;ssnface(ssn, face)&nbsp;&nbsp;&nbsp;&nbsp; set face expression of ssn
;ssnguard(ssn, #)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set ssn to guard 1=ON, 0=Off
;ssnturn(ssn, heading)&nbsp; turn ssn to heading 0-360
;ssnhp(ssn, hp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set ssn's hitpoints to hp
;blockfire(wpnkey, #)&nbsp;&nbsp; blockfiring of weapons under key, 0=fire, 1=block

///////////////////WAC HELP 2（NILE）///////////////////

//PLOOP -&gt; Real Player loop

//- PLOOP actions(player) END

//CODEPLOOP
//if condition
//actions(player)
//end condition
//END


//---------------------------------------------------

//GLOOP -&gt; Named Group Loop

//don't use group commands !

//- GLOOP group actions(item) END

//CODEGLOOP group
//if condition
//actions(item)
//end condition
//END


//------------------------------------------------------

//ENTER -&gt; to handle like 'and never()', but fire again was con. false

//CODEIF trigger ENTER // triggers on first true
//actions
//ENDIF


//------------------------------------------------------

//LEAVE -&gt; fired if condition no more true

//CODEIF trigger LEAVE // triggers after last true
//actions
//ENDIF


//-------------------------------------------------------

//IFNAME -&gt; give a condition a Name


//CODEIF [ifname] triggers THEN // optional named if
//actions
//ENDIF


//---------------------------------------------------------

// WAC Quick Reference Help File

//WAC Flow Control (basic statements)
// IF triggers THEN actions END
// IF [ifname] triggers THEN actions END (optional named if)
// IF triggers THEN actions ELSE actions END
// IF triggers THEN actions ELSEIF triggers THEN actions END
// PLOOP actions(player) END
// GLOOP group actions(item) END
// DOSEQ actions NEXT actions [NEXT actions..] END
// DORND actions NEXT actions [NEXT actions..] END
// IF triggers ENTER actions END (triggers on first true)
// IF triggers LEAVE actions END (triggers after last true)

// WAC Compiler Commands
// VAR variablename (declares a number variable, shows up on debug screen)
// CHEAT cheatname (declares a server cheat variable, shows up on debug screen)
// RUN filename (this includes the text from a file, good from command line)

// WAC Logic and Math (tests for inside IF statement)
// Arithmatics ()+-*/^
// Booleans AND OR NOT &lt; &gt; &lt;= &gt;= == !=
// Example: IF V1&lt;12 THEN
// Example: IF V1&lt;12 AND V2&lt;12 THEN
// Note: this compiles to IF (V1&lt;12) AND (V2&lt;12) THEN

// WAC Assignment Var = Value
// Example: V1 = 12
// Example: V2 = V2+V3*4
// Note: This compiles to V2+(V3*4)

// WAC Triggers (Things to do after IF statement)

// elapse (seconds)
// never ()
// previous ()
// chain (seconds)
// past (seconds)
// before (seconds)
// ontick (seconds)
// groupdead (number)
// groupalive (number)
// dooropen (number)
// SSNcritical (ssn)
// SSNexists (ssn)
// SSNdead (ssn)
// SSNalive (ssn)
// SSNwounded (ssn)
// SSNride (ssn)
// SSNonSSN (ssn, ssn)
// SSNnearSSN (ssn, ssn, distance)
// SSNlosSSN (ssn, ssn, distance)
// SSNseesSSN (ssn, ssn, distance)
// SSNarea (ssn, area)
// SSNarea3D (ssn, area)
// SSNloc (ssn, number)
// SSNLeadSSN2SSN (ssn, ssn, ssn, distance)

// WAC Actions (Things to do after THEN/ENTER/LEAVE statement)

// reset (ifname)
// Gkill (group)
// Gremove (group)
// Gsetaccuracy (number, number, number)
// GtoWP (number, wplist)
// kill (number)
// remove (number)
// teleport (number, target)
// GroupMin (number, distance)
// GroupMax (number, distance)
// GroupAtt (number, distance)
// GroupSpawn (number, number)
// GroupHP (number, number)
// opendoors (number)
// closedoors (number)
// text (text)
// wave (filename)
// hideSSN (ssn)
// unhideSSN (ssn)
// disableSSN (ssn)
// enableSSN (ssn)
// holdSSN (ssn)
// unholdSSN (ssn)
// setaccuracy (ssn, number, number)
// SSNtoWP (ssn, wplist)
// killSSN (ssn)
// removeSSN (ssn)
// teleSSN (ssn, target)
// SSNwave (ssn, filename, distance)
// SSNradio (ssn, filename)
// SS2SSN (soundset, ssn)
// SSNanim (ssn, anim)
// SSNMin (ssn, distance)
// SSNMax (ssn, distance)
// SSNAtt (ssn, distance)
// SSNSpawn (ssn, number)
// SSNHP (ssn, number)
// SSNADDHP (ssn, number)
// ssn2ssn (ssn, ssn)
// ssnrelease (ssn)
// ssnface (ssn, face)
// ssnturn (ssn, heading)
// ssnguard (ssn, number)
// ssnname (ssn, texttoken)
// ssnpspd (ssn, number)
// ssncspd (ssn, number)
// ssnuse (ssn)
// set (variable, value)
// add (variable, value)
// sub (variable, value)
// inc (variable)
// dec (variable)
// store (variable)
// load (value)
// TOD (hour)
// targetfx (target)
// ammo2tgt (ammo, target)
// fx2tgt (fx, target)
// ammoarea (ammo, area)
// sound2tgt (soundset, target)
// flash ()
// farflash ()
// quake (number)
// win (number)
// lose (number)
// music (number)
// skyspeed (number)
// skyheight (number)
// fogtype (number)
// fogdist (distance)
// movefog (distance, seconds)
// rain (number, seconds)
// snow (number, seconds)
// overcast (number, seconds)
// Help ()
// text# (text, number)
// consol (text)
// consol# (text, number)

// WAC Debug Commands (This also contains old stuff)

// sound (soundset, distance, heading)
// forceanim (anim)
// tele (ssn)
// fall ()
// fov (number)
// squadevent (number)
// random (number)
// outside ()
// location (number)
// area (area)
// area3D (area)
// waveready ()
// weaponfired (number)
// event (number)
// meride (ssn)
// meattached (ssn)
// medrive (ssn)
// meongun (ssn)
// ammorain (ammo)
// fxrain (fx)
// lightning (red, green, blue)
// face (face)
// anim (anim)
// sunfade (number, seconds)
// gain (red, green, blue)
// squadclear ()
// blockfire (number, number)
// colorfade (number)
// sun (red, green, blue)
// sky (red, green, blue)
// ground (red, green, blue)
// floor (red, green, blue)
// ceiling (red, green, blue)
// cloud (red, green, blue)
// fogcolor (red, green, blue)
// fog (red, green, blue)
// skyfogcolor (red, green, blue)
// skyfog (red, green, blue)
// crash (red, green, blue, green)
// eq (number, number)
// ne (number, number)
// lt (number, number)
// gt (number, number)
// le (number, number)
// ge (number, number)
// true (number)
// false (number) 

///////////////////WAC HELP 3///////////////////
&nbsp;;WAC Text in Color
;
;This WAC code was supplied by Bubbachuk~{PG}~
;
;Here is a short list of the color codes you can use to change
;the color of the WAC scripts: The script has to be enclosed
;between right and left arrows, i.e. &lt;#####&gt; and inserted
;before the line of text in the consol or text displays, i.e.
;
;if never() then
;consol("&lt;#####&gt;Insert color code before actual text. Characters")
;text("&lt;#####&gt;in the code count in the character count allowed")
;text("&lt;#####&gt;for each line.")
;endif
;
;cfff500 or co01 = Yellow
;co30 = Light Yellow
;co100 = Dark Yellow
;cff5000 = Orange
;cf50000 = Red
;cf500000 or c300004 = Dark Red
;c5000 = Dark Green
;cf500 = Green
;co99 = Light Blue
;cff50 = Turquoise
;cfffff5 = White (Don't see any reason to use this one)
;cr953o3 = Gray
;cffff50 = Tan
;cf or ca = Black
;cf50 or cab = Dark Blue
;cff5 or ca0 = Blue
;ca0014 = Purple
;b = Bold Text
;i = Text with Italics
;u = will Underline Text 

///////////////////WAC HELP 4///////////////////
;Previous()
;chain(SECONDS)
;then
;Elapse(SECONDS)
;Past(SECONDS)
;Before(SECONDS)
;OnTick(SECONDS)
;night()
;eq(VAR,NUMBER)
;ge(VAR,NUMBER)
;le(VAR,NUMBER)
;gt(VAR,NUMBER)
;lt(VAR,NUMBER)
;eq(VAR,VAR)
;ge(VAR,VAR)
;le(VAR,VAR)
;gt(VAR,VAR)
;lt(VAR,VAR)
;ne(VAR,NUMBER)
;ne(VAR,VAR)
;true(VAR)
;false(VAR)
;groupdead(GROUP)
;groupalive(GROUP)
;ssndead(SSN)
;ssnwounded(SSN)
;ssnride(SSN)
;ssnonssn(SSN,SSN)
;ssnnearssn(SSN,SSN,DISTANCE)
;ssnlosssn(SSN,SSN,DISTANCE)
;ssnseesssn(SSN,SSN,DISTANCE)
;ssnarea3d(SSN,AREA)
;ssnalive(SSN)
;NOT ssnalive(SSN)
;ssnnearssn(SSN,TEXT,DISTANCE)
;ssnseesssn(SSN,TEXT,DISTANCE)
;ssncritical(SSN)
;random(NUMBER)
;waveready()
;weaponfired(NUMBER)
;Player (coop/multiplayer mode) TEXT = HUMANS
;true(TEXT)
;false(TEXT)
;ssnride(SSN)
;Player (single player mode) TEXT = 10000
;meride(SSN)
;meattached(SSN)
;medrive(SSN)
;meongun(SSN)
;ssndead(TEX)
;ssnwounded(TEXT)
;ssnonssn(TEXT,SSN)
;ssnnearssn(TEXT,SSN,DISTANCE)
;ssnlosssn(TEXT,SSN,DISTANCE)
;outside()
;GtoWP(GROUP,WAYPOINTLIST)
;Gkill(GROUP)
;Gremove(GROUP)
;GroupMin(GROUP,DISTANCE)
;GroupMax(GROUP,DISTANCE)
;GroupAtt(GROUP,DISTANCE)
;GroupSpawn(GROUP,NUMBER)
;GroupHP(GROUP,NUMBER)
;Gsetaccuracy(GROUP,NUMBER,NUMBER)
;teleport(GROUP,TARGET)
;SSNtoWP(SSN,WAYPOINTLIST)
;KillSSN(SSN)
;removeSSN(SSN)
;teleSSN(SSN,TARGET)
;ssnWave(SSN,TEXT,DISTANCE)
;ssncspd(SSN,NUMBER)
;ssnpspd(SSN,NUMBER)
;SSNanim(SSN,ANIM)
;SSNMin(SSN,DISTANCE)
;SSNMax(SSN,DISTANCE)
;SSNAtt(SSN,DISTANCE)
;SSNSpawn(SSN,NUMBER)
;SSNHP(SSN,NUMBER)
;ssnUse(SSN)
;ssnRelease(SSN)
;SSN2SSN(SSN,SSN)
;ssnface(SSN,FACE)
;ssnTurn(SSN,HEADING)
;SSNtoWP(SSN,NUMBER)- Waypoint 123,124,125,126,127
;hideSSN(SSN)
;unhideSSN(SSN)
;enableSSN(SSN)
;disableSSN(SSN)
;fov(SSN,NUMBER)
;ssnguard(SSN,NUMBER)
;ssnblind(SSN,NUMBER)
;ssnclimb(SSN,NUMBER)
;setaccuracy(SSN,NUMBER,NUMBER)
;holdSSN(SSN)
;unholdSSN(SSN)
;SSNLeadSSN2SSN(SSN,SSN,SSN)
;ssnaddhp(SSN,NUMBER)
;set(VAR,NUMBER)
;add(VAR,NUMBER)
;sub(VAR,NUMBER)
;inc(VAR)
;dec(VAR)
;tod(TIME)
;ammo2tgt(AMMO,TARGET)
;fx2tgt(FX,TARGET)
;ammorain(AMMO)
;ammoarea(AMMO,AREA)
;fxrain(FX)&lt;/WAC&gt;
;sound2tgt(SOUNDSET,TARGET)
;flash()
;farflash()
;quake(SECONDS)
;fogdist(DISTANCE)
;win(NUMBER)
;rain(NUMBER,NUMBER)
;overcast(NUMBER,NUMBER)
;Wave(TEXT)
;fogtype(NUMBER)- Fog Type (0,1,2,3)
;gain(NUMBER,NUMBER,NUMBER)
;skyspeed(NUMBER)
;snow(NUMBER,NUMBER)
;movefog(DISTANCE,NUMBER)
;wind(NUMBER,SECONDS
;floor(NUMBER,NUMBER,NUMBER)
;ceiling(NUMBER,NUMBER,NUMBER)
;fog(NUMBER,NUMBER,NUMBER)
;accuracyspread = (NUMBER)
;breathtime = (NUMBER)
;fallmps = (NUMBER)
;text#(TEXT,VAR)
;consol(TEXT)
;consol#(TEXT,VAR)
;text(TEXT)
;PLOOP
;GLOOP
;
;
;1. Previous() - Previous conditional was true.
;Example:
;code
;if never enter
;consol("Sending an enemy to stand on a platform")
;ssn2ssn(xxxx,xxxxx)
;endif
;if previous() and never enter
;consol("Once enemy stands on that platform, Will send his buddy to a waypoint!")
;SSNtoWP(XXXX,X)
;endif
;
;2. chain() - Previous conditional was true and One seconds have elapsed.
;Example:
;code:
;if past(30) and never enter
;consol("Will rain in thirty seconds at 60%!")
;rain(60)
;endif
;if chain(30) enter
;consol("Thirty seconds later dark clouds will form overhead at 90%!")
;overcast(90)
;endif

;3. then - Means end of conditional. You can also use enter instead of then. if you choose to use enter replacing then use it throughout the entire wac script your working on. Don't mismatch then and enter either one or the other.
;Example:
;if never() enter
;consol("Enemy will follow the closest player!")
;ssn2ssn(xxxx,player)
;endif
;
;4. Elapse() - One seconds have elapsed since conditional was last true. Fires continually
;Example
;code:
;if elapse(40) enter
;consol("Every forty seconds their will be an explosion!")
;killSSN(XXXXX)
;endif
;
;
;5. past() - At least one seconds since the mission started before this firers.
;Example
;CODE:
;if past(50) and never enter
;consol("Your seeing this message because fifty seconds has pass!")
;endif
;
;
;6. Before - How many seconds since mission started
;Example
;CODE:
;if Before(3) and OnTick(3) and never enter
;consol("Setting up v1 to go to zero reguardless of humans")
;set(v1,0)
;endif
;if chain(3) enter
;consol("Now I Know v1 equals zero because their is humans!")
;v1 = = 0
;endif
;
;7. OnTick exactly one seconds since the mission started
;Example:
;CODE
;if OnTick(180) enter
;consol#("TIME NOW ",CurTOD)
;consol#("Time Update Will Be Every Three minutes")
;endif
;
;8. night() Night falls between these hours (19:00 - 6:00). Even know I knew this one did not work at the beginning, still wanted to test it anyway. Does not work. You can set-up the time rate and environment file using nile map editor. But
the wac script for night does not work. You can use NLH under terrain to change the environment files also. The lowest rate for time change is one hour = 24hour period.
;
;9. var = = Num or eq(VAR,NUMBER) First before I give the code below let me explain what is I'm doing here. I' am setting up a variable this case it's going to be v1. Next I will set up the number of items I want to keep track of, this case
it's three. Using the VAR to subtract the number of deaths.
;EXAMPLE:
;CODE
;if never enter
;set(v1,3)
;endif
;if eq(v1,0) and never enter
;consol("Each time an item is destroyed subtract intil it reaches zero!)
;endif
;if ssndead(xxxxx) and never enter
;dec(v1)
;endif
;if ssndead(xxxxx) and never enter
;dec(v1)
;endif
;if ssndead(xxxxx) and never enter
;;dec(v1)
;;endif
```

