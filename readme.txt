Motion Lab v9.11: 2-3 phone 3D skeleton tracking + cube game.
Deploy the folder with index.html at the site root to Netlify (same as before).
Keep the "backdrops", "panos", "music" and "motions" folders inside it.

1. Phone A: Create room, share invitation. Phone B (and optional Phone C):
   open the invitation, Join room. Phones get letters B, C in join order.
2. Enable camera + skeleton on every phone (rear 1x camera recommended).
   Tracking: body only, or body + detailed hands (21 finger joints per hand).
3. Phones on stands, not moving. On Phone A tap "Start alignment" and walk
   around ~10 s, arms up and out. With 3 phones, put C on the other side.
4. Live 3D skeleton with "Show" menu (body, upper, hands, left/right hand...).
   Record + export CSV/JSON.
   "Start raw session" saves every phone's 2D tracking with timestamps
   (send this file for tuning against real data).
5. Cube game: 4x4x4 see-through cubes lock in front of the player. Reach into
   the glowing cube and HOLD (0.5 s default) - the hold restarts if the hand
   leaves the cube, so swiping through doesn't count. Times each cube, shows
   average/fastest, keeps a personal best. Views: behind player / mirror / above.
   Full-screen button for a TV or laptop.

Demo mode simulates a person and 2 or 3 virtual phones; the simulated person
plays the cube game by itself.

How 3 cameras work: B and C are each aligned to A, C is put on B's scale using
joints all three see, then each joint is placed using every camera that sees it
(in simulation: ~30% lower error than 2 cameras, and a joint hidden from one
phone can still be placed by the other two).

Known limits: lens distortion not modelled; FOV setting must roughly match the
lens (default 67 deg = iPhone 1x); accuracy is a few cm.

New in v6.1 (from the first real 3-device raw session):
- Alignment solver retries from several starting points (a single attempt
  failed ~1 in 8 times on real data).
- Joints near the frame edge are trusted less and left out of alignment
  (MediaPipe guesses cut-off joints).
- With 3 cameras, a camera whose view of a joint disagrees with the others is
  dropped for that joint, and each camera is weighted by how well it aligned.
- Slower devices: frames up to 500 ms apart are still used.
- Setup check under each phone: frame rate, edge-touching, feet in view,
  off-centre, with what to fix.
On the real session: bone-length wobble 15% -> 7.4% (3 cameras).

New in v6.2:
- Cube game "Cubes centred" option. Default: where Phones A and B are aimed
  (the point where their lines of sight pass closest), fixed in the room and
  facing Phone A. The other option keeps the old behaviour (in front of the player).
- Alignment report shows the aim point's height and how closely A's and B's
  lines of sight meet, with tips if they miss or the point is too high/low
  for comfortable reaching (best around 0.6-0.7 x the player's height).
- 3D view draws the aim point and both cameras' dashed lines of sight.

New in v6.3:
- "Style" menu in the 3D view: Skeleton or Snowman suit. The snowman is built
  live from the 3D joints: hip/chest snowballs, head ball that turns with your
  head (coal eyes + smile, carrot nose), top hat, red scarf, coal buttons,
  twig arms with twig fingers (real finger joints with detailed hands on),
  snow-pant legs, boots, floor shadow and falling snow. Also used in the cube game.

New in v6.4:
- Full-screen button for the live 3D view (real full screen on computers,
  Android and iPad; fills the window on iPhone). Floating toolbar: costume,
  3D/Front/Side/Top, Exit. The cube game's full screen works the same way.
- Five more costumes in the Style menu, all driven by the live joints:
  Dragon (bat wings stretched between arms and hips - raise your arms to spread
  them - swaying tail, back spines, horned snout, slit-pupil eyes, claws),
  Human skeleton (skull with sockets and teeth, spine, ribcage, collarbones,
  pelvis, paired forearm and shin bones, finger bones), Witch (robe that
  follows your legs, flared sleeves, bent pointy hat, long hair, long nose with
  a wart, striped stockings, curly boots), Gymnast female (sparkly long-sleeve
  leotard, hair bun), Gymnast male (singlet with stripe, long white gym pants).

New in v7 - Ping pong (step 6):
- Full-size table (2.74 x 1.525 m, 76 cm high, regulation net) appears in front
  of you after a 3 s countdown. Your paddle follows your chosen hand in 3D.
- Camera tracking runs ~100-200 ms behind; the paddle is drawn where your hand is
  predicted to be now (line fit through the last 0.3 s of capture-timestamped
  hand positions).
- Real ball physics: gravity, air drag, table bounces, net, floor. Hits combine
  paddle speed and ball speed.
- Shot assist: Strong (your swing picks direction/speed, shot always lands on the
  table, generous hit zone), Light (can go long/wide), Off (pure physics, exact
  paddle face). Game speed 60/80/100%. Paddle: Big or regulation. Paddle angle:
  auto (faces the bot) or from your hand.
- Robot opponent Easy/Medium/Hard: reaction delay, limited speed, aiming error,
  occasional misses (more on fast or wide shots).
- Casual rules: ball must land on the other side; 2 serves each; to 11, win by 2;
  serve by swinging at the floating ball.
- Views: behind player, side, top; full screen; ball shadow on the table for
  depth; hit sounds; your costume (faded) if one is selected.
- Demo mode: the simulated person plays full rallies against the bot.

New in v7.1 - polished interface + documentation:
- App bar with live status chips (role/phones, cameras live, aligned) and Help.
- Four tabs: Setup, 3D view, Cubes, Ping pong (bottom tab bar on phones).
- Setup is a 3-step checklist (Connect, Cameras, Align) with automatic
  Start here / Next / Done badges. Raw session moved to Setup > Diagnostics.
- Consistent fields (labels above controls, 16 px inputs so iPhone doesn't zoom),
  one primary button per card, unified status callouts.
- View/full-screen controls float over each 3D canvas; on wide screens the 3D
  view and games get a side panel for settings, readouts and results.
- Phones B and C see "This phone is a camera" on the 3D/game tabs.
- help.html: setup checklist and troubleshooting for every message and problem.
- architecture.html: how it works and every component it uses.
Deploy the whole folder (index.html, help.html, architecture.html, README.txt).

New in v7.2 - Playhouse tab:
- Your costumed character performs live in front of a backdrop picture.
- Upload a backdrop per character (or a default for all); pictures are shrunk
  and remembered on the device. Built-in theatre stage when none is set.
- Character size, floor line (so feet land on the picture's floor), sideways
  position, mirror mode, keep-centred, full screen, and a Snapshot (PNG) button.
- Help and Architecture pages updated.
- help.html now includes copy-ready text-to-image prompts for a backdrop for
  every character (Help > Making backdrops with an image generator).

New in v7.3 - Duck shoot (finger-gun game on your TV):
- Make a finger gun; aim = line from your eyes through your index fingertip;
  fire = press your thumb down onto your index finger (detailed hands needed).
  Fallbacks: "hold steady" firing, or tap the picture.
- 5-point aim calibration (or Quick aim) maps your pointing onto the TV wherever
  it is in the room.
- TV display: open the TV link (role=tv) in the TV's browser or a laptop on the
  TV; it joins as a display only and Phone A streams the game to it (~15 KB/s).
- Waves of ducks, 3 shots per wave, 10 ducks per round, rising difficulty,
  score/tally/best, sound effects. Original artwork.
- Demo: the simulated person calibrates against a virtual TV and plays.

New in v7.4:
- "Update" tab with a button to the Netlify deploys page
  (https://app.netlify.com/projects/ornate-phoenix-320c9e/deploys), the update
  steps and the running version. Phone tab bar now swipes sideways.

New in v7.5:
- Playhouse "Cycle characters": every 15 s the next character takes the stage
  with its own backdrop (fade + countdown bar). Option to skip characters that
  have no backdrop of their own. Works in full screen.

New in v8:
- Control tab: TV display, pointing calibration (moved from Duck shoot),
  couch control, live gesture monitor, guided tuning session.
- Gesture toolkit for both hands: open palm, fist, point, pinch, peace,
  thumbs up, finger gun (cocked/fired), hand raised, wave - with events any
  mode can use.
- TV display for every mode: 3D view, Cubes, Ping pong, Playhouse and Theremin
  stream to the TV as live video (Duck shoot stays natively drawn); the TV
  follows Phone A's tab. Theremin sound can play on the TV.
- Couch control: hold up an open palm (or wave) to open a big-tile menu on the
  TV; point and pinch (or hover) to switch modes, change costume, start/stop.
- Theremin tab: classic (antenna + volume loop) and simple modes, scales, key,
  range, glide, 4 voices, vibrato, reverb, audio recording.
- Guided tuning session (spoken prompts, ~80 s) saves a labelled recording to
  send for tuning gestures to your setup.
- Demo can now run inside an open room (show the demo on a TV).
- Hidden ?peerhost=host:port/path option for a private PeerJS server.

New in v8.1:
- Playhouse flanking characters: pick a character for the left and/or right
  (or "same as main"). They stand a step behind you and copy your moves -
  in sync, with a 1/4 or 1/2 second echo, or a left-you-right ripple - and can
  mirror you. Adjustable spacing; choices are remembered.
- Help: full "Flanking characters" section (setup steps, every setting,
  ideas, and troubleshooting).

New in v8.2:
- Ripple timing now clearly flows left -> main -> right: left flanker moves
  with you, main character one step later, right flanker one step after that.
  New "Ripple step" (0.2 / 0.3 / 0.45 s, default 0.3) and "Ripple direction".

New in v8.3:
- Playhouse effects: Sparks, Rainbow trail, Magic stars, Fire and Bubbles
  pour out of a chosen body part (right/left/both hands, head, right/left/
  both feet, hips or whole body), follow it and fade. Faster movement makes
  more. Amount slider; optional effects on the flanking characters too.
  Shows in full screen, snapshots, on the TV and while cycling.
- Help: new "Effects" section (each effect, every setting, ideas,
  troubleshooting). Architecture: how the particle system works.

New in v8.4:
- Built-in Playhouse backdrops: every character now comes with its own
  picture (backdrops/<character>.jpg), so new users see them immediately and
  cycling characters changes scenery from the start. Uploads still replace
  them; Remove turns a built-in off; "Use built-in picture" brings it back.
- The site is now a folder with a "backdrops" subfolder - drag the whole
  folder onto Netlify.

New in v8.5:
- Demo person can now Dance, Twirl, Talk with hands, or Mix all three
  (plus the original Walk around). Pick it in Setup or from the menu at the
  top of every tab while the demo runs. Test the Playhouse, effects,
  flankers, theremin, gestures and TV without any cameras.
- Running the demo starts alignment automatically (~10 s).
- Help: new "Testing without cameras" section.

New in v8.6:
- Duck shoot: ducks now fly the right way up with the beak pointing the way
  they're flying (left or right), tilt up or down as they climb or dive, and
  flap their wings. They keep mostly sideways flight. Hit ducks fold their
  wings; falling ducks tumble beak-down.

New in v8.7:
- Playhouse 360 panorama view: tap "360" on the stage. The character stays
  in the centre while the camera circles round them, showing every side of
  the character and the whole panorama behind. Orbit speed (20 s - 1.5 min
  or off), direction, view width, horizon line; drag to turn by hand.
  Per-character panorama upload; built-in snowy-village panorama for the
  Snowman; other characters use their backdrop mirrored into a circle.
  Effects, flankers (depth-sorted), snapshot, full screen, cycling and the
  TV all work in 360 view.
- Help: "360 panorama" section with a panorama prompt template.

New in v8.8:
- New character: Raven. Glossy blue-black feathers, a heavy curved beak,
  bright-glint eyes, a shaggy throat ruff, scaly legs with four-toed talons
  and a wedge-shaped tail. Feathered wings follow your arms - raise them to
  spread the wings; spread your fingers to fan the long flight feathers.
  Available everywhere characters are (3D view, Playhouse, flankers, 360,
  cycling). No built-in backdrop yet: help has a backdrop prompt and a 360
  panorama prompt for it.

New in v8.9:
- Raven gets its built-in pictures: a moonlit standing-stone circle backdrop
  and a matching 360 panorama.
- 360 view: the "Horizon in the picture" setting is now remembered per
  character, and each built-in panorama starts at the right horizon.

New in v9.0:
- New demo motion "Raven things": the demo person struts with head-bobs,
  flaps, soars with wings out banking left and right, hops, caws, pecks
  at the ground and preens. Choose it in Setup or the menu at the top.

New in v9.1:
- Raven soundtrack: a dark & playful 35-second loop (pizzicato strings,
  harpsichord, cello, woodblocks, raven caws) composed on the same 150 bpm
  grid as "Raven things" and kept in sync with it - footsteps, wingbeats,
  hops, caws and pecks all land on the music. Turn it on in Setup or with
  the music button in the top bar while the demo runs; volume slider.
- Raven things moves retimed to the beat.

New in v9.2:
- Drums tab: an air-drum kit placed around you (snare, hi-hat, 2 toms,
  floor tom, crash, ride, bass drum). Hit down through the drums with your
  hands (harder = louder), stamp your right foot for the bass drum, lift
  your left foot to open the hi-hat. Early trigger hides camera delay.
  Kit height/size, sensitivity, foot options, drummer costume, behind or
  audience view, metronome, record / play back / loop. In demo mode the
  simulated person plays a rock groove with a fill. Works on the TV.

New in v9.3:
- Movement files: the demo person can perform a loaded routine. Built-in
  "Drum performance" (48 s: count-in, rock groove, ride groove, tribal
  toms, open hi-hats, fills, big ending) - Drums -> "Watch a drum
  performance" plays it in one tap. Load your own movement files or 3D
  recordings (3D view -> Record -> JSON) from Setup.
- Drum kit facing now taken from the hips; the kit waits until the
  player is standing still before placing itself.
- Fix: in v9.2 the demo's Dance / Twirl / Talk / Raven motions had
  stopped working (the person just walked). Fixed.

New in v9.4:
- "Theremin recital" movement file (about 2 minutes): Twinkle Twinkle
  Little Star, Ode to Joy, Amazing Grace and When the Saints Go Marching In
  (all public domain). Theremin -> "Watch a theremin recital" plays it in
  one tap (sets the theremin's settings, starts the demo and the theremin).
  The current song's title shows on the theremin screen.
- Movement files can carry theremin settings and a song list.

New in v9.5:
- A much more detailed Witch: flowing cloak (open at the front, sways as
  she moves) with purple lining, a ragged hem and embroidered gold stars;
  tall spiky collar; belt with buckle, leather pouch and two glowing potion
  bottles; necklace with a glowing crescent-moon amulet; bell sleeves with
  ragged cuffs; bony green hands with dark claws; wild grey-streaked hair;
  pointed ears, cheekbones, yellow slit-pupil eyes, angry brows, a hooked
  nose with a hairy wart, crooked grin with one tooth and a warty chin;
  crooked hat with a wavy brim, buckle, stitched patch and a spider
  dangling on a thread; striped stockings and curly buckled boots.

New in v9.11:
- The overlay buttons on every display (3D view, Cubes, Ping pong,
  Playhouse, Duck shoot, Theremin, Drums) fade away after 10 seconds without
  a touch and come back when the picture is touched (or the mouse moves over
  it). The revealing tap never presses a hidden button. Switching to a tab
  shows its buttons again.

New in v9.10:
- New demo motion "Gymnastics": a 40-second floor routine - salute,
  cartwheel, arabesque, chasse, split leap, pirouette, handstand opening
  into a split, cartwheel back, bridge, chasse, straddle jump, final
  salute. Leap/arabesque/handstand/bridge are performed side-on.
- Body model: the hips can now turn with the torso (needed upside down),
  knee/elbow bend directions follow the torso, and pointed toes continue
  the line of the shin.

New in v9.9:
- 3D characters (VRM): import rigged avatars in VRM format (VRoid Studio,
  VRoid Hub; VRM 0.x and 1.0) from the Playhouse's new "3D characters (VRM)"
  card. They join every character list (Playhouse, backup dancers, Drums),
  follow body, head, hands and fingers, are sized to the performer's legs,
  have spring-bone hair/clothes and blink. Stored in IndexedDB on the device.
- Built-in samples: "VRoid girl (sample)" (pixiv Inc., redistribution
  allowed) and "Seed-san" (VirtualCast, Inc., VRM Public License 1.0, credit
  shown on stage), in models/. The VRoid girl has a built-in sunset school
  rooftop backdrop and 360 panorama; Seed-san has a futuristic tech lab.
- three.js, GLTFLoader and three-vrm are now served from the site's lib/
  folder (import map) instead of a CDN; Pinocchio uses the same copy.

New in v9.8:
- Character songs in the Playhouse: each character can have its own song
  that plays (looping) while that character is on stage. Pinocchio has
  "I'm a Real Boy!" built in (music/pinocchio.mp3). Upload a song for any
  character from the new Character song card (stored on that device in
  IndexedDB); play/pause, restart, volume, remove, use built-in.

New in v9.7:
- Pinocchio is now a realistic 3D model instead of a flat cartoon: a
  varnished, carved wooden marionette with a sculpted face (eye sockets,
  cheekbones, painted lips and cheeks), glossy painted eyes that blink,
  carved hair, brass pins in the knees and elbows, articulated wooden
  fingers, a printed linen shirt, wool breeches, bark shoes and a lumpy
  bread-crumb cap. Lit with key, fill and rim lights, reflections and soft
  shadows, and rendered with each view's own camera (Playhouse flat,
  mirrored and 360, and Drums). Uses three.js loaded on demand; falls back
  to the painted puppet when offline or without WebGL.
- Pinocchio has a built-in workshop backdrop and 360 panorama.

New in v9.6:
- New character: Pinocchio, the wooden puppet from Collodi's 1883 book -
  jointed wooden limbs with ball joints and wood grain, a suit of flowered
  paper with a folded paper collar, bark shoes, a soft pointed cap of bread
  dough, carved hair, painted eyes and rosy cheeks. Touch your nose and his
  nose grows (leaves sprout and a little bird lands on it when it's long);
  let go and it shrinks back.
- Help: backdrop and 360 panorama prompts for Pinocchio's workshop.
