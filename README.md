# Party AI Scripts For Baldurs Gate Games

My own scripts for the Party AI in Baldurs Gate Enhanced Edition and Baldurs Gate 2 Enhanced Edition. I was getting annoyed at the party AI casting healing spells too late, as well as some other things, so I started looking into changing the AI and got a bit carried away with it. I used the software Near Infinity to edit the scripts. I'll take you through how you can apply these scripts to your game or even make your own. I'm using the enhanced edition of Baldur's Gate from steam, I can't promise this will work if you don't have all the expansions.

# Files

The files are split up by game. Baldurs Gate Enhanced Edition and Baldurs Gate 2 Enhanced Edition. This is because some items are not available in certain games and the numbers that represent the overhead messages for each game are different.

Scripts - The Party AI files. Add these to your scripts folder which is next to Baldur.exe. In game you can select one of these files for each of your characters. 

Override - Summons, project image and simulacrum AI files. Add these to your override folder which is next to Baldur.exe. This will automatically apply them to the summons and copies and they will automatically cast spells.

# Install Near Infinity

To edit the files yourself you can use [Near Infinity](https://baldursgate.fandom.com/wiki/Near_Infinity) which can be downloaded [here](https://github.com/NearInfinityBrowser/NearInfinity/wiki). Extract the zip file and place the folder anywhere, it should be able to find the games by itself. Run the executable jar file by navigating to it in the command prompt and type 'java -jar NearInfinity.jar'. To open the games files in Near Infinity in the menu click *Game* -> *Open Game* and browse for the game directory where the .key file is located. I'm using the steam version of Baldurs Gate, C:\Program Files (x86)\Steam\steamapps\common\Baldur's Gate Enhanced Edition, but this should work with any version of the game.

![Near Infinity](./images/nearInfinity.png)

# Add Overhead Messages

I've also added messages that appear above the party members heads that tell you what their next action is. e.g. 'Attacking Nearest Enemy'. To do this I've had to add new text strings to the game by editing the dialog.tlk file which is a list of all the different text in the game. This is mostly for debugging to make sure they are doing the right thing and can be useful for when you want to write your own scripts. If you don't want these messages to appear, then you can just use find and replace to comment out or remove all the lines with *DisplayStringHead(Myself,[number])* and skip this step.

To get these messages to appear you will need to download Modmerge from [here](https://github.com/ScottBrooks/modmerge/releases) and place the modmerge.exe into your root Baldur's Gate Folder, the folder with the Baldur.exe. Then double click modmerge.exe to run modmerge and this will allow the dialog.tlk file to become editable and for NearInfinity to recognize changes to the dialog.tlk. You can then replace the dialog.tlk file in the lang\en_US folder with the one included in this repo. To make sure its working, you can load up NearInfinity and go to *Edit* -> *String table* to see the dialog.tlk file.

![Dialog.tlk](./images/dialog.tlk.png)

Avoid updating the game through steam or backing it up through steam as it will change some of the files as it verifies the files. This may cause the new strings in the dialog.tlk to not be recognized again and you will end up with messages like 'Invalid: 71400'. To fix this you will have to run modmerge again. However, modmerge will think it doesn't need to be run as some of the files it creates, have already been created. To make modmerge run again you will have to delete some files as mentioned here. https://forums.beamdog.com/discussion/comment/1005105/#Comment_1005105. Delete the files sod-dlc.disabled, sod-dlc.key, chitin.key.bak and the folder sod-dlc. Then try running modmerge again. Otherwise it should work again if you reinstall and follow these steps again.

# Add Shouts

Shouts allow the Party AI to communicate with each other. Here, shouts are used to share items and potions. For example when one party member has low health but has run out of potions they will shout to the others that they need a healing potion. The others are constantly listening, hear them shout and respond by giving them a healing potion. This way you dont have to worry about passing potions between the AI. To enable this we just need to replace the contents of the SHOUTIDS.IDS file in NearInfinity within the IDS folder with the one in this repo.

# Add The New AI Scripts

To add the script files to your game, find the folder *Scripts* within Near Infinity and click on one of the .BS files to select it and then click *File* -> *Add Copy Of...* in the menu at the top. Name the new file ai.BS. Replace the contents of the file with the ai.BS in this repo and then click *Compile* and then *Save*. To apply the AI script to your character in the game, open the character menu and click *Customize* at the bottom, select *Script* and if you scroll down you should now see an option *Custom (AI)*. Select this and click save. That's it, your character should now follow these actions. 

![Scripts List](./images/scriptsList.png)

# The AI Actions

The file ai.BS is used for all melee fighters and the file aiRanged.BS is used for all ranged characters like wizards and archers. The only differences between the two is that aiRanged.BS makes the characters move away from enemies whenever enemies come near rather than just at low health to keep them at a distance and aiRanged.BS only use ranged weapons.

In order of priority of actions the AI takes
1. Cancel Fireball If Enemy Has Moved Closer
1. Status Messages
   
   1. Display Energy Level Drained - If Energy Level Drained
   1. Display Fatigued - If Fatigued
2. Use And Share Antidote Potions

   3. Antidote - If Poisoned
   4. Elixir of Health - If Poisoned
2. Use And Share Cure Disease Potions

   1. Elixir of Health - If Diseased
2. Use And Share Buff Potions
   
   1. Potion: Fire Resistance - If Taken Lots Of Fire Damage
   2. Potion: Cold Resistance - If Taken Lots Of Cold Damage
   3. Potion: Insulation - If Taken Lots Of Electricity Damage
1. Healing Spells 

   1. Cleric - Cure Light Wounds - If Ally Health Low
   2. Cleric - Cure Medium Wounds - If Ally Health Low
   3. Cleric - Cure Serious Wounds - If Ally Health Low
   4. Cleric - Mass Cure - If Ally Health Low
2. Use And Share Healing Potions 

   2. Potion of Extra Healing (27 Hit Points) - If Lost 27 Hit Points And < 60% Health
   1. Potion of Healing (9 Hit Points) - If Lost 9 Hit Points And < 70% Health
5. Run Away If Health Is Low (AIRanged Will Run Away As Soon As An Enemy Comes Near)

   1. If Enemy Nearby And Space To Run Away, Run Away
   2. If Enemy Nearby And No Space To Run Away, Continue With Normal Actions
6. Debuff/Control Spells  

   1. Cleric - Zone Of Sweet Air - If Gas/Cloud Spell In Area
   1. Cleric - Silence - If Enemy Spellcasters
   1. Wizard - Lower Resistance - If Enemy Has High Magical Resistance
   2. Wizard - Secret Word - If Spellcaster With Spell Blocking Spell
   2. Wizard - Detect Invisibility - If Enemy Casts Invisibility Spell
   2. Cleric - Hold Person
   2. Wizard - Sleep
   2. Wizard - Hold Person
   2. Wizard - Hold Monster - If Many Enemies
   2. Wizard - Chaos - If Many Enemies
   2. Wizard - Horror - If Many Enemies
   2. Wizard - Breach - If Enemy With Magical Buff
   3. Wizard - Dispel Magic - If Ally With Magical Debuff
   3. Wizard - Dispel Magic - If Enemy With Magical Buff
   3. Wizard - Remove Magic - If Enemy With Magical Buff
   4. Cleric - Dispel Magic - If Ally With Magical Debuff
   4. Cleric - Dispel Magic - If Enemy With Magical Buff
   5. Cleric - Miscast Magic - If Enemy Spellcasters
   6. Cleric - Doom - If Tough Enemy
   7. Wizard - Greater Malison - If Tough Enemy
6. Debuff Wands

   1. Wizard - Wand Of Fear - If Many Enemies
   1. Wizard - Wand Of Sleep - If Many Enemies
   2. Wizard - Wand Of Paralyzation- If Tough Enemy 
6. Cleric - Turn Undead

7. Summon Spells (Press Q HotKey)

   1. Cleric - Animate Undead
   1. Wizard - Animate Undead
   2. Cleric - Call Woodland Beings
   3. Cleric - Animal Summoning 3
   4. Cleric - Animal Summoning 2
   5. Cleric - Animal Summoning 1
   6. Cleric - Conjure Animals
   7. Cleric - Conjure Fire Elemental
8. Buff Spells  

   11. Cleric - Ironskin                        (1 Hour)
   2. Cleric - Remove Fear                      (5 minutes)
   11. Cleric - Protection From Lightning       (30 seconds/level)
   3. Cleric - Protection From Evil 10 Foot     (24 seconds/level)
   8. Cleric - Chaotic Commands                 (24 seconds/level)
   3. Cleric - Protection From Evil             (18 seconds/level)
   11. Cleric - Champions Strength              (18 seconds/level)
   4. Cleric - Barkskin                         (24 seconds + 6 seconds/level)
   7. Cleric - Shillelagh                       (24 seconds + 6 seconds/level)
   8. Cleric - Flame Blade                      (24 seconds + 6 seconds/level)
   10. Cleric - Armor Of Faith                  (18 seconds + 6 seconds/level)
   11. Cleric - Protection From Fire            (18 seconds + 6 seconds/level)
   11. Cleric - Magic Resistance                (18 seconds + 6 seconds/level)
   5. Cleric - Aid                              (6 seconds + 6 seconds/level)
   11. Cleric - Righteous Magic                 (6 seconds/level)
   11. Cleric - Resist Fire And Cold            (6 seconds/level)
   11. Cleric - Holy Power                      (6 seconds/level)
   11. Cleric - Strength Of One                 (60 seconds)
   2. Cleric - Chant                            (60 seconds)
   1. Cleric - Bless                            (36 seconds)
   2. Cleric - Defensive Harmony                (36 seconds)
   9. Cleric - Draw Upon Holy Might             (24 seconds)
   5. Wizard - Chaos Shield                     (30 seconds + 60 seconds/5 levels)
   17. Wizard - Melfs Minute Meteors            (Until Charges Depleted)
   21. Wizard - Stoneskin                       (1 Hour)
   18. Wizard - Strength                        (60 seconds/level)
   19. Wizard - Protection From Acid            (60 seconds/level)
   20. Wizard - Protection From Fire            (60 seconds/level)
   6. Wizard - Shield                           (5 minutes)
   12. Wizard - Resist Fear                     (5 minutes)
   14. Wizard - Mirror Image                    (18 seconds + 6 seconds/level)
   16. Wizard - Improved Invisibility           (18 seconds + 6 seconds/level)
   13. Wizard - Haste                           (18 seconds + 6 seconds/level) 
   15. Wizard - Minor Spell Deflection          (18 seconds/level)
   15. Wizard - Spell Deflection                (18 seconds/level)
   15. Wizard - Minor Spell Turning             (18 seconds/level)
   15. Wizard - Spell Turning                   (18 seconds/level)
   15. Wizard - Minor Globe Of Invulnerability  (6 seconds/level)
9. Target Next Enemies (Target one of the following in order of priority, then continue to next block)

   1. Target Attacker Of Wizard Allies
   1. Target Attackers Of Hurt Allies
   1. Target Enemy Spellcasters
   2. Target My Attacker If They Are Not Disabled (Panic, Confusion etc.)
   3. Target Weakest Nearby Enemies If They Are Not Disabled (Panic, Confusion etc.)
   2. Target Nearby Enemies
9. Offensive Spells On Chosen Target

   1. Wizard - Fireball - If Enemy Far Away
   1. Wizard - Magic Missile
   1. Wizard - Chromatic Orb
   2. Wizard - Flame Arrow
   2. Wizard - Melf Acid Arrow
   3. Wizard - Agannazar Scorcher
   4. Cleric - Holy Smite
9. Offensive Wands On Chosen Target

   1. Wizard - Wand Of Frost - If Tough Enemy (Not In BG2 As Has Area Effect In BG2)
   2. Wizard - Wand Of Magic Missile
   3. Cleric - Wand Of Heavens - If Tough Enemy
   2. Wizard - Wand Of Fire - Fireball If Enemy Far Away
11. Equip And Share Arrows If Out Of Ammo  
11. Use Melfs Minute Meteors
11. Use Normal Melee/Ranged Attack On Chosen Target  
12. Regroup
13. Thief - Find Traps

 ### Spells That Block Spells
 
 The following table shows a list of the different spells that are used to block other spells along with their descriptions, whether they will eventually break after protecting against a finite number of spells and whether they are dispelled by the spell 'dispel magic' or a 'spell breach' type spell.
 
 | Spell                    | Description                                   | Break After Many Spells | Dispel Magic | Spell Breach |
|--------------------------|-----------------------------------------------|-------------------------|--------------|--------------|
| Globe Of Invulnerability | Blocks all spells of a certain level or lower | No                      | No           | Yes          |
| Spell Deflection         | Absorbs spells                                | Yes                     | No           | Yes          |
| Spell Turning            | Turns spells back towards the original caster | Yes                     | No           | Yes          |
| Spell Immunity           | Blocks all spells from a certain magic school | No                      | No           | Yes          |
| Shield                   | Immune to magic missile spell                 | No                      | Yes          | No           |
 
 #### Spell Logic
 
 The following logic is applied to all spells cast to make sure they are not wasted if the enemy has a spell blocking spell active.  
 
 If the spell level is 3 or lower, cast it if the enemy doesn't have a Globe of Invulnerability. Theres no separate check to determine whether they have minor globe or normal globe active so assume you cant cast level 3 or lower spells for now. This will mean sometimes level 4 spells will still be wasted if the enemy does have globe of invulnerability.
 
 !CheckStat(LastSeenBy(Myself),1,MINORGLOBE)
 
 If the spell is a debuff spell, cast it if the enemy doesn't have Spell Deflection. Theres no separate check to determine whether they have minor spell deflection or normal spell deflection.
 
 !CheckStat(LastSeenBy(Myself),1,WIZARD_SPELL_DEFLECTION)
 
 If the spell is an offensive or debuff spell, cast it if the enemy doesn't have Spell Turning. Theres no separate check to determine whether they have minor spell turning or normal spell turning.
 
 !CheckStat(LastSeenBy(Myself),1,WIZARD_SPELL_TURNING)
 
 If the spell is of X magic school, cast it if the enemy doesn't have Spell Immunity to magic school X.
 
 !CheckStat(LastSeenBy(Myself),X,WIZARD_SPELL_IMMUNITY)  
 
 If the spell is magic missile, cast it if the enemy doesn't have Shield.  
 
 !CheckSpellState(LastSeenBy(Myself),WIZARD_SHIELD)  
 
 If the spell is remove magic or dispel magic, cast it if the enemy has a magical buff (including Shield)  
 If the spell is a spell breach type spell, cast it if the enemy has a spell blocking spell (not including Shield)  

 #### Spell Schools  
 1 = Abjuration   
 2 = Conjuration   
 3 = Divination  
 4 = Enchantment   
 5 = Illusion  
 6 = Evocation  
 7 = Necromancy  
 8 = Alteration   

#### Check Immune To Spell Level

For each spell, we also check that the enemy isn't immune to spells of that spell level  

!ImmuneToSpellLevel(LastSeenBy(Myself),8)

#### Check For Resistances

We also check for Magic, Fire, Cold, Electricity and Acid Resistance

CheckStatLT(LastSeenBy(Myself),75,RESISTMAGIC)  
CheckStatLT(LastSeenBy(Myself),75,RESISTFIRE)  
CheckStatLT(LastSeenBy(Myself),75,RESISTCOLD)  
CheckStatLT(LastSeenBy(Myself),75,RESISTELECTRICITY)  
CheckStatLT(LastSeenBy(Myself),75,RESISTACID)  

#### Check Theres Nothing Stopping Us Casting The Spell

We also check nothing is stopping us from casting spells

!CheckStatGT(Myself,0,CLERIC_INSECT_PLAGUE)  
!StateCheck(Myself,STATE_SILENCED)  
CheckStatLT(Myself,50,SPELLFAILUREMAGE)  

#### Similar Spells

There are several spells that a Cleric and Mage have that are similar. This table lists those spells and their differences. The Cleric can cast some spells at a level earlier than the wizard but sometimes they are less effective or have a lower duration.

| Cleric                    |                                                                                                    | Mage                        |                                                                                 |
|---------------------------|----------------------------------------------------------------------------------------------------|-----------------------------|---------------------------------------------------------------------------------|
| __Remove Fear__           | Level 1  <br/> 15ft Radius <br/> 1 Hour                                                            | Resist Fear                 | Level 2  <br/> 15ft Radius <br/>  1 Hour                                        |
| Protection From Lightning | Level 4  <br/> 1 Creature  <br/> 30 Seconds/Level                                                  | Protection From Electricity | Level 5  <br/> 1 Creature  <br/> 60 Seconds/Level                               |
| __Protection From Evil__  | Level 1  <br/> 1 Creature  <br/> 18 Seconds/Level                                                  | Protection From Evil        | Level 1  <br/> 1 Creature  <br/> 12 Seconds/Level                               |
| Protection From Fire      | Level 3  <br/> 1 Creature  <br/> 18 Seconds + 6 Seconds/Level  <br/> 100% Fire Resistance          | __Protection From Fire__    | Level 3  <br/> 1 Creature  <br/> 60 Seconds/Level  <br/> 100% Fire Resistance   |
|                           |                                                                                                    | Protection From Cold        | Level 3 <br/> 1 Creature <br/> 60 Seconds/Level <br/> 100% Cold Resistance      |
| Resist Fire And Cold      | Level 2 <br/> 1 Creature <br/> 6 Seconds/Level <br/> 50% Fire Resistance <br/> 50% Cold Resistance |                             |                                                                                 |
|                           |                                                                                                    | Protection From Acid        | Level 5 <br/> 1 Creature <br/> 60 Seconds/Level <br/> 100% Acid Resistance      |

#### Attack Inbetween Actions

Using a normal melee or ranged attack does not count as an action. You can only perform one action per round (6 seconds) and so we use a normal attack inbetween actions instead of standing still doing nothing before our next action. We implement this by using a timer. When we perform an action we start a 6 second timer and check that the timer has expired first before trying to perform another action. There is a wizard spell, Improved Aclarity, that will lift this restriction for spells temporarily and allow the wizard to cast spells continuously for a short while.

| Action                          | Not An Action        |
|---------------------------------|----------------------|
| Casting A Spell                 | Normal Attacks       |
| Using An Item's Special Ability | Using Melf's Meteors |
| Using A Potion                  | Giving Items         |
| Using A Wand                    | Equipping Items      |
|                                 | Shouting             |
|                                 | Targeting An Enemy   |
|                                 | Displaying Messages  |

 ### Testing
 
 CLUAConsole:CreateCreature("skelet") Skeleton   
 CLUAConsole:CreateCreature("werewogr") Greater Werewolf   
 CLUAConsole:CreateItem("?")

 ### Useful links

Descriptions of all the AI and game actions https://gibberlings3.github.io/iesdp/scripting/actions/bgeeactions.htm

Introduction to scripting AI http://www.pocketplane.net/tutorials/simscript.html

Creature Spawn List https://gamefaqs.gamespot.com/pc/75251-baldurs-gate/faqs/7777

Checking Spell States And Effects https://sorcerers.net/Games/BG2/SpellsReference/SpellEffects.htm

How Scripts Work http://www.pocketplane.net/tutorials/simscript.html#EssVariables

Wands And Saving Throws https://forums.beamdog.com/discussion/65851/save-vs-wand-a-k-a-save-vs-nothing
