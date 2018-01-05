# Roll20 Shaped Script
This is a script designed for use with the API on [Roll20](http://roll20.net). This script is specifically designed to provide services and enhancements for the [5e Shaped Sheet](https://github.com/mlenser/roll20-character-sheets/tree/master/5eShaped) by Kryx.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Key features](#key-features)
- [Setup](#setup)
  - [Prerequisites:](#prerequisites)
  - [Installation](#installation)
  - [Updating](#updating)
  - [Optional Extras](#optional-extras)
- [Usage](#usage)
  - [Import a Monster from Monster Manual Statblock text](#import-a-monster-from-monster-manual-statblock-text)
  - [Importing Monsters from the Script Database](#importing-monsters-from-the-script-database)
  - [Importing Spells from the Script Database](#importing-spells-from-the-script-database)
  - [Tracking advantage using the script](#tracking-advantage-using-the-script)
  - [Creating token actions for characters](#creating-token-actions-for-characters)
  - [Decrementing ammo](#decrementing-ammo)
  - [Death saves, Hit Dice](#death-saves-hit-dice)
  - [Roll HP for monsters](#roll-hp-for-monsters)
  - [Process HD rolls](#process-hd-rolls)
  - [Decrement uses](#decrement-uses)
  - [Short/Long rest](#shortlong-rest)
- [Full command list](#full-command-list)
  - [!shaped-statblock](#shaped-statblock)
  - [!shaped-monsters](#shaped-monsters)
  - [!shaped-monster](#shaped-monster)
  - [!shaped-monster-by-token](#shaped-monster-by-token)
  - [!shaped-spell](#shaped-spell)
  - [!shaped-spells](#shaped-spells)
  - [!shaped-at](#shaped-at)
  - [!shaped-abilities](#shaped-abilities)
  - [!shaped-config](#shaped-config)
  - [!shaped-apply-defaults](#shaped-apply-defaults)
  - [!shaped-rest](#shaped-rest)
  - [!shaped-update-character](#shaped-update-character)
  - [!shaped-expand-spells](#shaped-expand-spells)
  - [!shaped-remove-spell](#shaped-remove-spell)
- [Configuration](#configuration)
  - [Advantage Tracker](#advantage-tracker)
  - [Token Defaults](#token-defaults)
  - [Token Defaults/Token Bar Options](#token-defaultstoken-bar-options)
  - [Token Defaults/Token Aura Options](#token-defaultstoken-aura-options)
  - [New Characters](#new-characters)
  - [New Characters/Houserule Settings](#new-charactershouserule-settings)
  - [New Characters/Houserule Settings/Saving Throws](#new-charactershouserule-settingssaving-throws)
  - [Character Sheet Enhancements](#character-sheet-enhancements)
  - [Houserules & Variants](#houserules--variants)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Key features
* Automatically create characters from Monster Manual statblock text
* Import spells to characters from user-created databases of spells in JSON format
* Import monsters from user-created databases in JSON format
* Automatic population of NPC spells from JSON (if present) on import (both JSON and statblock)
* Auto HP rolling on drop for monster tokens
* Token setup with configurable defaults on monster import
* Auto ammo decrement
* Auto death save increment
* Auto HD reduction/HP increase on HD roll
* Advantage tracking commands - set a character to have (dis)advantage and mark which tokens this applies to
* Commands to create token actions for new characters
* Short/Long Rest handling
* Trait use count decrementing
* Automatic configuration of new characters

# Setup
## Prerequisites:
* You, or the person who created your game, must be a **Pro** subscriber to Roll20 to use this script.
* You should be familiar with [how to use roll20](https://app.roll20.net/editor/tutorial), have a basic understanding of how to [configure a game](https://wiki.roll20.net/Game_Management) on Roll20, including setting up [character sheets](https://wiki.roll20.net/Character_Sheets) and installing API scripts. If you've only just started using Roll20 and haven't found your feet yet, setting this script up and using it is probably a bad place to start - it's an advanced tool, many of whose features presume a good working understanding of standard Roll20 capabilities. Play around with a simple campaign first, get used to making characters, working with tokens, and using the 5e Shaped Sheet manually; and then come back here for power-user goodness.
* You should have a recent version of the 5e Shaped Sheet. Due to the lack of versioning support with Roll20 character sheets, we recommend that you install a specific version from [Kryx's Github](https://github.com/mlenser/roll20-character-sheets/tree/master/5eShaped).

## Installation
1. Open the [raw text of the script](https://raw.githubusercontent.com/mlenser/roll20-api-scripts/master/5eShapedScript/latest/5eShapedScript.js)
1. Copy all the text
1. Got to the API scripts page for your campaign
1. Paste the text into a new script, and then give it a sensible name in the box at the top - e.g. "5e Shaped Script"
1. Hit save
1. When the page refreshes, check the console box at the bottom of the page to make sure that there are no errors. If
 everything has worked you should see a line that looks a bit like: `"ShapedScript 1460893071206 INFO :   -=> ShapedScript v0.8.3 <=-"`. If you see a line that includes the text `ERROR` then Something Bad Has Happened and you should ask for help on the API scripts forum if you can't work the problem out for yourself from the error message (sometimes they are even helpful, I promise!)

You are now set up and should be able to use the script as detailed below.

## Updating
Sometimes a new version of the script comes out. If you have installed the script according to the instructions above, you will need to update it manually when this happens. The process is exactly the same as installing the original, except that when you paste the script into your campaign, you should **replace** the script text of the previous version rather than installing a new one. This is very important, because if you end up with two different versions of the script running at the same time, Bad Things Will Happen.

## Optional Extras
Some of the script commands allow you to import spells or monsters from custom data files. This way of importing can be faster and more customisable than dragging from the Compendium, and it's also extensible: you can add extra data files to add more content. The script comes with all the SRD monsters and spells built-in. Creating additional custom data files is beyond the scope of this README at the moment, but you may find pre-made files floating around on the internet made by other people if you search for them. These should be script files that end in .js (**not .json**), and you install them in exactly the same way that you install the main script.

# Usage
## Import a Monster from Monster Manual Statblock text
You first of all need a statblock from somewhere - generally speaking people get these from PDFs of adventures, or perhaps an OCRed copy of the Monster Manual. The statblock must conform to the layout conventions used by WotC for their 5e products or it will not work; fortunately most published material respects these conventions. Once you have statblock, follow these steps:

1. Create/find a token to represent your monster/character, and place it on the tabletop
1. Open the token settings
1. Paste your statblock into the GM notes for the token. Make sure that you haven't lost all of the linebreaks - the Roll20 text editor controls can be a bit funny about line-endings. Sometimes I have to paste the text into a text editor first, and then copy it out of there before I paste it into Roll20. It should look roughly like the original you copied it from, minus all the formatting. At this stage you might also want to check that it's not completely garbled, if e.g. it comes from an OCR scan of a print book. The script will fix a lot of mistakes, but it's still only a script - if the text is totally garbled you may need to fix some of the mistakes manually.
1. When you're done, come out of the token editing and select the token.
1. Type **!shaped-statblock** into the chat window and hit enter.
1. You should see a message in the chat window, and end up with a new character in the journal. If you see an error
message at this point you may need to fix up some problems with the text of the statblock

Your new monster is now ready for use!

### Notes on Statblock Text
The script does its best to fix up errors in the text and be tolerant of variances in layout, but there are some things
that it absolutely requires to be correct:

* Character/monster name needs to be on its own line at the beginning
* Sections headers like "Actions" and "Reactions" need to be the last thing on a line to be recognised (and preferably on a line on their own, although the parser usually deals with this ok)
* Trait and action names need to be title case and end with a full stop, like in the MM.
* Don't paste in descriptions at the bottom of the statblock - just the statblock proper. This is because actions can be multiline and there is no way of telling where a final multiline action ends and a description begins (see e.g. Priest in the MM - don't paste the final description paragraph in)

## Importing Monsters from the Script Database
### Manually

1. Find a token for your new monster and place it on the tabletop
1. Select the token
1. Type **!shaped-monster --Lich** into the chat window and hit enter, replacing 'Lich' with the name of the monster you want to import. Obviously the monster name must match something in your database - which by default includes SRD monsters unless you've added additional data files. If you're not sure what names are present in the database, or exactly what spelling is being used, try the Guided approach mentioned immediately below.
1. You should see a message in the chat window, and end up with a new character in the journal.

Your new monster is now ready for use!

### Guided
When you run **!shaped-monsters** it will output a scrollable list of all of the monsters in the chat window, along with filter buttons to allow you to narrow the list down by things like type, CR, school, class, etc. Clicking a monster on this list will import it into your library. This script comes with all the SRD monsters and spells built in.

## Importing Spells from the Script Database
### Manually

1. Select a token that represents a character
1. Type **!shaped-spell --Fireball, Cure Wounds** into the chat window and hit enter, replacing the spell names with whatever spells you want to import. If you're not sure what spells are available, try the Guided approach mentioned immediately below.
1. You should see a confirmation message in the chat window

Your new spells are now ready for use!

### Guided
Similar to monsters, **!shaped-spells** will display a list of all available spells in the database, pre-filled with all SRD spells. You need a character token selected, or you need to pass a character id like **!shaped-spells --character <character_id>**.

## Tracking advantage using the script
1. Configure your characters with the "Normal" roll option in the character sheet options (see later on for details of how to ensure that this is set automatically for characters imported with the script)
1. Run **!shaped-config** from the chat window, click on 'Advantage Tracker' and then set 'Show Markers' to 'on' if
it isn't already
1. During play, when a character should have advantage or disadvantage, select their token and run **!shaped-at --advantage** from the chat window to give them advantage. Replace **--advantage** with **--disadvantage** or **--normal** for disadvantage/normal respectively.

**IMPORTANT NOTE:** Advantage/disadvantage is a per-character setting, not per-token. If you have monster characters for which many tokens appear on the map at once (e.g. a horde of goblins), they will all get (dis)advantage at the same time - so don't forget to turn it off when you're finished with the token if it's only supposed to apply to one!

### Auto reverting advantage/disadvantage
When enabled, if the script detects a d20 roll with advantage or disadvantage, the character's roll option will be automatically reverted back to "normal".

## Creating token actions for characters
It can be useful to create token action buttons for your characters to allow quick access to commonly used offense, utility, spells, actions, etc. For this you need to use the **!shaped-abilities** command. For example, **!shaped-abilities --offense** will create a token action for each offense item defined for your character. For full details of all the available options, please see [below](#shaped-abilities)

## Decrementing ammo
The script will automatically decrement ammo providing that the information is filled in correctly on the character sheet and the use ammo automatically option is turned on in the sheet options. See the character sheet documentation for more information.

## Death saves, Hit Dice
The script will automatically decrement hit dice and apply regained HP when you roll them from the character sheet. You can turn this off in the [configuration](#configuration). When you roll a death save, it will add it to your death save successes/failures automatically, unless you have the roll 2 option on for your sheet rolls (since it won't know whether you succeeded or failed!)

## Roll HP for monsters
You can configure one of your token bars to represent HP by default in the script [configuration](#configuration). If you do this, and enable the 'Roll HP on Drop' option, the script will automatically Roll HP for any character whose default token doesn't have a linked attribute for the specified bar. PCs and important individual NPCs should have their HP bar linked directly to the character sheet in their token setting, since they are individuals with a single HP value. Generic monsters ('mooks') should not have their HP bar linked to a character sheet attribute, however, since there can be several individuals all sharing a single character sheet. For these monsters, the script will automatically roll HP and apply it to a new token whenever you drag the character from the journal onto the table top. If you would prefer to have a static HP value for your mooks, you should disable this feature in the script configuration and set the HP value for the token manually before setting it as the default token for your character.

## Process HD rolls
If enabled in the configuration (see below), the script will automatically apply hit dice rolled by clicking on the relevant HD line on the character sheet to the character's HP total. It will also decrement the number of HD remaining. It will issue a warning if no HD of the relevant size remain and won't add the amount to the HP total.

## Decrement uses
If enabled in the configuration (see below), the script will automatically decrement the number of uses remaining for traits, actions, offense, utility, features, etc each time they are used. It will also issue a warning if something is used when no uses remain.

## Short/Long rest
The script will apply the effects of a short/long rest if you run the command **!shaped-rest --long** or **!shaped-rest --short** with a character token selected. There are also buttons that link to this functionality on the character sheet itself. You can show these by entering "edit mode" on the sheet and ticking the checkbox on the Settings page for "Show Rests".

# Full command list
## !shaped-statblock
Imports details from a text statblock into a Roll20 character. The statblock must be inserted into the GM notes field of a token (_not_ a character!) and the token must be selected before running this command. The imported character will be configured with the default settings that you specify in the [default character settings configuration](#new-characters) . In addition the tokens you use will be configured according to the [default token settings configuration](#token-defaults) ready to be set as the default token for the character.

### Options
* **--overwrite** if the selected token already represents a character in the journal, the import will fail to avoid accidentally overwriting data, unless you supply this option to confirm that you wish to do so.
* **--replace** if there is already a character in the journal with the same name as the one you are importing, the import will fail, whether or not the current token represents that character. This is to avoid creating loads of duplicates by mistake, which is almost never what you want to do. If you supply **--replace** the script will overwrite any character with the same name, unless there is more than one, in which case it will fail rather than risking overwriting the wrong one. Note that **--replace** implies **--overwrite**.

### Selection
You must have at least one token selected. If you have more than one, it will attempt to import a statblock from each one in turn. When importing, each token will be set to represent the newly created character for it, and the script will also attempt to set the avatar for the character to be the token graphic<sup>[1](#avatar-note)</sup>.

## !shaped-monsters
Displays a list of monsters in the currently loaded JSON database. The list is displayed in the chat window and includes buttons to filter the results on Type, CR and Size. The old "Click to select a monster/spell" query input (**!shaped-monster**) is available as a link at the top of the monsters/spells listings. Clicking a monster that is already imported won't remove it.

## !shaped-monster
Imports details of named monsters from a database of custom monsters loaded as a separate script. The imported character will be configured with the default settings that you specify in the [default character settings configuration](#new-characters) . In addition the tokens you use will be configured according to the [default token settings configuration](#token-defaults) ready to be set as the default token for the character.

### Options
* **--<monster name>** (e.g. **--Lich**) specifies a monster to import. You may supply multiple monsters as separate options, or you may supply multiple in one option separated by commas (**--Ghoul, Zombie, Ghost**)
* **--overwrite** if the selected token already represents a character in the journal, the import will fail to avoid accidentally overwriting data, unless you supply this option to confirm that you wish to do so.
* **--replace** if there is already a character in the journal with the same name as the one you are importing, the import will fail, whether or not the current token represents that character. This is to avoid creating loads of duplicates by mistake, which is almost never what you want to do. If you supply **--replace** the script will overwrite any character with the same name, unless there is more than one, in which case it will fail rather than risking overwriting the wrong one. Note that **--replace** implies **--overwrite**.
* **--as <new name>** if supplied, the new monster will be given the name specified instead of the default name defined in the database.

### Selection
You may no or 1 tokens selected when running this command:

* If you have no tokens selected, this command will just add the new characters to the journal.
* If you have 1 token selected, and you are importing one monster, the script will attempt to use the token image as the avatar for your new character<sup>[1](#avatar-note)</sup>, and will also set the token to represent the newly created character
* If you have 1 token selected and you are importing more than one monster, the script will only attempt to use the token image as the avatar for your new character<sup>[1](#avatar-note)</sup>.

<a name="avatar-note"/><sup>1</sup> Note that avatars will ony successfully be set for token images from your library, not marketplace or web content. This is due to security restrictions within the Roll20 platform. If you really want an image as a character avatar, please unpload it to your library and then create a token from it before doing your import.

## !shaped-monster-by-token
This basically does the same thing as [!shaped-monster](#shaped-monster), except that instead of passing it a list of monsters as parameters to the chat command, it infers the names of the monster to import from the names of the selected tokens. This is a quick way to configure and import a whole bunch of monsters all at once. Find the tokens you want for all your monster and drag them to the tabletop; name the tokens according to the monsters they represent, select them all, and run this command. It will find monsters by name from your custom JSON database, and configure each token to represent the new characters it creates.

### Options
* **--overwrite** as for [!shaped-monster](#shaped-monster)
* **--replace** as for [!shaped-monster](#shaped-monster)

### Selection
You must have at least one token selected for this command. As described above, it will use the name assigned to each token to lookup the monster it will represent from your JSON database.

## !shaped-spell
* Alias !shaped-import-spell *
Imports details of named spells from a database of customer spells loaded as a separate script. All spells will be added to the currently selected character. 

### Options
* **--<spell name>** (e.g. **--Fireball**) specifies a spell to import. You may supply multiple spells as separate options, or you may supply multiple in one option separate by commas (**--Fireball, Lightning Bolt, Wish**)
* **--overwrite** If this option is included, the script will ovewrite existing spells with the same name with the new spells requested, otherwise they will be skipped

### Selection
You must have exactly one token that represents a character selected when running this command.

## !shaped-spells
* Alias !shaped-list-spells
Displays a list of spells in the currently loaded database that match the supplied criteria. Each spell will be a link to either import and delete the spell, depending on whether the selected/supplied character already has that spell in their spell list or not.

### Options
* **--character <characterID>** If there is no token selected, you must supply a character id for the character whose spells are to be edited.

### Selection
You can have up to one token that represents a character selected when running this command.

## !shaped-at
Gives the selected character advantage or disadvantage, or  neither.

### Options
* **--advantage** give the selected character advantage
* **--disadvantage** give the selected character disadvantage
* **--normal** clears (dis)advantage status for the selected character
* **--revert** turns on auto revert advantage/disadvantage for the selected character
* **--persist** turns off auto revert advantage/disadvantage for the selected character

### Selection
You must select at least one token that represents a character. The specified setting will apply to the characters of all selected tokens.

## !shaped-abilities
Creates token actions for your character as shortcuts to a variety of rolls and other actions from the sheet.

### Options
* **--advantageTracker** - create 3 abilities, one for each Advantage Tracker option (Advantage, Disadvantage, Normal)
* **--advantageTrackerShort** - create 3 abilities, one for each Advantage Tracker option, but with shorter names (Adv, Dis, Normal)
* **--advantageTrackerShortest** - as **advantageTrackerShort**, but omit the "normal" option (for use with the auto-revert advantage option)
* **--advantageTrackerQuery** - create an ability to set an Advantage Tracker option from a drop-down query
* **--offense** - create an ability for each offense present in the character sheet
* **--offenseMacro** - create an ability to launch chat window offense buttons
* **--utility** - create an ability for each utility present in the character sheet
* **--utilityMacro** - create an ability to launch chat window utility buttons
* **--racialTraits** - create an ability for each racial trait present in the character sheet
* **--racialTraitsMacro** - create an ability to launch the chat window racial traits buttons
* **--classFeatures** - create an ability for each class feature present in the character sheet
* **--classFeaturesMacro** - create an ability to launch the chat window class feature buttons
* **--feats** - create an ability for each feat present in the character sheet
* **--featsMacro** - create an ability to launch the chat window feat buttons
* **--traits** - create an ability for each trait present in the character sheet
* **--traitsMacro** - create an ability to launch the chat window traits buttons
* **--actions** - create an ability for each action present in the character sheet
* **--actionsMacro** - create an ability to launch the chat window actions buttons
* **--reactions** - create an ability for each reaction present in the character sheet
* **--reactionsMacro** - create an ability to launch the chat window reactions buttons
* **--legendaryActions** or **--legendaryA** - create an ability for each legendary action present in the character sheet
* **--legendaryActionsMacro** - create an ability to launch chat window legendary actions buttons
* **--lairActions** or **--lairA** - create an ability to launch chat window lair actions buttons
* **--regionalEffects** or **--regionalE** - create an ability to launch chat window regional effects buttons
* **--initiative** - create an ability to roll initiative
* **--saves** or **--savingThrows** - create an ability to launch the chat window save buttons
* **--savesQuery** or **--savingThrowsQuery** - create an ability to launch the save drop-down query
* **--abilityChecks** or **--abilChecks** - create an ability to launch the chat window ability check buttons
* **--abilityChecksQuery** or **--abilChecksQuery** - create an ability launch the ability check drop-down query
* **--statblock** - create an ability to launch a chat window statblock display
* **--spells** - create an ability to launch chat window spellbook display
* **--rests** - create an ability to do a short or long rest (pops up a query to ask which)
* **--DELETE** - delete all abilities on the token (including those not created by this script)

In addition, you can pass the names of spells like **--Fireball** to create token actions for each spell. Obviously the character in question must actually have this spell in its spellbook for this to work.

### Selection
You must have at least one token that represents a character selected for this command to work. If you have multiple characters selected you cannot specify spells in the options since the different characters may have different spells.

## !shaped-config
Display configuration UI to change default behaviours. The significance of all the options is detailed [below](#configuration)

## !shaped-apply-defaults
Apply the same defaults that are used when setting up tokens on import to whatever tokens are currently selected. Useful for mass-configuring manually created tokens. See [below](#config-token-settings) for more details on what these options are.

### Selection
You must have at least one token that represents a character selected for this command to work. 

## !shaped-rest
Applies the effects of a long or short rest, or a turn recharge

### Options
* **--long** Long rest
* **--short** Short rest
* **--turn** This will perform a 'turn recharge' - resetting uses for all actions/traits/etc that have X/turn usages.
* **--character** Apply the rest to the supplied character id instead of the selected tokens

### Selection
You must select at least one token that represents a character unless you supply the --character option. The selected character(s) will have the effects of the specified type of rest applied to them.

## !shaped-update-character
Triggers any pending character sheet upgrades for the selected characters. When a new version of the character sheet is released, there are often upgrade scripts that must run before previously created characters can be used with it. Normally these are run the first time you open each character sheet. For convenience, you can use this command to trigger these updates on batches of characters without having to open their sheets.

### Options
* **--all** Apply any pending character sheet upgrades for all characters in the current campaign. Note that this might take a *long* time in a large campaign - use with caution!

## !shaped-expand-spells
When you drag characters from the SRD/Compendium, they are imported with a spell list, but none of the spells have any content. This command will expand these "stub spells" using data from a custom spells database, if one is present.

### Selection
You must select at least one token that represents a character. The selected character(s) will be checked for empty spells and these will be expanded from the custom JSON database.

### Selection
Unless you specify **--all** you must have at least one token that represents a character selected for this command to work.

## !shaped-remove-spell
Removes spells from the selected character.

### Options
* **--all** If supplied, remove all spells from the selected character.
* **--<spell name>** Remove spells by name from the selected character. You may supply multiple spells as separate options, or you may supply multiple in one option separate by commas (**--Fireball, Lightning Bolt, Wish**)

### Selection
You must select exactly one token which represents a character.

# Configuration
## Advantage Tracker
* **Output** Decides how Advantage Tracker will report changes (whisper the gm, public to all, or silent)
* **Show Markers** If this is 'on', all tokens for a character will be marked with the specified markers if they have advantage or disadvantage. Designed for use with the [!shaped-at](#shaped-at) command
* **Ignore NPCs** If this is 'on', Advantage Tracker will ignore NPCs
* **Advantage Marker** Sets the status marker to be displayed when a character has Advantage if **Show Markers** is on
* **Disadvantage Marker** Sets the status marker to be displayed when a character has Disadvantage if **Show Markers** is on

## Token Defaults
Note that all of the settings under Token Defaults are applied to tokens only when **!shaped-monster**, **!shaped-statblock** or **!shaped-apply-defaults** are run.

* **Numbered Tokens** If this is 'on', new tokens will have %%NUMBERED%% appended to their name to work with Aaron's TokenNameNumbered script. Please search for this on the API forum for more details.
* **Show Name Tag** If this is 'on', the token will show its name tag to anyone who has permission to see it
* **Token Name Override** If this is not empty, new tokens will have their name tag set to this value. This is most useful in combination with Numbered Tokens, which allows you to have all your monsters labelled something like 'Monster 1', 'Monster 2', so your players can reference them but don't get any clues about what they are from the name.
* **Show Name to Players** If this is 'on', and **Show Name Tag** is also 'on', players will be able to see the token's name.
* **Light Radius** Default light radius emitted from token (Note that this value will be overriden by a value derived from a monster's senses attribute where available)
* **Dim Radius** Start of dim light within the above-specified light radius (Note that this value will be overriden by a value derived from a monster's senses attribute where available)
* **Other players see light** Whether light emitted from the token is visible only to the controlling player(s) or to all players
* **Has Sight** If 'on' then controlling players will be able to see areas of maps with dynamic lighting enabled based on the position of this token
* **Light angle** Emitted light will be cast in arc this many degrees wide.
* **LOS angle** the token's vision will be limited to an arc this many degrees wide
* **Light multiplier** The multiplier affects how far the token can see from existing light sources. This is a good way to simulate a character who has the ability to see further than normal in low light situations or has an alternate form of vision that might allow them to navigate in the dark. For example, someone who can see twice as far in low light would have a multiplier of two.

## Token Defaults/Token Bar Options
* **Bar X Attribute** Set this to the name of a sheet attribute (e.g. HP, AC, speed) to automatically set the specified token bar/bubble to the value of this attribute
* **Bar X Link** If this is 'on' then the specified bar/bubble will be _linked_ to the configured attribute on the character sheet, i.e. changes to the bar/bubble will update the value on the character sheet. This is generally a good idea for everything except HP for generic tokens.
* **Bar X Set Max** If this is 'on' then the specified bar will be given a maximum value (which will initially be the same as the main bar value) and will consequently display as a bar above the token as well as a value in the 'bubble'. Mainly useful for HP.
* **Bar X Show Players** If this is 'on' then the specified bar/bubble will be shown to everyone, otherwise it will only be visible to the GM and controlling players
* **Do not link NPC HP** If this is 'on' then the HP attribute will not be linked if the token represents an NPC, useful on HD-rolled characters.

## Token Defaults/Token Aura Options
* **Auras** Sets up default auras for new character tokens. If you specify a range, new characters will have an aura with the specified size, colour and shape
* **Aura X Show Players** If this is 'on' then the specified aura will be shown to everyone, otherwise it will only be visible to the GM and controlling players

## New Characters
The settings in this section will be applied to new characters when they are created, since the sheet has no way of storing default options
for new characters.

* **Apply to all new chars?** If this is on, then the settings in this section will be applied to every new character, otherwise they will only be applied to characters created by the script using the various import commands, or when **!shaped-apply-defaults** is run. Please note that these defaults
are applied at the moment of character creation, so some settings may not apply correctly - for example token actions which reference traits that have not yet been created. In this case, you should run **!shaped-apply-defaults** after your character is completely configured to complete the setup. You should also be aware that as things currently stand, this functionality does not work well for items dragged from the Compendium due to limitations in how the drag and drop functionality interacts with the sheet import systems.
* **Sheet Output** Set whether output from the new character sheet should be public or whispered to the controlling player by default.
* **Death Save Output** Same as Sheet Output, but specifically for death saves.
* **Hit Dice Output** Same as Sheet Output, but specifically for Hit Dice.
* **Roll Options** How are d20 rolls handled by default:
    * **Normal**: Roll one die only.
    * **Roll with Advantage**: Roll two dice, display the highest value.
    * **Roll with Disadvantage**: Roll two dice, display the lowest value.
    * **Roll 2**: Roll two dice, display both results.
* **Initiative Settings** How are d20 rolls handled by default:
    * **Initiative Output** Same as Sheet Output, but specifically for Initiative rolls
    * **Init Roll** Similar as Roll Options, but only applies to Initiative rolls. There is no Roll 2 option in this case because initiative must have a single value to be sent to the turn tracker.
    * **Init to Tracker** If 'on', automatically send all initiative rolls to the turn tracker.
    * **Break Init Ties** If 'on', the value a character's initiative bonus will be divided by 100 and added to their initiative roll to break initiative ties (matching rolls mean the character with the highest bonus goes first)
* **Show name on Roll Template** If 'on', the character's name will be shown on all their rolls in chat.
* **Show Target AC** If 'on', all attacks with a target will require a target to be clicked and will display the target's AC on the Roll output
* **Show Target Name** If 'on', all attacks with a target will require a target to be clicked and will display the target's character name on the Roll output
* **Auto Roll Dmg Attacks** If 'on', all attacks with automatically roll and display damage.
* **Auto Roll Dmg Saves** If 'on', all attacks with saves will automatically roll and display damage applied on save failure.
* **Display Settings** This submenu configures the respective sheet fields to be visible by default on new characters.
    * **Show Passive Skills** If 'on', will enable the display of passive skills in the skills section of the Core sheet page.
    * **Show Weight** If 'on', will enable the display of weight information on the Core sheet page.
    * **Show Emote** If 'on', will enable the Emote field which can be customized to automatically display the Emote on the roll template.
    * **Show Freetext** If 'on', will enable the Freetext field.
    * **Show Freeform** If 'on', will enable the Freeform field.
    * **Show Dice Modifiers** If 'on', will enable Dice modifiers.
    * **Show Crit Range** If 'on', will show Crit Range.
    * **Extra on a Crit** If 'on', will enable the Extra on Crit field which can be used to specify damage to be added automatically on crits.
* **Automatic Higher Level Queries** If 'on', the script will query the player to cast spells at a higher level whenever possible.
* **Auto Use Ammo** If 'on', ammo will automatically decrement when launching offense items that are configured with ammo.
* **Revert advantage** If enabled, (dis)advantage will automatically be reverted back to rolling normally after the next d20 roll. Useful for the most common case where you get advantage on a single roll.
* **Auto spell slots/points** If enabled, (dis)advantage will automatically be reverted back to rolling normally after the next d20 roll. Useful for the most common case where you get advantage on a single roll.
* **Houserule Settings** See [!New Characters/Houserule Settings][#new-charactershouserule-settings].
* **Measurement Systems** Choose between imperial or metric units:
    * **Distance System** Select feet or meters.
    * **Weight System** Select pounds or kilograms.
* **Hide Settings** This submenu configures the default settings for the various "hide" options from the settings page of the character sheet. These settings are designed for use with a browser extension to help mask parts of the sheet output (see character sheet documentation for more details).
* **Default token actions** Configures which token actions will be created automatically for the character. For details of what each of these actions are, please see the documentation for [!shaped-abilities](#roll-hp-for-monsters).

## New Characters/Houserule Settings
* **HP Recovered Long Rest** Configures how much HP should be restored on a long test.
* **HD Recovered Long Rest** Configures how many Hit Die should be recovered on a long rest.
* **Critical Damage** Configures the rule to apply for crits, normal rules (2x damage), max dice roll or none.
* **Proficiency Dice** Enables the use of proficiency dice.
* **Psionics** Enables the rules for Psionics.
* **Custom Classes** Enables Custom classes.
* **Expertise as advantage** Configures the default value of the "expertise as advantage" option on the character sheet.
* **Medium Armor Max Dex** Sets the default value for medium armor max dex setting on the character sheet. This changes the maximum dexterity bonus allowed when wearing medium armor.
* **Base DC** Configures the default value for Base DC from the character sheet.
* **Honor** Enables the Honor system on the character sheet.
* **Sanity** Enables the Sanity system on the character sheet.
* **Multiple Inspiration** Allows multiple levels of inspiration on the character sheet.

## New Characters/Houserule Settings/Saving Throws
* **Half Proficiency Saves** Will tick the box for adding half proficiency to unproficient saving throws.
* **Use Custom Saves** Will turn on the option to use custom saving throws for new characters.
* **Use average of Highest Abils** If true, the custom saves configured below will use the average of the two highest abilities assigned to them rather than just taking the highest ability.
* **Fortitude, Reflex, Will** These submenus configure the three custom saving throws for new characters. Under each one, you can assign which abilities are used to generate the respective saving throw.

## Character Sheet Enhancements
* **Roll HP On Drop** If 'on' the script will automatically Roll HP for any character whose default token doesn't have a linked attribute for the specified bar. See the [Roll HP for monsters](#roll-hp-for-monsters) section for more details.
* **Process HD Automatically** If 'on' the script will automatically decrement your HD count when you roll hit dice and will also add the amount rolled to your HP total.
* **Process Uses automatically** If 'on' the script will automatically decrement Use count for abilities with limited uses per resting period.
* **Recharge uses on new turns** Automatically reset uses on "per-turn" abilities at the End of Turn.
* **Show ammo recovery buttons** Enable buttons to automatically process recovered ammo.

## Houserules & Variants
* **Long Rest: No HP, full HD** If 'on', the long rest command will reset all HD to full, but will not restore any HP.