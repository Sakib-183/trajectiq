# TRAJECTIQ

**Design the mission. Face the consequences. Master the trade-offs.**

Team: **Kemon Dilam Bondhura** · Challenge: **Space Mission Design Game**

A complete playable preparation prototype based on `TRAJECTIQ_NASA_Space_Apps_Winning_Playbook_v1(1).pdf`. The provisional mission is **Lunar Polar Mapper**. Check the final challenge resources and competition rules before submitting or representing pre-event work as hackathon work.

## Start locally

No dependencies, package installation, API key, hardware or paid AI is required.

**Easiest: double-click `START_GAME.bat` on Windows, or open `dist/TRAJECTIQ_Play.html` in Chrome, Edge or Firefox.** This file contains the entire playable application, including its engine, component catalog, styling and illustrations. It needs no server, Python, Node.js, API key or internet connection. Browser run history is local to the file/browser. Downloaded HTML may need to be opened in a browser rather than a document preview.

Developers can use the modular source through a local HTTP server:

```bash
cd TRAJECTIQ
python3 -m http.server 3000 --directory dist
```

On Windows, use `py -m http.server 3000 --directory dist` if `python3` is unavailable. Open **http://localhost:3000**. The modular `index.html` needs HTTP; the self-contained `TRAJECTIQ_Play.html` opens directly from disk. A Node/Python development server is not needed on a static host.

Run tests with **Node.js 22+**:

```bash
npm test
```

After source changes, regenerate the standalone output with `npm run standalone`. Both editions use the same engine and UI code; the standalone generator only inlines the modules and assets and disables service-worker registration.

## Play

### Version 4 mission copilot

Click **Ask assistant** to open the free offline copilot. Its answers use your current draft or the mission shown on the flight/result screen; a context card identifies which. Opening it pauses playback.

- **Mission status** explains the next click based on current battery, recorder, phase and crisis.
- **Review my design** simulates a bounded set of single-variable alternatives. It prioritizes removing launch blockers, then meeting the objective, then lower cost among successful designs (otherwise higher returned science). It does not search every combination.
- **Try a change** offers a component/distance picker and numerical settings. Or type `What if fuel = 45`, `What if battery size = 1.5`, or `What if duty = 75%`.
- Read the side-by-side forecast, then choose **Apply to draft** if desired. Previews never modify the real mission. Stale proposals are rejected and hardware cannot be changed during a flight.
- During a crisis, **Preview consequences with copilot** simulates each enabled decision from the exact paused state. Return to the game to make the actual decision.
- **Compare presets** uses a common policy for a fair comparison. `why?` explains the preceding supported topic; this is limited conversational context, not general-purpose dialogue memory.

Design forecasts start fresh at launch, using sunlit mapping and a disclosed conservative recovery policy. Crisis forecasts continue the current command/state, then use the same later recovery policy. The comparison tables show returned science, final battery, discarded data, cost and mass. Forecasts are game-model outcomes, not real-world probabilities. NASA/JPL environmental values continue to feed the underlying calculations.

The assistant is a deterministic local reasoning tool with simulation access, **not a live generative AI service**. English and Bangla answers, common Banglish phrases, source links and an honest unknown-answer fallback are included. No API fees or keys. See `docs/research/SUBMISSION_REVIEW_V4.md` for the published-project review motivating these improvements.

1. The game opens directly on the mission offer. Click **Accept mission** to enter the interactive hangar.
2. Rotate the 3D spacecraft by dragging, zoom with the mouse wheel, or use arrow keys and +/−. Click a visible subsystem or its named slot. Choose its component class, or drag a module onto the spacecraft. The visual architecture and engine margins update together.
3. Build bus, payload, power, propulsion, communications, storage and launch class. Selecting the launch category displays the carrier schematic. Adjust fuel, altitude and inclination in the right rail.
4. Open **Launch readiness**. Mass, budget and recorder-interface failures block launch. Other warnings are trade-offs you may accept.
5. Authorize launch. The scene changes from launch vehicle to Earth–Moon transfer and then the lunar orbit determined by your chosen altitude/inclination and derived period. Launch/transfer visuals are schematic and compressed, not trajectory predictions.
6. Command the spacecraft: **Map in sunlight**, **Map through shadow**, **Conserve energy** or **Return priority**. These commands change actual science generation, loads and downlink scheduling in the engine. Pause, step, change speed, or advance to the next event.
7. Crises pause the game. Choose an available recovery action, inspect the consequence, then press **Resume**. The assistant also pauses the clock while you read.
8. Inspect the mission result and causal chains. Redesign, play again and compare the actual returned science/cost/data/energy evidence. Detailed telemetry, uncertainty, trade-space and sources remain available in the engineering lab.
9. Export a JSON mission report with telemetry, design, causal facts and source references.

## Deterministic pitch demo

1. Click **Pitch mode**. It loads the fragile high-science design.
2. Review the warnings, authorize launch, and use **Fast-forward**.
3. In the recorder crisis select **Keep mapping**. In the battery crisis select **Keep collecting**. For a degraded link choose **Wait for next contact**. Use Fast-forward between decisions.
4. Open the causal debrief: the design loses data and runs short of energy.
5. Click **Apply power + comms fix** or **Redesign fix**. The same high-science payload now has endurance power and high-gain communications.
6. Review and relaunch, using the same operating mode and crisis responses for a controlled comparison.
7. Compare science return, lost bytes, safe-mode time and cost. The corrected design meets the modeled objective.

The outcome comes from the engine, not a scripted success/failure flag. Engine tests repeat the full failure/fix loop five times. This is not a claim that a human presentation has been timed or rehearsed.

## Engineering model

- **Mass:** selected hardware + 15 kg educational structure allowance + propellant. Wet mass is checked against injected-mass capacity.
- **Power:** battery energy integrated over intervals of at most one minute, split at contact and shadow boundaries, in Wh. Solar generation stops in eclipse. Loads reflect science, transmission, recovery and safe mode. The player advances by up to five minutes per action, stopping at crises. Empty energy terminates the mission. Safety thresholds are evaluated after each subinterval.
- **Propulsion:** Tsiolkovsky equation with standard gravity, educational Isp and mass values. Maneuver fuel is consumed with the inverse equation.
- **Orbit:** circular two-body period derived from sourced lunar GM and radius. Inclination/altitude science penalties are educational approximations. Eclipse and contact windows are assumed, not ground-geometry predictions.
- **Data:** MB generated and transmitted using Mbps × seconds / 8. A finite recorder discards new data when full. Only quality-weighted downlinked data contributes to science completion.
- **Budget:** fictional cost credits, not market prices. Operations overhead is included.
- **Robustness:** 200 fixed-seed, independent uniform variations in solar, link, Isp and pointing performance. The same conservative recovery policy runs every sample. Reported as Educational Robustness Index, never real-world probability.
- **Debrief:** rule-based causal traces and recommendations. No LLM controls science, readiness or outcomes.
- **Pareto comparison:** non-dominated cost/science points among completed saved runs. Bubble size shows safe-mode time, not probabilistic risk.

**Success:** ≥80% of the 650-unit returned-science target and final battery above zero. Scenario requirements and all fictional hardware values are explicitly marked as educational assumptions.

## NASA / JPL use

Actual sourced constants: Moon mean radius **1,737.4 km**, gravitational parameter **4,902.800 km³/s²** from [JPL satellite physical parameters](https://ssd.jpl.nasa.gov/sats/phys_par/sep.html). These feed the displayed orbital period.

The propulsion equation comes from [NASA Glenn's ideal rocket equation](https://www1.grc.nasa.gov/beginners-guide-to-aeronautics/ideal-rocket-equation/). Subsystem and trade-study concepts refer to [NASA Small Spacecraft Technology](https://www.nasa.gov/smallsat-institute/sst-soa/) and the [NASA Systems Engineering Handbook appendix](https://www.nasa.gov/reference/system-engineering-handbook-appendix/).

Hardware masses, power values, Isp, capacities, costs, science quality scores and operational scenarios are **assumptions**, not extracted vendor or NASA spacecraft specifications. No live Horizons query or satellite imagery is used. Exact source links and classifications are visible inside the app.

## Version 3 additions

- Beginner coach, a visible orbit/contact/shadow timeline, readable controls and slower default playback.
- **Customize**: mission name, three spacecraft colors, array and battery size (75–150%), instrument duty (25–100%), plus NASA reference distance scenario. Seven subsystem categories now contain 17 component choices. Size changes carry mass/cost consequences.
- **Ask mission assistant**: English/Bangla local knowledge, live design/run values, 45 help topics, NASA links and an explicit unsupported-question fallback. This is a deterministic project assistant, not an unrestricted generative AI chatbot. It does not make network calls or require payment.
- Smooth orbital position transitions, reusable GPU buffers, transmission pulses, reduced-motion preference and accessible focus indicators.
- Safe defaults for malformed browser saves and numeric inputs; version-2 history is retained with a model-version comparison warning. Completed runs are saved; an unfinished flight is not persisted across refresh.

The NASA numeric snapshot in `dist/data/nasa.js` and `dist/data/nasa-moon-snapshot.csv` contains **8 sourced values**, verified 2026-10-06. The NASA fact sheet was last updated 2024-01-11. Source HTML snapshots are retained under `docs/research` for reproducibility.

Solar irradiance **1,361 W/m²** determines output through an assumed 28% cell efficiency and array area. Actual operation applies 85% derating. Power-kit nameplate wattage fixes the baseline area; panel size modifies it. Mean perigee **363,300 km**, reference semimajor axis **384,400 km**, and mean apogee **405,500 km** determine an educational inverse-square link-rate factor and the displayed light-travel delay. These are static reference scenarios, not current ephemerides. Displayed signal delay does not delay player commands. No LRO raster or thermal model is loaded.

## Architecture

```text
dist/data/nasa.js        Curated NASA/JPL numeric dataset and provenance
dist/engine/assistant.js Grounded project help and honest fallback
dist/data/catalog.js     Versioned components, mission, presets, sources, uncertainty
dist/engine/model.js     Pure deterministic equations, constraints, events and debrief
dist/ui/app.js           Screens, user actions, local run archive and export
dist/ui/scene.js         Original lightweight SVG orbit illustration
dist/ui/space3d.js       Native WebGL spacecraft/hangar/launch/orbit renderer
dist/styles.css         Responsive design and reduced-motion support
dist/sw.js              Static offline cache
tests/                  Engine acceptance and application DOM-adapter tests
docs/                   Requirements, model, demo and submission notes
```

The PDF recommends Next.js/TypeScript/Three.js, but does not mandate them. This version uses dependency-free browser ES modules, keeping physics isolated from the UI. The game scene is an interactive **native WebGL 3D schematic**, with an SVG fallback when WebGL is unavailable. Hardware shapes are representative, not CAD models. The illustrative orbit is not to scale and does not constitute a full trajectory solver. No accounts, backend, multiplayer, tracking or third-party runtime calls.

## Offline and privacy

All model data and modules load locally. The mission works without internet once loaded. A service worker caches the static assets for revisits on HTTPS or localhost; wait until its first installation completes before closing the initial session. External source links need internet to open, but explanations remain in the app. Completed-run history is stored in the current browser only, limited to 20 runs. Unfinished flights are not saved on refresh. JSON export provides a portable copy.

## Validation

Version 3: **42 automated tests pass**, including a seeded sweep of 100 candidate spacecraft configurations. See `docs/QA_V3.md` for scope and limitations.

`npm test` covers mass and energy trends, hard launch limits, incompatibility, data conservation, propulsion consumption, crisis gating, recovery, seeded robustness, Pareto comparison, orbit constraints and repeated failure/redesign. Application DOM-adapter tests exercise the complete interactive game path including module drop, operations commands, recovery and redesign, plus the engineering screens, history, mentor and quiz. 3D tests cover camera/orbit math, missing-WebGL fallback and mocked WebGL drawing calls for each mission phase.

These are automated model and application checks, not a real-browser visual audit or a user study. Responsive CSS and keyboard navigation are implemented, but browser/device QA, user observation and pitch timing must still be performed on the team's actual demo machine.

## Limits and next steps

This is an educational first-order game, **not flight design certification or NASA approval**. There is no full launch trajectory, n-body propagation, thermal/radiation model, detailed RF budget or measured hardware reliability. The alternate-capture penalty is a teaching approximation. Robustness samples do not model probabilistic hardware failures. The quiz is a self-check, not evidence of validated learning impact.

Audit the final brief, run classroom feedback sessions, perform browser/device and actual GPU-rendering QA, rehearse the pitch, record the demo and add the six real team members' contribution statements before submission.

## Credits

Concept and team: Kemon Dilam Bondhura. Implemented with Codex assistance. Reference material: NASA and JPL as listed. Visuals and icons are original code-generated SVG/CSS; no external art, font, game engine, paid AI or external asset license is required. This project is not affiliated with or endorsed by NASA.
