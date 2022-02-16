# Corruption Cards CCARDS Lump

The CCARDS lump for Corruption Cards customizes decks, card generation and more. It's useful for getting your own custom cards into the game, altering how cards are generated for your custom monsters or tweaking your Corruption Cards experience beyond what is offered in the options menu.

Simply create a new lump called CCARDS and add it to the pk3/wad of the mod you want to add it to. From there you can use any of the following:

## Deck Manipulation

Simply having a CCARDS lump in one of your mods will create the "MODIFIED" deck, for ease of use. This deck is a copy of the REGULAR deck, which contains all available Corruption Cards. Using the commands below you can alter what cards exist.

### `clearcards`
Removes all the previously defined cards from the deck. Useful if you want a blank slate approach.

### `addcard`, `classname`
Adds a card to the deck. The card is the full classname of the zscript Card class. You can add your own or pre-existing cards. Card classes are case-sensitive.

### `removecard`, `classname`
Removes a card from the deck. The card is the full classname of the zscript Card class. Card classes are case-sensitive.

### `allowdupicatemonstercards`
By default the game will not offer two of the same card type, even if they are for two different monsters. This changes that behaviour. Useful for very small decks.

### `nodifficultyscaling`
By default lower tier cards are removed from the game as your in game collection grows to make the game more difficult later on. This disables the removal of those cards. Note that the CUSTOM game mode setting has an option to override this setting.

## Card Generation

### `excludeactor`, `classname`
Prevents an actor from getting cards generated for it. Will also prevent it from being effected by certain card effects. Useful for hacky actors that use ISMONSTER for some reason.

### `addmonster`, `classname`
Forces the generator to consider the specified actor class as a valid monster for card generation. Corruption Cards implements various checks to make sure cards are only generated for monsters that make sense to have cards for them. This overrides those checks.

## Game Options

### `excludemap`, `mapname`
All maps except TITLEMAP or HUBMAP offer the player cards at the start of the game. This prevents the map from offering players cards (but any permanent cards will still be active).

### `cleardeckmap`, `mapname`
Upon entering this map, the player's permanent card collection is stripped. Useful for map sets that a game/episode selection hub.

### `setupdelay`, `tics`
Corruption Cards begins the card generation and selection process after 4 tics, to allow certain custom actors to initialize and set up. You can increase this delay if necessary if your mod requires longer.

### `dontfreeze`, `classname`
At the start of the game, all monsters get frozen (deactivated) before the game starts. This prevents them from being frozen. Certain custom monsters that have an activate/deactivate state, which this is useful for. 

## Monster Groups

Cards will be generated for individual monster types. However many cards have a `usespecies` flag which allows them to effect all monsters of that species. Certain mods use species effectively to group certain types of monsters together (i.e an Imp, a bigger Imp, a fire infused Imp). Other mods have every monster flagged as the same species, which can be a problem as certain cards designed for one monster now effect all monsters.

By setting up your own custom monster groups you can control which monsters get the shared card effects. If a monster is associated with a group set up in CCARDS, its species will not gain the effects of the card, but other members of the group will instead.

### `monstergroup`, `groupname`, `classname1`, `classname2`, `...`

Creates a new monster group. `groupname` is the name of the group displayed on the card selection screen. The following classes, separated by a comma, will be added to the group.

### `monstergroupsprite`, `groupname`, `graphic`

Adds a custom image for an already defined monster group. The image must be 68x102 pixels to correctly fit inside the card.

### `monstercardsprite`, `classname`, `graphic`

Adds a custom image for the monster. Corruption Cards will try to grab one from the sprite currently used in the world, but certain actors maybe too large or look wrong when displayed on a card. This allows you to manually change it to `graphic`. If the monster is part of a group, it will only be displayed for cards that have `usespecies` disabled.

# Examples

A simple CCARDS lump that adds a custom card you've made.

```
addcard CCard_MyCustomCard
```

A CCARDS lump that removes all the previous cards and adds some random favourites.

```
clearcards
addcard CCard_ExtraCurses
addcard CCard_MutatingCard
addcard CCard_SpawnIllusions
addcard CCard_RescueMission
addcard CCard_Specialists
addcard CCard_DuplicationCurseMonster
addcard CCard_LethalityCurseMonster
addcard CCard_ResurrectionCurseMonster
addcard CCard_ClownCarMonster
addcard CCard_MirrorCurseMonster
```

A CCARDS lump that sets up some custom monster groups for a monster replacement mod.

```
monstergroup "Zombies", PistolZombieMan, ChainsawZombieMan, RifleZombieMan
monstergroup "Imps", MyImp, BigImp, FireImp
monstergroup "Floaty Boys", Cacodemon, NastyCacodemon, CacodemonBoss
```

Optional images for the same imaginary mod above and their groups.

```
monstergroupsprite "Zombies", CCGZOMB
monstergroupsprite "Imps", CCGIMPS
monstergroupsprite "Floaty Boys", CCGFLOAT

monstercardsprite "FireImp", FIMPCARD
monstercardsprite "DarthMastermind", MSTRCARD
```
