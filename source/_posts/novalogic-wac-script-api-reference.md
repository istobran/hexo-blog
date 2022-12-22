---
title: 三角洲特种部队 WAC 脚本帮助
tags: [三角洲特种部队, 联合行动, 地图制作]
date: 2022-12-22 11:58:29
urlname: novalogic-wac-script-api-reference
categories: 游戏
---

<br />
///////////////////WAC HELP 1（MED）///////////////////<br />
<br />
;wac file - nestable IF/THEN/ELSE/ENDIF boolean logic<br />
;<br />
;WAC post directory w:\vp\program\wac - commands listed in GAME.WAC<br />
;WAC debug screen under shift-F12, numlock arrows to select and scroll<br />
;<br />
;GLOBAL WAC is GAME.WAC (executed first)<br />
;<br />
;MISSION WAC is misname.WAC (executed second)<br />
;<br />
;Left to Right order of operations<br />
;<br />
<!--more-->
;---WAC LANGUAGE COMMANDS<br />
;nestable flow control<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; IF THEN ELSE ENDIF<br />
;boolean logic<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; AND OR XOR<br />
;function modifier<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; NOT<br />
;comments<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ; /<br />
;<br />
;Syntax - parens optional, line returns and tabbing optional<br />
;<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if trigger1(params) and trigger2(params) then<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; event1(params)<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; event2(params)<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; endif<br />
;<br />
;Example<br />
;<br />
;&nbsp;&nbsp; I want to open doors in group 12 the first time I enter area 1501<br />
;<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if location(1501) and never() then<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; opendoors(12)<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; endif<br />
;<br />
<br />
;---VARIABLES &amp; IMMEDIATES (a variable can always be used as an immediate)<br />
;"STRING"&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate string value<br />
;#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate decimal number<br />
;anim_move&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate equate from ADM file<br />
;ammo_name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate ammo name (ex. ammo2tgt ammo_rocket 1)<br />
;fx_fxname&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate effect name (ex. fxrain fx_effect_lightning)<br />
;effect_name&nbsp;&nbsp;&nbsp; immediate effect name (ex. fxrain effect_lightning)<br />
;SS_SoundSet&nbsp;&nbsp;&nbsp; immediate soundset name<br />
;sSoundSet&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate soundset name (alternate syntax)<br />
;face_name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; immediate face name (ex. ssnface 45 face_happy)<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_normal<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_happy<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_sad<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_smirk<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_angry<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_surprise<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_disgust<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_fear<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; face_aggressive<br />
;<br />
;V#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; V0 to v511 game variables, cleared at start of mission<br />
;G#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; G0 to vG11 global variables, not cleared during link<br />
;M#&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Music Script Variable<br />
;result&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; current return/accumulator value (mostly for debug)<br />
;ticks&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; number of seconds into game<br />
;GameOver&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true is game is over<br />
;Win&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if blue team won<br />
;Lose&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if red team won<br />
;wind&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; used by SWING, FLICKER, and particle wind2<br />
;health&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; player's health/hp value<br />
;mana&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; player's mana<br />
;neartype&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the type of the nearest enemy (from items.def dialog)<br />
;neardist&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the distance to the nearest organic<br />
;nearmove&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the anim move of the nearest organic (setable)<br />
;nearid&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the unique dcb/id of the nearest organic<br />
;neartid&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the id of the organic's target or 0 if no target<br />
;nearblind<br />
;nearflying<br />
;nearguard<br />
;nearSSN<br />
;nearWP<br />
;nearGroup<br />
;nearHP<br />
<br />
;deadDist&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the nearest corpse info<br />
;DdeadType<br />
;ddeadSSN<br />
;DdeadMove<br />
;DdeadGroup<br />
<br />
;bbadDist&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; the nearest badguy info (team 2)<br />
;bbadType<br />
;bbadSSN<br />
;bbadMove<br />
;bbadGroup<br />
<br />
<br />
<br />
<br />
;---TRIGGERS (# param can be number or variable)<br />
;never()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if current IF has never fired, for one time only events<br />
;elapse(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if past # seconds since current IF activated<br />
;<br />
;-IF Link (these commands fire once only for every fire of linked IF)<br />
;previous&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if previous (same nesting level) IF fired<br />
;chain(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if previous (same nesting level) IF fired # seconds ago<br />
;link(#1,#2)&nbsp;&nbsp;&nbsp; true if IF #1 away from current IF fired #2 seconds ago<br />
;<br />
;-Game Time (seconds of WAC script running)<br />
;past(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if past # seconds into game, keeps firing after #<br />
;before(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if before # seconds into game.&nbsp; keeps firing before #<br />
;ontick(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if ontick #, only fires once<br />
;<br />
;random(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; randomly true 1 in # times, sets RND for additional tests<br />
;location(#)&nbsp;&nbsp;&nbsp; true if you are at that music location<br />
;area(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if within MED area without checking vertical<br />
;area3D(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if within MED 3D area<br />
;outside()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if you are not in a blink box<br />
;waveready()&nbsp;&nbsp;&nbsp; true if no talking going on<br />
;groupdead(#)&nbsp;&nbsp; true if entire group is dead<br />
;groupalive(#)&nbsp; true if anyone in group is alive<br />
;ssnwounded(#)&nbsp; true if ssn is wounded<br />
;ssndead(#)&nbsp;&nbsp;&nbsp;&nbsp; true if ssn is dead<br />
;ssnalive(#)&nbsp;&nbsp;&nbsp; true if ssn is alive<br />
;ssnride(#)&nbsp;&nbsp;&nbsp;&nbsp; true if any organic is standing on SSN<br />
;ssnonssn&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if SSN is standing on SSN<br />
;ssnnearssn&nbsp;&nbsp;&nbsp;&nbsp; true if SSN is near SSN within dist<br />
;ssnlosssn&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if SSN is near SSN within dist and has LOS<br />
;ssnseesssn&nbsp;&nbsp;&nbsp;&nbsp; true if SSN is near SSN within dist and can see it<br />
;meride(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if player is standing on SSN<br />
;meattached(#)&nbsp; true if player is attached to SSN<br />
;medrive(#)&nbsp;&nbsp;&nbsp;&nbsp; true if player is driving SSN<br />
;meongun(#)&nbsp;&nbsp;&nbsp;&nbsp; true if player is on emplaced weapon on SSN<br />
;ssnloc(#,#)&nbsp;&nbsp;&nbsp; true if vehicle or person is in music location<br />
;ssnarea(#,#)&nbsp;&nbsp; true if vehicle or person is in 2D med area #<br />
;ssnarea3D(#,#) true if vehicle or person is in 3D med area #<br />
;dooropen(#)&nbsp;&nbsp;&nbsp; true if group # has door open<br />
;event(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if med event has triggered<br />
;squadevent(#)&nbsp; true if squad event is in que, sets squadSSN to talker<br />
;playerfired(#) true if player has pressed fire and has slot # selected<br />
;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1=knife, 2=small arm, 3=main weapon, ect..<br />
;<br />
;---VARIABLE COMPARE<br />
;eq(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #==#<br />
;ne(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #!=#<br />
;lt(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #&lt;#<br />
;gt(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #&gt;#<br />
;le(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #&lt;=#<br />
;ge(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #&gt;=#<br />
;true(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #!=0<br />
;false(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; true if #==0<br />
;<br />
;---VARIABLE MODIFY<br />
;set(var,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set var to #<br />
;add(var,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; add # to var<br />
;sub(var,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; subtract # from var, clamp at 0<br />
;inc(var)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; add 1 to var<br />
;dec(var)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; subtract 1 from var, clamp at 0<br />
<br />
;---EVENTS (# param can be number or variable)<br />
;squadclear&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; clears squadevent selected by squadevent(#)<br />
;forceanim(anim)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; forces all organics into anim slot (debug only)<br />
;report("text")&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pop-up debug report window<br />
;report#("text",#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pop-up debug report window with number<br />
;text("text")&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output text to chat - right side<br />
;text#("text",#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output text to chat w/# - right side<br />
;consol("text")&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output text to consol - left side<br />
;consol#("text",#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output text to consol w/# - left side<br />
;<br />
;flash&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; produce a flash of lightning &amp; thunder<br />
;farflash&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; produce a far away flash of lightning &amp; thunder<br />
;quake(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; earthquake for # 10th of a seconds<br />
;<br />
;colorfade(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; seconds for color to transition (zero is normal)<br />
;sun(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets sun rgb&nbsp;&nbsp;&nbsp; ENV override<br />
;sky(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets sky rgb<br />
;ground(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets ground rgb<br />
;ceiling(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets ceiling rgb<br />
;floor(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets floor rgb (inside ground)<br />
;lightning(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the color of the lightning<br />
;cloud(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the cloud color<br />
;gain(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the brightness of the whole scene<br />
;<br />
;fogcolor(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set fogcolor to R,G,B, changes at color fade rate<br />
;fog(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; same as fogcolor<br />
;skyfogcolor(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp; set skyfogcolor to R,G,B<br />
;skyfog(#,#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; same as skyfogcolor<br />
;fogtype(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set fog type 0=fog, 1=haze, 2=haze wall, 3=fog wall<br />
;fogdist(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets fogdist to # meters<br />
;movefog(#,#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; move fogdist to # meters over # seconds<br />
;skyspeed(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the sky movement speed<br />
;skyheight(#)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; sets the height of the sky<br />
;<br />
;sound(sSSNAME, dist, head)&nbsp; plays soundset at distance(meters) and heading(bangle)<br />
;<br />
;nearwave("wave.wav", dist) plays wave file from the mouth of the nearest enemy with max dist to be heard<br />
;nearanim(anim_move)&nbsp;&nbsp;&nbsp; sets the nearest enemy to ADM move slot<br />
;<br />
;SSNwave(ssn, "wave.wav", dist) plays wave file from the mouth of the ssn with max dist to be heard<br />
;wave("wave.wav", dist) plays wave file from mouth of player<br />
;SSNanim(ssn, anim_move)&nbsp;&nbsp;&nbsp; sets the ssn to ADM move slot<br />
;SSNradio(ssn, "wave.wav")&nbsp; sets the ssn to talk on radio<br />
;<br />
;SSNmax(ssn, maxengage) set max engage distance<br />
;SSNmin(ssn, minengage) set min engage distance<br />
;SSNatt(ssn, maxattack) set max attack distance<br />
;SSNSpawn(ssn, spawn count) set the spawn count, 0=don't respawn<br />
;GroupMax(group, maxengage) set max engage distance<br />
;GroupMin(group, minengage) set min engage distance<br />
;GroupAtt(group, maxattack) set max attack distance<br />
;GroupSpawn(group, spawn count) set the spawn count, 0=don't respawn<br />
;remove(grp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; remove group # without a trace<br />
;kill(grp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; kill group #<br />
;removeSSN(ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; remove SSN # without a trace<br />
;killSSN(ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; kill SSN #<br />
;teleport(grp,tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; teleport group # to target #<br />
;telessn(ssn, tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; teleport SSN # to target #<br />
;targetfx(tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create med particle fx at target #<br />
;sound2tgt(ss,tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create ssoundset at target # (ex. sound2tgt sSoundSet 1)<br />
;ss2ssn(ss,ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create ssoundset at center of ssn<br />
;ammo2tgt(ammo,tgt)&nbsp;&nbsp;&nbsp;&nbsp; create ammo # at target # (ex. ammo2tgt ammo_rocket 1)<br />
;fx2tgt(fx,tgt)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; create fx # at target # (ex. fx2tgt effect_lightning 1)<br />
;opendoors(group)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; open doors in group #<br />
;closedoors(group)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; close doors in group #<br />
;SSNtoWP(ssn, wp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; redirect SSN to WP list<br />
;GtoWP(group, wp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; redirect Group to WP list<br />
;ammorain(ammo)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rain down ammo # somewhere near player<br />
;ammoarea(ammo, area)&nbsp;&nbsp; rain down ammo # somewhere inside area #<br />
;fxrain(fx)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rain down effect # somewhere near player<br />
;ssncspd(ssn,speed)&nbsp;&nbsp;&nbsp;&nbsp; set ssn to combat speed of #<br />
;ssnpspd(ssn,speed)&nbsp;&nbsp;&nbsp;&nbsp; set ssn to patrol speed of #<br />
;ssnuse(ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; snap ssn to emplaced weapon if within 3 meters<br />
;ssnrelease(ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; remove ssn from emplaced weapon<br />
;ssn2ssn(ssn, ssn)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; have ssn goto ssn and try to snap to emplaced<br />
;ssnface(ssn, face)&nbsp;&nbsp;&nbsp;&nbsp; set face expression of ssn<br />
;ssnguard(ssn, #)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set ssn to guard 1=ON, 0=Off<br />
;ssnturn(ssn, heading)&nbsp; turn ssn to heading 0-360<br />
;ssnhp(ssn, hp)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set ssn's hitpoints to hp<br />
;blockfire(wpnkey, #)&nbsp;&nbsp; blockfiring of weapons under key, 0=fire, 1=block<br />
<br />
///////////////////WAC HELP 2（NILE）///////////////////<br />
<br />
//PLOOP -&gt; Real Player loop<br />
<br />
//- PLOOP actions(player) END<br />
<br />
//CODEPLOOP<br />
//if condition<br />
//actions(player)<br />
//end condition<br />
//END<br />
<br />
<br />
//---------------------------------------------------<br />
<br />
//GLOOP -&gt; Named Group Loop<br />
<br />
//don't use group commands !<br />
<br />
//- GLOOP group actions(item) END<br />
<br />
//CODEGLOOP group<br />
//if condition<br />
//actions(item)<br />
//end condition<br />
//END<br />
<br />
<br />
//------------------------------------------------------<br />
<br />
//ENTER -&gt; to handle like 'and never()', but fire again was con. false<br />
<br />
//CODEIF trigger ENTER // triggers on first true<br />
//actions<br />
//ENDIF<br />
<br />
<br />
//------------------------------------------------------<br />
<br />
//LEAVE -&gt; fired if condition no more true<br />
<br />
//CODEIF trigger LEAVE // triggers after last true<br />
//actions<br />
//ENDIF<br />
<br />
<br />
//-------------------------------------------------------<br />
<br />
//IFNAME -&gt; give a condition a Name<br />
<br />
<br />
//CODEIF [ifname] triggers THEN // optional named if<br />
//actions<br />
//ENDIF<br />
<br />
<br />
//---------------------------------------------------------<br />
<br />
// WAC Quick Reference Help File<br />
<br />
//WAC Flow Control (basic statements)<br />
// IF triggers THEN actions END<br />
// IF [ifname] triggers THEN actions END (optional named if)<br />
// IF triggers THEN actions ELSE actions END<br />
// IF triggers THEN actions ELSEIF triggers THEN actions END<br />
// PLOOP actions(player) END<br />
// GLOOP group actions(item) END<br />
// DOSEQ actions NEXT actions [NEXT actions..] END<br />
// DORND actions NEXT actions [NEXT actions..] END<br />
// IF triggers ENTER actions END (triggers on first true)<br />
// IF triggers LEAVE actions END (triggers after last true)<br />
<br />
// WAC Compiler Commands<br />
// VAR variablename (declares a number variable, shows up on debug screen)<br />
// CHEAT cheatname (declares a server cheat variable, shows up on debug screen)<br />
// RUN filename (this includes the text from a file, good from command line)<br />
<br />
// WAC Logic and Math (tests for inside IF statement)<br />
// Arithmatics ()+-*/^<br />
// Booleans AND OR NOT &lt; &gt; &lt;= &gt;= == !=<br />
// Example: IF V1&lt;12 THEN<br />
// Example: IF V1&lt;12 AND V2&lt;12 THEN<br />
// Note: this compiles to IF (V1&lt;12) AND (V2&lt;12) THEN<br />
<br />
// WAC Assignment Var = Value<br />
// Example: V1 = 12<br />
// Example: V2 = V2+V3*4<br />
// Note: This compiles to V2+(V3*4)<br />
<br />
// WAC Triggers (Things to do after IF statement)<br />
<br />
// elapse (seconds)<br />
// never ()<br />
// previous ()<br />
// chain (seconds)<br />
// past (seconds)<br />
// before (seconds)<br />
// ontick (seconds)<br />
// groupdead (number)<br />
// groupalive (number)<br />
// dooropen (number)<br />
// SSNcritical (ssn)<br />
// SSNexists (ssn)<br />
// SSNdead (ssn)<br />
// SSNalive (ssn)<br />
// SSNwounded (ssn)<br />
// SSNride (ssn)<br />
// SSNonSSN (ssn, ssn)<br />
// SSNnearSSN (ssn, ssn, distance)<br />
// SSNlosSSN (ssn, ssn, distance)<br />
// SSNseesSSN (ssn, ssn, distance)<br />
// SSNarea (ssn, area)<br />
// SSNarea3D (ssn, area)<br />
// SSNloc (ssn, number)<br />
// SSNLeadSSN2SSN (ssn, ssn, ssn, distance)<br />
<br />
// WAC Actions (Things to do after THEN/ENTER/LEAVE statement)<br />
<br />
// reset (ifname)<br />
// Gkill (group)<br />
// Gremove (group)<br />
// Gsetaccuracy (number, number, number)<br />
// GtoWP (number, wplist)<br />
// kill (number)<br />
// remove (number)<br />
// teleport (number, target)<br />
// GroupMin (number, distance)<br />
// GroupMax (number, distance)<br />
// GroupAtt (number, distance)<br />
// GroupSpawn (number, number)<br />
// GroupHP (number, number)<br />
// opendoors (number)<br />
// closedoors (number)<br />
// text (text)<br />
// wave (filename)<br />
// hideSSN (ssn)<br />
// unhideSSN (ssn)<br />
// disableSSN (ssn)<br />
// enableSSN (ssn)<br />
// holdSSN (ssn)<br />
// unholdSSN (ssn)<br />
// setaccuracy (ssn, number, number)<br />
// SSNtoWP (ssn, wplist)<br />
// killSSN (ssn)<br />
// removeSSN (ssn)<br />
// teleSSN (ssn, target)<br />
// SSNwave (ssn, filename, distance)<br />
// SSNradio (ssn, filename)<br />
// SS2SSN (soundset, ssn)<br />
// SSNanim (ssn, anim)<br />
// SSNMin (ssn, distance)<br />
// SSNMax (ssn, distance)<br />
// SSNAtt (ssn, distance)<br />
// SSNSpawn (ssn, number)<br />
// SSNHP (ssn, number)<br />
// SSNADDHP (ssn, number)<br />
// ssn2ssn (ssn, ssn)<br />
// ssnrelease (ssn)<br />
// ssnface (ssn, face)<br />
// ssnturn (ssn, heading)<br />
// ssnguard (ssn, number)<br />
// ssnname (ssn, texttoken)<br />
// ssnpspd (ssn, number)<br />
// ssncspd (ssn, number)<br />
// ssnuse (ssn)<br />
// set (variable, value)<br />
// add (variable, value)<br />
// sub (variable, value)<br />
// inc (variable)<br />
// dec (variable)<br />
// store (variable)<br />
// load (value)<br />
// TOD (hour)<br />
// targetfx (target)<br />
// ammo2tgt (ammo, target)<br />
// fx2tgt (fx, target)<br />
// ammoarea (ammo, area)<br />
// sound2tgt (soundset, target)<br />
// flash ()<br />
// farflash ()<br />
// quake (number)<br />
// win (number)<br />
// lose (number)<br />
// music (number)<br />
// skyspeed (number)<br />
// skyheight (number)<br />
// fogtype (number)<br />
// fogdist (distance)<br />
// movefog (distance, seconds)<br />
// rain (number, seconds)<br />
// snow (number, seconds)<br />
// overcast (number, seconds)<br />
// Help ()<br />
// text# (text, number)<br />
// consol (text)<br />
// consol# (text, number)<br />
<br />
// WAC Debug Commands (This also contains old stuff)<br />
<br />
// sound (soundset, distance, heading)<br />
// forceanim (anim)<br />
// tele (ssn)<br />
// fall ()<br />
// fov (number)<br />
// squadevent (number)<br />
// random (number)<br />
// outside ()<br />
// location (number)<br />
// area (area)<br />
// area3D (area)<br />
// waveready ()<br />
// weaponfired (number)<br />
// event (number)<br />
// meride (ssn)<br />
// meattached (ssn)<br />
// medrive (ssn)<br />
// meongun (ssn)<br />
// ammorain (ammo)<br />
// fxrain (fx)<br />
// lightning (red, green, blue)<br />
// face (face)<br />
// anim (anim)<br />
// sunfade (number, seconds)<br />
// gain (red, green, blue)<br />
// squadclear ()<br />
// blockfire (number, number)<br />
// colorfade (number)<br />
// sun (red, green, blue)<br />
// sky (red, green, blue)<br />
// ground (red, green, blue)<br />
// floor (red, green, blue)<br />
// ceiling (red, green, blue)<br />
// cloud (red, green, blue)<br />
// fogcolor (red, green, blue)<br />
// fog (red, green, blue)<br />
// skyfogcolor (red, green, blue)<br />
// skyfog (red, green, blue)<br />
// crash (red, green, blue, green)<br />
// eq (number, number)<br />
// ne (number, number)<br />
// lt (number, number)<br />
// gt (number, number)<br />
// le (number, number)<br />
// ge (number, number)<br />
// true (number)<br />
// false (number) <br />
<br />
///////////////////WAC HELP 3///////////////////<br />
&nbsp;;WAC Text in Color<br />
;<br />
;This WAC code was supplied by Bubbachuk~{PG}~<br />
;<br />
;Here is a short list of the color codes you can use to change<br />
;the color of the WAC scripts: The script has to be enclosed<br />
;between right and left arrows, i.e. &lt;#####&gt; and inserted<br />
;before the line of text in the consol or text displays, i.e.<br />
;<br />
;if never() then<br />
;consol("&lt;#####&gt;Insert color code before actual text. Characters")<br />
;text("&lt;#####&gt;in the code count in the character count allowed")<br />
;text("&lt;#####&gt;for each line.")<br />
;endif<br />
;<br />
;cfff500 or co01 = Yellow<br />
;co30 = Light Yellow<br />
;co100 = Dark Yellow<br />
;cff5000 = Orange<br />
;cf50000 = Red<br />
;cf500000 or c300004 = Dark Red<br />
;c5000 = Dark Green<br />
;cf500 = Green<br />
;co99 = Light Blue<br />
;cff50 = Turquoise<br />
;cfffff5 = White (Don't see any reason to use this one)<br />
;cr953o3 = Gray<br />
;cffff50 = Tan<br />
;cf or ca = Black<br />
;cf50 or cab = Dark Blue<br />
;cff5 or ca0 = Blue<br />
;ca0014 = Purple<br />
;b = Bold Text<br />
;i = Text with Italics<br />
;u = will Underline Text <br />
<br />
///////////////////WAC HELP 4///////////////////<br />
;Previous()<br />
;chain(SECONDS)<br />
;then<br />
;Elapse(SECONDS)<br />
;Past(SECONDS)<br />
;Before(SECONDS)<br />
;OnTick(SECONDS)<br />
;night()<br />
;eq(VAR,NUMBER)<br />
;ge(VAR,NUMBER)<br />
;le(VAR,NUMBER)<br />
;gt(VAR,NUMBER)<br />
;lt(VAR,NUMBER)<br />
;eq(VAR,VAR)<br />
;ge(VAR,VAR)<br />
;le(VAR,VAR)<br />
;gt(VAR,VAR)<br />
;lt(VAR,VAR)<br />
;ne(VAR,NUMBER)<br />
;ne(VAR,VAR)<br />
;true(VAR)<br />
;false(VAR)<br />
;groupdead(GROUP)<br />
;groupalive(GROUP)<br />
;ssndead(SSN)<br />
;ssnwounded(SSN)<br />
;ssnride(SSN)<br />
;ssnonssn(SSN,SSN)<br />
;ssnnearssn(SSN,SSN,DISTANCE)<br />
;ssnlosssn(SSN,SSN,DISTANCE)<br />
;ssnseesssn(SSN,SSN,DISTANCE)<br />
;ssnarea3d(SSN,AREA)<br />
;ssnalive(SSN)<br />
;NOT ssnalive(SSN)<br />
;ssnnearssn(SSN,TEXT,DISTANCE)<br />
;ssnseesssn(SSN,TEXT,DISTANCE)<br />
;ssncritical(SSN)<br />
;random(NUMBER)<br />
;waveready()<br />
;weaponfired(NUMBER)<br />
;Player (coop/multiplayer mode) TEXT = HUMANS<br />
;true(TEXT)<br />
;false(TEXT)<br />
;ssnride(SSN)<br />
;Player (single player mode) TEXT = 10000<br />
;meride(SSN)<br />
;meattached(SSN)<br />
;medrive(SSN)<br />
;meongun(SSN)<br />
;ssndead(TEX)<br />
;ssnwounded(TEXT)<br />
;ssnonssn(TEXT,SSN)<br />
;ssnnearssn(TEXT,SSN,DISTANCE)<br />
;ssnlosssn(TEXT,SSN,DISTANCE)<br />
;outside()<br />
;GtoWP(GROUP,WAYPOINTLIST)<br />
;Gkill(GROUP)<br />
;Gremove(GROUP)<br />
;GroupMin(GROUP,DISTANCE)<br />
;GroupMax(GROUP,DISTANCE)<br />
;GroupAtt(GROUP,DISTANCE)<br />
;GroupSpawn(GROUP,NUMBER)<br />
;GroupHP(GROUP,NUMBER)<br />
;Gsetaccuracy(GROUP,NUMBER,NUMBER)<br />
;teleport(GROUP,TARGET)<br />
;SSNtoWP(SSN,WAYPOINTLIST)<br />
;KillSSN(SSN)<br />
;removeSSN(SSN)<br />
;teleSSN(SSN,TARGET)<br />
;ssnWave(SSN,TEXT,DISTANCE)<br />
;ssncspd(SSN,NUMBER)<br />
;ssnpspd(SSN,NUMBER)<br />
;SSNanim(SSN,ANIM)<br />
;SSNMin(SSN,DISTANCE)<br />
;SSNMax(SSN,DISTANCE)<br />
;SSNAtt(SSN,DISTANCE)<br />
;SSNSpawn(SSN,NUMBER)<br />
;SSNHP(SSN,NUMBER)<br />
;ssnUse(SSN)<br />
;ssnRelease(SSN)<br />
;SSN2SSN(SSN,SSN)<br />
;ssnface(SSN,FACE)<br />
;ssnTurn(SSN,HEADING)<br />
;SSNtoWP(SSN,NUMBER)- Waypoint 123,124,125,126,127<br />
;hideSSN(SSN)<br />
;unhideSSN(SSN)<br />
;enableSSN(SSN)<br />
;disableSSN(SSN)<br />
;fov(SSN,NUMBER)<br />
;ssnguard(SSN,NUMBER)<br />
;ssnblind(SSN,NUMBER)<br />
;ssnclimb(SSN,NUMBER)<br />
;setaccuracy(SSN,NUMBER,NUMBER)<br />
;holdSSN(SSN)<br />
;unholdSSN(SSN)<br />
;SSNLeadSSN2SSN(SSN,SSN,SSN)<br />
;ssnaddhp(SSN,NUMBER)<br />
;set(VAR,NUMBER)<br />
;add(VAR,NUMBER)<br />
;sub(VAR,NUMBER)<br />
;inc(VAR)<br />
;dec(VAR)<br />
;tod(TIME)<br />
;ammo2tgt(AMMO,TARGET)<br />
;fx2tgt(FX,TARGET)<br />
;ammorain(AMMO)<br />
;ammoarea(AMMO,AREA)<br />
;fxrain(FX)&lt;/WAC&gt;<br />
;sound2tgt(SOUNDSET,TARGET)<br />
;flash()<br />
;farflash()<br />
;quake(SECONDS)<br />
;fogdist(DISTANCE)<br />
;win(NUMBER)<br />
;rain(NUMBER,NUMBER)<br />
;overcast(NUMBER,NUMBER)<br />
;Wave(TEXT)<br />
;fogtype(NUMBER)- Fog Type (0,1,2,3)<br />
;gain(NUMBER,NUMBER,NUMBER)<br />
;skyspeed(NUMBER)<br />
;snow(NUMBER,NUMBER)<br />
;movefog(DISTANCE,NUMBER)<br />
;wind(NUMBER,SECONDS<br />
;floor(NUMBER,NUMBER,NUMBER)<br />
;ceiling(NUMBER,NUMBER,NUMBER)<br />
;fog(NUMBER,NUMBER,NUMBER)<br />
;accuracyspread = (NUMBER)<br />
;breathtime = (NUMBER)<br />
;fallmps = (NUMBER)<br />
;text#(TEXT,VAR)<br />
;consol(TEXT)<br />
;consol#(TEXT,VAR)<br />
;text(TEXT)<br />
;PLOOP<br />
;GLOOP<br />
;<br />
;<br />
;1. Previous() - Previous conditional was true.<br />
;Example:<br />
;code<br />
;if never enter<br />
;consol("Sending an enemy to stand on a platform")<br />
;ssn2ssn(xxxx,xxxxx)<br />
;endif<br />
;if previous() and never enter<br />
;consol("Once enemy stands on that platform, Will send his buddy to a waypoint!")<br />
;SSNtoWP(XXXX,X)<br />
;endif<br />
;<br />
;2. chain() - Previous conditional was true and One seconds have elapsed.<br />
;Example:<br />
;code:<br />
;if past(30) and never enter<br />
;consol("Will rain in thirty seconds at 60%!")<br />
;rain(60)<br />
;endif<br />
;if chain(30) enter<br />
;consol("Thirty seconds later dark clouds will form overhead at 90%!")<br />
;overcast(90)<br />
;endif<br />
<br />
;3. then - Means end of conditional. You can also use enter instead of then. if you choose to use enter replacing then use it throughout the entire wac script your working on. Don't mismatch then and enter either one or the other.<br />
;Example:<br />
;if never() enter<br />
;consol("Enemy will follow the closest player!")<br />
;ssn2ssn(xxxx,player)<br />
;endif<br />
;<br />
;4. Elapse() - One seconds have elapsed since conditional was last true. Fires continually<br />
;Example<br />
;code:<br />
;if elapse(40) enter<br />
;consol("Every forty seconds their will be an explosion!")<br />
;killSSN(XXXXX)<br />
;endif<br />
;<br />
;<br />
;5. past() - At least one seconds since the mission started before this firers.<br />
;Example<br />
;CODE:<br />
;if past(50) and never enter<br />
;consol("Your seeing this message because fifty seconds has pass!")<br />
;endif<br />
;<br />
;<br />
;6. Before - How many seconds since mission started<br />
;Example<br />
;CODE:<br />
;if Before(3) and OnTick(3) and never enter<br />
;consol("Setting up v1 to go to zero reguardless of humans")<br />
;set(v1,0)<br />
;endif<br />
;if chain(3) enter<br />
;consol("Now I Know v1 equals zero because their is humans!")<br />
;v1 = = 0<br />
;endif<br />
;<br />
;7. OnTick exactly one seconds since the mission started<br />
;Example:<br />
;CODE<br />
;if OnTick(180) enter<br />
;consol#("TIME NOW ",CurTOD)<br />
;consol#("Time Update Will Be Every Three minutes")<br />
;endif<br />
;<br />
;8. night() Night falls between these hours (19:00 - 6:00). Even know I knew this one did not work at the beginning, still wanted to test it anyway. Does not work. You can set-up the time rate and environment file using nile map editor. But
the wac script for night does not work. You can use NLH under terrain to change the environment files also. The lowest rate for time change is one hour = 24hour period.<br />
;<br />
;9. var = = Num or eq(VAR,NUMBER) First before I give the code below let me explain what is I'm doing here. I' am setting up a variable this case it's going to be v1. Next I will set up the number of items I want to keep track of, this case
it's three. Using the VAR to subtract the number of deaths.<br />
;EXAMPLE:<br />
;CODE<br />
;if never enter<br />
;set(v1,3)<br />
;endif<br />
;if eq(v1,0) and never enter<br />
;consol("Each time an item is destroyed subtract intil it reaches zero!)<br />
;endif<br />
;if ssndead(xxxxx) and never enter<br />
;dec(v1)<br />
;endif<br />
;if ssndead(xxxxx) and never enter<br />
;dec(v1)<br />
;endif<br />
;if ssndead(xxxxx) and never enter<br />
;;dec(v1)<br />
;;endif
