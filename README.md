# Outpost Ares — Mars Base Commander

A 3D browser game for ages 7–12. You are an astronaut commanding a small base in Jezero Crater.
Keep the crew alive for 10 sols while doing as much science as you can. Every radiation storm in
the game is a real solar event recorded by NASA.

## Play

Open `index.html` in any modern browser (double-click works). No install, no account.
To put it online for judges, upload the folder to any static host (GitHub Pages, Netlify, itch.io).

It needs an internet connection once, to load the three.js 3D library from a CDN. The live NASA
download is optional: if it fails, the game falls back to the archived NASA data built into the file.

Controls: **W A S D / arrows** or click/tap the ground to walk · **E** to use a building ·
drag to look · scroll to zoom · **R** rover orders · **M** ask Mission Control ·
**Esc** pause menu (resume, restart, leave).

## What's in the latest version

- **Mission length:** choose 3, 5, 7, 10, 15, 20 or 30 sols on the start screen. One sol lasts 3 minutes of play.
  In shorter missions the window is placed so the real storm day still falls inside it.
- **Tutorial:** "New here? Play the tutorial" runs a ten-step coached training mission (move, look,
  power levels, a repair, a daily task, sampling, the rover, sheltering).
- **Every facility has an interior:** habitat, greenhouse, science lab, comms room, robot garage and
  power house. Each holds one daily task played as a short hands-on mini-game (change the air
  filter, harvest crops, study a rock, download tomorrow's Sun forecast, tune up the robot,
  rebalance the battery cables). Each task gives a real benefit and costs half an hour.
- **Equipment damage:** machines break at random (more often after dust and radiation storms). A
  broken machine stops working until the player goes inside and repairs it. Radiation storms
  mostly damage electronics, as real solar particle events do.
- **Telescope (Science Lab):** a separate 3D scene you can drag and zoom, with a full night-sky sphere, a solar-system map with the planets' positions computed
  for the mission's real date, Earth and Moon, Jupiter, Saturn, Mars' moons, and a relayed view
  of the International Space Station (labelled as relayed: the ISS cannot be seen from Mars).
- **Real signal delay:** the Earth–Mars radio delay now comes from the two planets' positions on
  the mission date (circular-orbit approximation), about 17–20 light-minutes for the built-in
  missions, instead of a random number.
- **Sky and camera:** about 7,500 stars with a Milky Way band, Phobos and Earth as an evening
  star; drag up to tilt the camera up at the sky. Indoors the camera is top-down. Rocks, the
  rover and other large objects are solid. All icons are drawn shapes (no emoji). Right-click drag turns the camera (the browser menu is
  suppressed).
- **Rule changes:** solar output raised 20%; starting air and food scale with mission length;
  star ratings need 18 and 32 science per sol.

World detail: every model and texture is built in code (no image, model or audio files).
Buildings have panel seams, ribs, portholes, doors, tanks, pipes and cabling; the solar panels
track the Sun; the antenna is a lattice tower with a parabolic dish. The crew vehicle is an
Orion/Apollo-style conical capsule (heat shield, tiled 32° cone, windows, side hatch, thruster
pods, docking ring) on a landing-and-ascent stage. Choosing "Shelter inside" takes the player
into a walk-around habitat interior with the air machine, a mission computer showing the real
Sun and Mars data so far, bunks for resting (time passes), a galley, lockers and a treadmill.

Sound is synthesised with the Web Audio API: Mars wind that rises in dust storms, habitat hum,
a soft music pad, footsteps, airlock hiss, Geiger-counter clicks that speed up with radiation
exposure, a storm siren, rover motor, flare boom and rocket rumble. It can be switched off in
the pause menu.

Models and presentation: the astronaut is modelled on NASA's EMU/xEMU suits (gold sun visor,
helmet lights and camera, chest control module, PLSS life-support backpack, red crew stripes).
The helper rover follows Perseverance/Curiosity (six wheels on a rocker-bogie suspension, camera
mast, robotic arm with drill turret, RTG, hexagonal high-gain antenna). Skippable cutscenes play
for the mission intro, the first flare alert, each radiation storm, dust storms, and the crew
rocket launch at the end.

## The trade-offs (what the game teaches)

One scarce thing — **power** — has to be split between everything else. No choice is free.

| Building | Costs | Gives | What you give up |
|---|---|---|---|
| Air machine | 12 per level | air (crew uses 22/sol) | Power for anything else, especially at night |
| Greenhouse | 10 per level | food (crew eats 12/sol) + a little air | Science and shielding |
| Science lab | 14 per level | science score | Keeps nobody alive |
| Antenna | 6 per level | Daily Sun report, earlier storm warnings, Mission Control | Turn it off and you're blind |
| Shield robot | 16 per level | regolith shield on the habitat | The hungriest machine on base |
| Solar panels | 1 hour outside to clean | More power | Extra radiation and suit air while outside |

| Rover Rusty | 12 while working | Drills samples (+6 science) or cleans the panels, with no radiation risk to the crew | Slow, one job at a time, stops in storms and blackouts |

Other mechanics that mirror real constraints:

- **Day/night:** solar power only by day; the battery must carry the base through the night.
  Spending power at noon can cost oxygen at midnight.
- **Radiation storms:** being outside is dangerous, the habitat halves the dose, and the regolith
  shield cuts it further — but sheltering means no samples and no access to other buildings.
  Alerts can be false alarms, so sheltering every time has a real cost.
- **Dust:** panels slowly get dirty; random dust storms cut sunlight to 35%.
- **Communication delay:** Mission Control's advice arrives 8–44 "minutes" after you ask and is
  based on what Earth knew when you sent the message, so it can be out of date.
- **Fail softly:** a failed mission ends with the crew evacuating safely, a specific tip, and a
  table of the real space weather they faced. Random start dates, dust storms and sample locations
  make each run different.

## NASA data used

| Data | Source | How it is used |
|---|---|---|
| Solar flares (count, strongest class, peak time per day) | NASA **DONKI** `FLR` — https://ccmc.gsfc.nasa.gov/DONKI/WS/get/FLR | Each sol is one real calendar day. X-class day → big radiation storm; M5+ → small storm. Peak time sets when it hits. |
| Solar Energetic Particle events | NASA **DONKI** `SEP` — https://api.nasa.gov/DONKI/SEP | A day with an X-class flare or SEP event → a **flare alert** from Earth. |
| Radiation measured on Mars | NASA OSDR **RadLab**, instrument `MSL-RAD-Surface` (Curiosity rover's RAD detector) — `https://visualization.osdr.nasa.gov/radlab/api/?instrument_id=MSL-RAD-Surface&timestamp>=…&timestamp<…&absorbed_dose_rate` | Daily mean and peak absorbed dose rate (µGy/h). Decides whether an alerted storm **really hit Mars**, how strong it was, and the everyday background dose. Shown live in the HUD "Sun Watch" panel. |
| Live last-30-days flares + SEP | DONKI, fetched in the browser at page load | Unlocks the "The Sun Right Now" mission, labelled LIVE. |

### How DONKI and RadLab work together

DONKI says what Earth saw on the Sun; RadLab says what actually arrived on Mars. The game
plays both, so the player has to decide under real uncertainty:

- A flare alert arrives. Shelter (and lose science time), or gamble that it misses?
- A storm only strikes on days when Curiosity really measured a jump above 1.4× that month's
  normal dose rate. Every other alert is a **false alarm**, and the game shows Curiosity's
  ordinary reading for that date.
- **September 2017:** five alerts miss; then the X8.2 flare of 10 Sept reaches Mars a day later
  (Curiosity: 17.87 µGy/h mean on 11 Sept vs a normal 7.7).
- **May 2024:** the flares that gave Earth its great aurora on 10–14 May leave Curiosity's
  readings flat. Then on 20 May, with no big flare in Earth's list, Curiosity records a peak of
  303.99 µGy/h. In the game that storm arrives with only a 3-second base-sensor alarm.
- **December 2019:** no flares at all, yet the background is the highest of the three months
  (9.24 µGy/h vs 4.73 in May 2024), because a quiet Sun lets in more galactic cosmic rays.

**Archived vs live.** Four months of DONKI data are embedded in `index.html` as a per-day
summary, retrieved 14 Nov 2026: May 2024 (181 flares), Oct 2024 (103), Sept 2017 (41), Dec 2019
(0). RadLab daily values (from about 80 readings per day) are embedded for Dec 2019, Sept 2017
and May 2024. These are labelled "NASA archive" in the game. Only the "LIVE" mission's flare list
is downloaded at play time.

**Where there is no Mars measurement.** RadLab returned no MSL-RAD rows for October 2024, and its
server sends no CORS header, so a web page cannot query it live. For "Stormy October" and the
LIVE mission the game therefore assumes every big Earth-facing storm reaches Mars, and the HUD
shows "no data" for Mars dose.

Real figures referenced in the in-game facts and rules:

- Mars surface radiation ≈ 0.67 mSv/day — Curiosity RAD (Hassler et al., *Science*, 2014)
- Mars receives ≈ 43% of Earth's sunlight (~590 vs 1361 W/m²)
- InSight ended by panel dust (2022); Opportunity by the 2018 global dust storm
- MOXIE (Perseverance) produced 122 g of oxygen from Mars' CO₂ atmosphere
- One-way Mars–Earth signal delay ≈ 4–24 minutes; a sol is 24 h 39 min 35 s

## Honest limits

- Power, air, food and the radiation meter are **game units**, not kW or mSv. Storm doses are
  exaggerated so their effect is visible within 10 sols.
- Time is compressed: one sol = 48 seconds; one light-minute of delay = 0.5 seconds.
- The µGy/h figures shown are Curiosity's real ones, but each storm is only placed on the
  calendar day it was measured; its time of day in the game is approximate.
- Curiosity is in Gale Crater, not Jezero; its readings stand in for the base's own sensor.
- Dust storms and wind "cleaning events" are random, not from a dataset.
- The OSDR biology repository and Mars Trek are linked for further reading but not used as inputs.

## Ideas for next steps

- Show a real OSDR study card (bone loss, plant growth) when the lab finishes research.
- Real Jezero terrain from Mars Trek elevation data.
- On-screen joystick for tablets; a classroom mode with a shared leaderboard.

NASA does not endorse this project.
