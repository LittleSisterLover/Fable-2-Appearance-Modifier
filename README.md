# Disclaimer: Fable 2's internals are not well-known, documented, or completely accessible - I cannot guarantee the presence of some mechanic unknown to myself that may be adversely affected by this mod. Use of this is at your own discretion. Please read this page in its entirety to understand the mod's functions.

</br>
</br>
</br>

# Fable 2 Appearance Modifier
Fable 2 Appearance Modifier is an external script mod intended to allow players greater control over their character's appearance regardless of the ability investments they choose to make.

This mod was developed using and is intended for playing the game through the Canary branch of the Xenia emulator with readback_resolve in use, though it should work on physical hardware provided you have a modded console.

This mod will NOT address Xenia's black character bug nor allow for character customization without readback_resolve. If you're looking to play Fable 2 on Xenia without readback_resolve for the purpose of scaling or performance, the best option at this time is to use Guy/JustSomeGuy1234's texture morph disabling patch, which can be found within the patches on Xenia Canary's patch page.

By having texture morphs disabled, visual changes such as will lines and skin tone from alignments will not function. However, changes which affect your mesh rather than textures (such as bulk and height) will, and this mod can be used to give more control over that.

# Installation
Installation will only be possible through an XEX version of the game. If you have the game in GOD or ISO format, utilities exist online to convert it to an XEX.

Within your game folder, find data\gamescripts_r.bnk. Rename this file to "gamescripts_r_o.bnk". This serves as your backup in case you wish to uninstall the mod at a later date.

Additionally, you may wish to backup data\dir.manifest in the same manner. The mod's alteration to this file is minimal and it can easily be undone with any text editor, so this is not strictly necessary.

Drag and drop the "data" folder from the mod's download into your game folder. It will ask to overwrite "dir.manifest" if you opted not to back this up/change this file's name - be sure to allow this.

The mod should function as expected on existing saves, but as per the Disclaimer, you may wish to backup your save prior to installation. This can be done by opening your "content" folder within Xenia and making a copy of your "4D5307F1" folder.

# Mod Usage
Within data\scripts will be a file named "AM.lua". This is the lua script that defines the mod's functions. Open this script with any text editor to find the following:

```
--------------------------------------------------
-- The three functions below will change the
-- visible effects of the three stat categories.
--------------------------------------------------

-- Alter the "Strength" value of the player character.
-- This is the muscular effect provided by the
-- "Physique" ability.
-- Valid Values: 0 to 1

--Debug.SetHeroStrength(0)

-- Alter the "Skill" value of the player character.
-- This is the height effect provided by the
-- "Accuracy" ability.
-- Valid Values: 0 to 1

--Debug.SetHeroSkill(0)

-- Alter the "Will" value of the player character.
-- This is the will line effect on the player's
-- body that occurs as you level spells.
-- Valid Values: 0 to 1

--Debug.SetHeroWill(0)

--------------------------------------------------
-- I do not recommend touching the three functions 
-- below as these have additional gameplay effects.
-- They are included for those who want to test
-- their effects on character appearance.
--------------------------------------------------

-- Alter the "Fat" value of the player character.
-- This is the visible weight gain/loss that
-- occurs from eating, and enabling this will
-- prevent that system from working correctly
-- in-game.
-- Valid Values: 0 to 1

--Debug.SetHeroFat(0)

-- Alter the "Morality" value of the player character.
-- This will affect both the visuals of your character
-- as well as their actual reputational stats, enabling
-- this will then prevent that system from working
-- correctly in-game.
-- Valid Values: -1000 to 1000

--Debug.SetHeroMorality(0)

-- Alter the "Purity" value of the player character.
-- This will affect both the visuals of your character
-- as well as their actual reputational stats, enabling
-- this will then prevent that system from working
-- correctly in-game.
-- Valid Values: -1000 to 1000

--Debug.SetHeroPurity(0)
```

This file is broken up into two sections. The first has functions related to the physical alterations "Strength", "Skill", and "Will" apply to your character. The second has functions related to "Fat", "Morality", and "Purity" - these are included because they do have visual effects on your character users may wish to mess with, but in normal play should be left alone as they have additional effects in the game.

Each line with "Debug" is a function to alter that attribute of your character. To enable this, remove the two "-" at the beginning of the line. To disable the function, place two "-" at the beginning of the line.

Each function has above it a small description of the effects of that aspect on your character, as well as the valid values for the function. Values outside those given will still function, but the effect will be the same as those extremes (for instance, "Debug.SetHeroStrength(500)" will have the same effect as "Debug.SetHeroStrength(1)"). In the cases of those with valid values between 0 and 1, decimals between them indicate their degree, with 0 being the lower bound (for instance, for Will lines, "Debug.SetHeroWill(0)" results in no Will lines, while "Debug.SetHeroWill(1)" results in full Will lines. "Debug.SetHeroWill(0.5)" would then result in half-transparent Will lines).

Be careful with this file, these functions are pieces of code that run following every load screen, inappropriate alterations may result in game crashes.

You may occasionally see visuals on your character that are not in accordance with your settings, these should be fixed by traveling through a load screen. See the "[Limitations](# limitations)" section below.

# Uninstallation

The mod can be disabled by restoring your backup of "gamescripts_r.bnk". To do this, delete your current "gamescripts_r.bnk" and rename "gamescripts_r_o.bnk" to "gamescripts_r.bnk".

To remove the mod completely, replace "dir.manifest" with your backup or open it in a text editor and remove the last line, "scripts\AM.lua". Then, go into data\scripts and delete "AM.lua".

This mod should be safe to uninstall on existing saves, but as per the Disclaimer, you may wish to backup your save prior to doing so.

# Limitations
Our ability to alter Fable 2's functionality is somewhat limited. In most situations when a character does something that would alter their visuals, they're flagged for a "texture morph", which is applied following a load screen. This mod then morphs your character again following this, allowing for those changes to be overwritten by the mod script and your appearance be maintained.

There are two primary cases where this is not true:

- Mesh morphs. Mesh morphs are mainly things which affect your character's physical space, such as leveling "Accuracy", which makes your character taller. Mesh morphs occur instantly rather than following a load screen, and because the morph system is largely outside our control, there's little that can be done about this. If you have the related function enabled in the mod's script, then this physical change should be undone and your character will appear as expected following a load screen.

- Quests. Occasionally quests have a trigger to regenerate your texture morphs for various visual effects. This may result in your will lines showing up suddenly, but as with mesh morphs, this should go away following the next load screen.

# How Does This Mod Work?
This mod needs to consistently apply player preferences to their character in order to overwrite the changes done by the game's morph system. This is done by hijacking an internal AI setup script that runs following load screens to direct "AM.lua" to run before allowing the AI script to perform its normal function. The mod functions themselves are internal debug functions left by the game developers for directly altering player stats, their main function being in random character generation functions and several presets that were used for playtesting or potentially promotional material. As far as I have been able to tell, at least for the three stat functions, there is no bearing on gameplay and these values exist purely for the morph system, meaning abilities should work as normal.

It would be possible to instead have this script run every few seconds, which would reduce the visual impact of the limitations, but in my personal opinion this is a very small benefit for far higher interference.

My primary goal was to remove will lines from my character; I want to use magic, but not at the cost of being ugly. I've added the other functions and released this mod for those with similar issues but not necessarily the skillset to address it.

# Credits
I primarily credit Guy/JustSomeGuy1234's for this mod. Above all else, the time save of his creation and assemblage of tools to access the game's internals, as well as his video describing their use, is likely the main reason it happened. I highly recommend the video to anyone interested in modifying Fable 2:

https://youtu.be/Gvi9v9_c6KY?si=KrDl8bZAJSSIaBi9
