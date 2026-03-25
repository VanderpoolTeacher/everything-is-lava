# Everything is Lava

A 3-level Neo Tokyo street runner built with HTML5 Canvas and vanilla JavaScript. Play as the Get Good Dad ninja escaping a city consumed by lava — jump pools, dodge vehicles, climb streetlamps, and survive the volcanic apocalypse. No dependencies, no build step, just open and play.

## How to Play

Open `index.html` in any modern browser.

**Controls:**
- **Jump**: Space, W, or Up Arrow
- **Double Jump**: Press jump again while airborne
- **Crouch**: Down Arrow or S (while grounded)
- **Move Left/Right**: A/D or Left/Right Arrows
- **Mute/Volume**: M (mute), +/- (adjust)
- **Reset to Menu**: Q
- **Help Overlay**: ? or /
- **Mobile**: Tap to jump

**Secret Codes:**
- **Konami Code**: Up Up Down Down Left Right Left Right B A, then press 1/2/3 to skip to that level, or S to jump to the story scroll
- **Dojo**: Type GGD on the title screen to enter the practice dojo

**Dojo Controls:**
- **N**: Next music track
- **I**: Toggle birds
- **O**: Toggle water drops
- **T**: Start timed trial

**Goal:** Survive all three levels (1-1, 1-2, 1-3), reach the village at the edge of the city, and receive the emerald from the mysterious hooded figure. Bonus points for remaining lives — keep all 5 to earn Grand Legend status.

## Features

- 3 handcrafted levels with increasing difficulty across a Neo Tokyo cityscape
- Animated pixel-art ninja with walk cycle, jump spin, double-jump spin, and crouch
- Lava rain from the sky that forms pools on the street
- Emergency vehicles (police cars, fire trucks with extended ladders, school bus) as moving hazards
- Jumpable streetlamp platforms with flickering lights
- 5-life system with invincibility frames on respawn
- Movie-style opening credits and title scroll with original lore
- Story scroll finale with emerald figure, score breakdown, and life bonus
- Dojo practice mode with birds, water drops, dodge/hit counters, and timed trials
- Village ending with shadowed hut, torches, and ambient transition
- Master volume control, multiple music tracks per mode
- Konami code with level-skip and story-skip options
- Fully responsive canvas, keyboard + touch controls

## Tech Stack

- HTML5 Canvas API
- Vanilla JavaScript
- Zero dependencies
- Single-file architecture (`index.html`)

## Assets

- `assets/logo-get-good-dad-standard-blue.png` — Ninja logo reference for player character
- `assets/hooded-figure-green-emerald-transparent.png` — Emerald figure for story scroll
- `assets/ggd-clean.png` — Get Good Dad sprite for credits
- `music/` — Level music, title theme, game over, dojo training tracks, victory music

## Build Prompts

This game was vibe-coded with Claude (Anthropic) across multiple sessions. Below is every prompt used, in order, to produce the current version of the game.

### Session 1 — Initial Build (Prompts 1–4)

**Prompt 1** — Initial Concept
```
let's make a game
```

**Prompt 2** — Game Identity
```
it a runner game called Everything is Lava
```

**Prompt 3** — Character Design
```
ok... decent concept, let's make a character from the assets folder
```

**Prompt 4** — Repo Setup
```
make repo and connect to: https://github.com/VanderpoolTeacher/everything-is-lava
```

### Session 2 — Neo Tokyo & Level Design (Prompts 5–19)

**Prompt 5** — Level Theme
```
ok.... we are going to make it have levels - start with city street... neo tokyo look and feel is level one. lava drops from the sky and makes pools that need to be jumped over
```

**Prompt 6** — Scrolling Ground
```
the street/ground should scroll
```

**Prompt 7** — Lava Pool Fix
```
the lava pools stay in one place on the street, they should scroll with the city
```

**Prompt 8** — Character Bug
```
the 32 bit characater is just a blue square now?
```

**Prompt 9** — Character Asset
```
try with the new 32 bit ggd
```

**Prompt 10** — Background
```
extend the volcano image to fill the bakcgorund
```

**Prompt 11** — Background Fix
```
use volcano bg for backgrond
```

**Prompt 12** — Emergency Vehicles
```
an emergenchy vehicle needs to come in at the 1/3 point and then randomly in the 2nd half of the level
```

**Prompt 13** — Vehicle Style
```
make the vehicles 32 bit
```

**Prompt 14** — Level Music
```
iuse the song in music for the level
```

**Prompt 15** — Streetlamps
```
add street lamps that you can jump off of
```

**Prompt 16** — Spacing & Size
```
make character slightly larger, evenly space street lamps make a few of them flicker
```

**Prompt 17** — Character Fidelity
```
keep get good character close to original design
```

**Prompt 18** — Lamp Height
```
make street lights taller so you can only access them with double jumps
```

**Prompt 19** — Lava Pools & Character
```
make the lava pools taper at the edges, also redo character and make a bit larger
```

### Session 3 — Animation & Difficulty (Prompts 20–26)

**Prompt 20** — Character Bug Again
```
the character is just a blue square
```

**Prompt 21** — Animation
```
can you animate the character slightly, walk and spin on jump, and kick when pressing arrow forward
```

**Prompt 22** — Double Jump Spin
```
add spin on double jump
```

**Prompt 23** — Spin Polish
```
the spin on the double jump needs better animation
```

**Prompt 24** — Lava Distribution
```
in the first half of the level, 10% of lava should drop on right 1/3 of screen, 2nd half raise that to 25%
```

**Prompt 25** — Vehicle Audio
```
sound of emergency vehilce whouls start before it appears
```

**Prompt 26** — More Lava
```
need more lava drops on right side of screen
```

### Session 4 — Dojo & Game Over (Prompts 27–35)

**Prompt 27** — Dojo Mode
```
when you press the key sequence g g d on the title screen, go into a demo screen where players can practice charcater movement with nothing else happening, make it a dojo setting
```

**Prompt 28** — Game Over Music
```
use game-over song
```

**Prompt 29** — Game Over Scroll
```
scroll up this on the game over screen: GAME OVER: The Legend of Get Good Dad
```

**Prompt 30** — Music Check
```
did you add the game over music?
```

**Prompt 31** — Debug
```
now look
```

**Prompt 32** — Lives System
```
give the player 5 lives, use icons of the chacter to count lives - after 5 lives - give game over
```

**Prompt 33** — Game Over Text
```
on game over, change press start to try again to press start to get good
```

**Prompt 34** — Dojo Music
```
use dojo training song for dojo
```

**Prompt 35** — Dojo Music Switching
```
add dojo training 2 song as option - let user change song with M
```

### Session 5 — Vehicles, Lore & Polish (Prompts 36–50)

**Prompt 36** — More Vehicles & Bus
```
need more vehicles coming in from right, also, add a school bus of screaming kids at exactly 1/3 through the level - make it slighly larger than the other vehicles
```

**Prompt 37** — Title Scroll Lore
```
on the title screen, add a scroll of the legend of get good dad - a new once you make up... at the end, mention the dojo
```

**Prompt 38** — Font Consistency
```
all text in game should use that font
```

**Prompt 39** — Level Count
```
the game only has 3 levels
```

**Prompt 40** — Title Music
```
i want the title song to play when the game loads adn theroughout the title scroll
```

**Prompt 41** — Title Screen
```
increase font on title screen, leave title graphic up longer
```

**Prompt 42** — Title Scroll & Lava Sound
```
increase title scroll font size, add louder slop sounds when lava hits
```

**Prompt 43** — Lore Addition
```
add the line "he is the only hope." into the title scroll
```

**Prompt 44** — Vehicle Explosions
```
exploding animation when vehicles hit lava and sound
```

**Prompt 45** — Duplicate Text Fix
```
press space to start is on the screen twice - rmove smaller one
```

**Prompt 46** — Vehicle Destruction
```
when the vehicles hit lava, have them break apart, also add a bit more shake
```

**Prompt 47** — Level 3 Difficulty
```
on level 3, randomly remove the light poles
```

**Prompt 48** — Level 2 Tweak
```
level 2, remove a random street lamp early in the level, just one
```

**Prompt 49** — Game Over Polish
```
game over screen - remove flashing press start until end
```

**Prompt 50** — Lava Visual
```
make the lava drops flash as they come down
```

### Session 6 — Level Flow & Audio (Prompts 51–62)

**Prompt 51** — Level Transitions
```
go right into the next level screen
```

**Prompt 52** — Bus Screams
```
make it so school bus of kids make loud scream sound
```

**Prompt 53** — Vehicle Volume
```
raise the other vehicle sounds slightly
```

**Prompt 54** — Victory Screen
```
add congratulations 01 to end win state
```

**Prompt 55** — Music Update
```
update the mp3 with the new file
```

**Prompt 56** — File Confirmation
```
that's the new file
```

**Prompt 57** — Konami Code
```
if the user inputs up up down down left right left right a b spacebar, take them to end congratutlsions - that is the konami code
```

**Prompt 58** — Victory Story
```
update teh congratulations screen to be, he made it to the edge of the city, thousands died but many more survive, he must make it to the volcano to stop the demon with it from destryong the world
```

**Prompt 59** — Konami Input Fix
```
how to enter konami code? if user presses the up arrow, don't start game
```

**Prompt 60** — Victory Polish
```
remove press start from congratulations screen
```

**Prompt 61** — Konami Score Fix
```
when konami code is used - make score 0000, there is some odd scrolling anition of get good character on win screen
```

**Prompt 62** — Opening Credits
```
when the game starts, fade in credits like this is a movie of actor - get good dad, director - michael vanderpool, music - suno
```

### Session 7 — Bus, Bonus & Audio Fixes (Prompts 63–73)

**Prompt 63** — Bus & Credits Timing
```
make bus come in by itself first time on level 1, make the credits over the beginning of play9ng lebel 1
```

**Prompt 64** — Bus Audio
```
the bus shouldn't have a sired, just screams
```

**Prompt 65** — Life Bonus System
```
add a bonus when you complete the game of 1000 points each live and 10,000 if all 5 lives. if someone complete with all 5 lives, label them the goodest of get good dads - grand legend status
```

**Prompt 66** — Bus Siren Removal
```
remove sirens from bus
```

**Prompt 67** — Bus Audio Fix
```
the bus still makes siren sounds, needs to be more like screams
```

**Prompt 68** — Bus Sound Design
```
bus sounds like wooshes - can it hand high and low yells?
```

**Prompt 69** — Bus Sound Retry
```
bus sounds like wooshes - can it hand high and low yells?
```

**Prompt 70** — Audio Error Fix
```
there is an audio error when you beat the game
```

**Prompt 71** — Git Push
```
commit and push
```

**Prompt 72** — Repo Connection
```
connect to this repo
```

**Prompt 73** — Repo URL
```
connect to this repo: https://github.com/VanderpoolTeacher/everything-is-lava
```

### Session 8 — Fire Trucks, Background & Speed (Prompts 74–101)

**Prompt 74** — Ladder Truck Variant
```
on level 2, create a variation of the fire truck that has the ladder extended up, use this one 10% of the time
```

**Prompt 75** — Git Check
```
is this folder a git repo?
```

**Prompt 76** — Confirmation
```
yes
```

**Prompt 77** — Score Display
```
add total score to congratulations screen
```

**Prompt 78** — Ladder Frequency
```
the extended ladder should appear at least once in the 2nd level
```

**Prompt 79** — Title Glitch
```
there seems to be a background color gltch in the opening title crawl
```

**Prompt 80** — Commit
```
commit
```

**Prompt 81** — Mechanics Lock
```
log that this is the point where game mechanics are locked for the game jam - since i just beat it in this state with all lives - don't change any mechanics until we say we are doing a new version on a new branch
```

**Prompt 82** — Title Animation
```
on the title screen have the everything is lava graphic slowly get larger
```

**Prompt 83** — Fade Fix
```
the fade is still off
```

**Prompt 84** — Commit
```
commit
```

**Prompt 85** — Title Scroll Bug
```
the title scroll first few lines of text is still fading / disappearing and reappeareing
```

**Prompt 86** — Scroll Speed
```
speed up opening title scroll slightly
```

**Prompt 87** — Speed Regression
```
somethign you did changed teh gaem speed - go back and make it like it was
```

**Prompt 88** — Revert
```
go back to the version where i said ot lock game mechanics
```

**Prompt 89** — Speed Issue
```
everything still feels slow
```

**Prompt 90** — Speed Boost
```
make the gameplay 50% faster
```

**Prompt 91** — Speed Debug
```
it's still not playing at the speed - could it be a browser issues?
```

**Prompt 92** — Hard Refresh
```
i did a hard refresh - go back before lock to see what game speed was
```

**Prompt 93** — Time Check
```
how much time have i spent building this?
```

**Prompt 94** — Speed Boost Retry
```
okay, speed everything up by 50%
```

**Prompt 95** — Browser Issue
```
so it plays right in the display, but slow in chrome.... so go back to when we locked the game please and update everythign to match the lock
```

**Prompt 96** — Safari vs Chrome
```
it works fine in safari, someothing is wrong in chroe
```

**Prompt 97** — Title Zoom
```
make the title graphic zoom in slowly
```

**Prompt 98** — Commit
```
commit
```

**Prompt 99** — Volume Reduction
```
reduce the sirens and game noises slightly
```

**Prompt 100** — Volume Reduction (Repeat)
```
reduce the sirens and game noises slightly
```

**Prompt 101** — Confirmation
```
yes
```

### Session 9 — Optimization & Polish (Prompts 102–113)

**Prompt 102** — Performance Pass
```
i've noticed the game does lag occasionally, can you do a pass to make sure it's efficient
```

**Prompt 103** — Commit
```
commit and log
```

**Prompt 104** — Settings Question
```
how do you access settings?
```

**Prompt 105** — Cache Clearing
```
when the game is over, make sure to clear all cache to start over
```

**Prompt 106** — Commit
```
commit and log
```

**Prompt 107** — Building Lights
```
on the buildings in the background, keep many of the lights on an constant, only flicker a few
```

**Prompt 108** — File Cleanup
```
we should onlyh ahve the index file - delete the everything-is-lava one
```

**Prompt 109** — Flicker Fix
```
the building lights are flickering a lot
```

**Prompt 110** — Window Flicker Fix
```
in the game play itself, the lights are still flickering on the building windows
```

**Prompt 111** — Remove Kick
```
remove the kick animation on arrow left and right
```

**Prompt 112** — Regression
```
you broke something
```

**Prompt 113** — Title Scroll Bug
```
the opening title scroll is fading and disappearing - should stay same
```

### Session 10 — Village Ending & Final Polish (Prompts 114–133)

**Prompt 114** — Village Ending
```
at the end of level 3, make it exit the city and end at a house in a village
```

**Prompt 115** — Respawn & Ladder
```
add a small amount of invincibility when respawning - make the firetruck ladder a hit box
```

**Prompt 116** — Village Shadows
```
have the village house be cast in dark shadows
```

**Prompt 117** — House Scroll
```
the house shouldn't fade in, it should scroll in from the right... also, near the end, everything should go into shadow, including all the buildings in the backgorund
```

**Prompt 118** — House Scroll Fix
```
the house shouldn't fade in, it should scroll in from the right... also, near the end, everything should go into shadow, including all the buildings in the backgorund - don't fade, scroll in
```

**Prompt 119** — Village Detail
```
as we near the end of level 3, reduce the number of buildings in the background, don't show hills, just transition from road to grass, the hut that scrolls in should be a bit larger and fully in shadow with a candle light flickering inside; on the city buidlings, slow down and randomize the flikcering of the red lights on top of the buildinds
```

**Prompt 120** — Memory Optimization
```
do one more pass to make the game memory efficient
```

**Prompt 121** — Cache Clear
```
can you make it clear browser cache for game on startup?
```

**Prompt 122** — Itch.io Requirements
```
check itch.io requirements for submitting html5 games - https://itch.io/docs/creators/html5
```

**Prompt 123** — Canvas Size Check
```
does our game match the canvas size?
```

**Prompt 124** — Performance Issue
```
something is slowing the game down after repeated plays in the browser?
```

**Prompt 125** — Persistent Slowdown
```
it's still slow after multiple plays in different browsers - anything we can try?
```

**Prompt 126** — Konami Level Skip
```
when we do the konami code up up down down left right left right b a - the number typed after should take you to that level - when using the konami code, end screen total score should always be zero
```

**Prompt 127** — Help & README Keys
```
when player hits the R key - take them to the readme file; the ?/ button should show all keys
```

**Prompt 128** — Village Torches & House
```
As the village approaches, have torches outside the house to make it easier to see... redesign the house
```

**Prompt 129** — Dojo Cleanup
```
in the dojo remove the kick arrow keys text
```

**Prompt 130** — Background Bug
```
as the game scrolls, the background image seems to float to the left, and then leaves artifacts on the right side of the screen
```

**Prompt 131** — Display Bug
```
now the game doesn't show at all?
```

**Prompt 132** — Village Audio
```
as you near the village, reduce the volume of the music
```

**Prompt 133** — Commit
```
commit and log
```

### Session 11 — Submission & Story (Prompts 134–141)

**Prompt 134** — Itch.io Description
```
write this descripotn fo the game... i am 5 miles from the maumee: To make sure everyone can play and rate your game fairly, please include the following in your itch.io submission description: 1. The Local Legend Metric (Required) How close are you to the source? 2. Your Development Category We embrace all forms of creation, but we value transparency. Please label your game as one of the following: 3. Your Tech Stack 4. The "Forward Motion" Implementation
```

**Prompt 135** — Death Volume Bug
```
the music gets louder after a death
```

**Prompt 136** — Level Renaming
```
rename the level, to 1-1, 1-2, and 1-3
```

**Prompt 137** — Asset Check
```
do you see th green emerald image in the assets folder
```

**Prompt 138** — Story Scroll
```
let's replace the winning condition scroll at the end of level 1 with a sstory about how, you made it through the city, saved lives, but are not done... the man gives you the green emerald and you must return it to the heart of the volcano, include the image in scoll - make it sticky at the top of the page as texts scrolls into it
```

**Prompt 139** — Ladder Hitbox
```
all firetrucks in 1-1 and 1-2 should have the ladder extended, the top of the ladder is a hitbox you can stand on, the side of the ladder is a hurt box
```

**Prompt 140** — Police Speed
```
increase max speed of police cars by 35%
```

**Prompt 141** — Story Placement Fix
```
the emerald story... that doesn't happen at the end of 1-1, it should replace the end of after 1-3
```

### Session 12 — Story Scroll, Dojo Features & Final Polish (Prompts 142–158)

**Prompt 142** — Scroll Spacing
```
the line spacing on the end scroll ins't consistent
```

**Prompt 143** — Score Display
```
make sure to include the score and the bonus points for remaining lives
```

**Prompt 144** — Score Display (Repeat)
```
make sure to include the score and the bonus points for remaining lives
```

**Prompt 145** — Score & Image
```
put the score at the end of the scroll - use the png of the emerald guy, make the emerald guy 25% larger
```

**Prompt 146** — Konami S Shortcut
```
add the option of ending the konami code with S to take them to end of level 1 credits
```

**Prompt 147** — Scroll Rewrite
```
the end credits still has spacing issues - some lines are closer to each other than others, make it all equally spaced with an extra line between paragraphs, also use the new png in the folder, just have hte emerald guy image scroll up with text
```

**Prompt 148** — Scroll Stop
```
have everything is lava and text below it stop in the middle of the screen
```

**Prompt 149** — Dojo Birds & Water
```
in the dojo, make N go to next music track, and I will toggle bring in flying birds from the right side to replicate moving objects from th game, and O will toggle dropping water - nothing kills the hero, just stuns him briefly
```

**Prompt 150** — Water Warning
```
in the dojo, have a spot on the floor flash where the water is going to drop
```

**Prompt 151** — Dojo Scoring
```
in the dojo, when water and birds are turned on... do a counter to keep score
```

**Prompt 152** — Dojo Hint
```
add to title screen, type ggd to enter the dojo
```

**Prompt 153** — Dojo Hint (Repeat)
```
add to title screen, type ggd to enter the dojo
```

**Prompt 154** — Timed Trial
```
add T to dojo to activate birds and water on a timer, same as level progress - count score of hits
```

**Prompt 155** — Trial Warning Style
```
in the trial, make the drop indicators quick white flashes
```

**Prompt 156** — Commit
```
log and commit
```

**Prompt 157** — README Update
```
update the readme file to include every prompt
```

**Prompt 158** — History Question
```
do you remember the original version of the game - what you made first?
```

## Project Structure

```
Everything is Lava/
├── index.html                # The game (single file, open in browser)
├── assets/
│   ├── logo-get-good-dad-standard-blue.png    # Ninja logo reference
│   ├── hooded-figure-green-emerald-transparent.png  # Story scroll figure
│   └── ggd-clean.png                          # Get Good Dad sprite
├── music/                    # All game music tracks
├── github-issue-level-2-1.md # Level 2-1 design doc
├── CHANGELOG.md              # Version history
└── README.md                 # This file
```

## License

Built for the Toledo.codes March Game Jam (Category B: Vibe-Coded). Theme: "Forward Motion."
