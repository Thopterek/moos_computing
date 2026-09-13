# INFORMATION NOT CHECKED

There is more than just a grain of salt to be taken around for it

Bunch of those information has to go through verification

## Function-Based Equipment Specification for a Hackerspace Moss Electrophysiology & "Moss Computing" Laboratory

## TL;DR

- The single non-negotiable capability is **DC-coupled, low-drift, multi-channel biopotential acquisition**: you must resolve sub-millivolt features (0.25–0.4 mV) riding on ~30 mV of slow drift, continuously for 7+ days, with a channel that passes true DC. A 24-bit DC data logger of the Pico ADC-24 class (noise-free resolution 19 bits on the ±156 mV range, offset accuracy 9 µV) satisfies this; AC-coupled EEG/neural front-ends (Intan RHD2000, OpenBCI Cyton) do NOT, because their 0.1–1 Hz high-pass erases the multi-hour depolarization waves that are the entire published finding.
- The second-order capabilities that turn a recording rig into a research lab are **rigorous negative controls with viability/death and sterility verification**, **calibration/phantom instrumentation to prove your own noise floor**, and **environmental measurement in the correct units** (PAR/PPFD in µmol m⁻² s⁻¹, not lux; gravimetric water content, not "wet"). These directly attack the field's n=1, no-controls, unmonitored-environment problem.
- Prioritize **replication over bit-depth**: build the cheapest credible DC channel and multiply it across four simultaneous preparations (living / freshly dead / autoclaved / inert substrate). Everything else — impedance spectroscopy, four-terminal dielectric work, isolated charge-balanced stimulation, closed-loop actuation, reservoir-computing benchmarking, microbial-fuel-cell integration — is a staged add-on that should not be attempted until the acquisition + control + calibration triad is verified against a dummy load.

## Key Findings

**The anchor measurement chain.** Adamatzky, "Multi-scale electrical activity and signal propagation in the moss *Brachythecium rutabulum*," *R. Soc. Open Sci.* 1 July 2026; 13(7):252341 (DOI 10.1098/rsos.252341; sole author, University of the West of England), recorded moss collected from North Somerset, SW England, using "pairs of iridium-coated stainless steel sub-dermal needle electrodes (Spes Medica S.r.l., Italy), connected to twisted cables and an ADC-24 high-resolution data logger from Pico Technology (UK) equipped with a 24-bit analog-to-digital converter." Sampling was 1 sample/s (the logger "capturing as many measurements as possible (typically up to 600 per second) and saving the average value"), ±156 mV range, eight differential pairs, 1–2 cm spacing, 178 h continuous, on wet substrate in a closed high-humidity container at ~5 lux, producing >5 million measurements. Crucially, the paper itself states: "No dedicated control recordings (e.g. inert substrates or non-viable moss tissue) were performed in the present study, and environmental variables, such as humidity and temperature, were not continuously monitored." Those two sentences define the exact gaps your lab must close.

**Signal targets that set the specs.** Fast spikes ~0.4 mV; medium oscillations ~0.25 mV over ~1 h; slow waves over ~4.2 h; and large events with a 6.2 h depolarization of ~30 mV, 1.3 h repolarization, 2.8 h refractory, recurring ~every 18 h. The combination — 0.25 mV features on a 30 mV excursion evolving over hours — is what forces DC coupling and high dynamic range simultaneously.

**Why AC coupling fails, with the arithmetic.** A single-pole high-pass with corner f_c has time constant τ = 1/(2πf_c). Intan RHD2000 chips have a programmable lower cutoff adjustable from 0.1 Hz (on-chip registers) down to 0.02 Hz only with off-chip resistors; the OpenBCI Cyton and typical EEG/EMG front-ends sit at 0.1–1 Hz. At f_c = 0.1 Hz, τ ≈ 1.6 s. A 6.2-hour (22,320 s) depolarization is ~14,000 time constants long, so the high-pass has fully settled to zero and the entire wave is removed; only the fast leading edge survives, hugely distorted. Even at the most aggressive off-chip 0.02 Hz cutoff, τ ≈ 8 s — still four orders of magnitude shorter than the event. **Functional requirement: the recording path must pass true DC (0 Hz), or at worst have a lower cutoff below ~1 µHz, which in practice means DC-coupled instrumentation only.**

**The ADC-24 class quantified (primary source, Pico datasheet MM076 / User's Guide adc20.en).** Nominal 24 bits but *noise-free* resolution is 19 bits on the ±156 mV range at 660 ms conversion (17 bits at 60 ms; 20 bits max on wider ranges). On the ±156 mV range the noise-free step is ≈ 312.5 mV/2¹⁹ ≈ 0.6 µV — roughly 400× headroom on a 0.25 mV feature. Published offset accuracy is 9 µV on ±156 mV (6 µV on ±39 mV); Adamatzky's methods corroborate this: "the acquisition voltage range was set to 156 mV with an offset accuracy of 9 μV at 1Hz to maintain a gain error of 0.1%." ADC input bias current is 50 nA max and input impedance 2 MΩ differential — both problematic against 1–2 MΩ moss (see Blocks 1/1b). Pico does not publish an offset temperature-drift figure; quoted accuracy holds only 20–30 °C. These are peak-to-peak noise-free bits, not RMS ENOB.

## Details — Functional Capability Blocks

Each block: (a) capability, (b) why / which gaps, (c) quantitative spec with reasoning, (d) instrument classes DIY→commercial with EUR bands, (e) acceptable substitutions and what is lost, (f) verification.

---

### Block 1 — Low-drift DC biopotential acquisition (multi-channel, multi-day)

**(a)** Continuously digitize 8–32 differential channels of DC-coupled biopotential for ≥7 days without gaps.

**(b)** This is the core capability; it serves all eight gaps because every experiment reads moss potentials. Gaps 1–4 (hydration mapping, controls, stimulus-response, spatial mapping) are impossible without it.

**(c) Specifications and reasoning:**

- **Coupling: DC (0 Hz).** Reasoning above — a 6.2 h wave demands it.
- **Dynamic range / effective resolution.** You must resolve a 0.25 mV feature while a 30 mV excursion is present. On a fixed ±156 mV range, 30 mV + 0.25 mV both fit. To see 0.25 mV cleanly you want the noise floor and quantization step ≲25 µV (10:1 margin on the smallest feature). On the ±156 mV range the ADC-24 gives 19 noise-free bits at 660 ms (≈0.6 µV noise-free step) and 9 µV offset accuracy. **Functional threshold: ≥16 noise-free bits over a ≥±100 mV span, i.e. noise floor ≤ ~10 µV RMS and quantization step ≤ ~25 µV.** This corresponds to a genuine ≥20-bit converter, since nominal 24-bit parts deliver only ~19–20 noise-free bits.
- **Input impedance ≥ 100× source.** Moss electrode pairs are 1–2 MΩ; the ADC-24's 2 MΩ differential input is actually marginal (it forms a divider with a 2 MΩ source, attenuating ~50%). **Functional requirement: differential input impedance ≥ 1 GΩ, achieved by per-channel high-impedance buffering (Block 1b) ahead of any logger whose input is ≤ 10 MΩ.**
- **Input bias current.** Through a 2 MΩ source, the ADC-24's 50 nA max ADC bias current develops 50 nA × 2 MΩ = 100 mV of error — catastrophic. This is the strongest single argument for a buffer stage: **bias current must be ≤ ~10 pA so that I_bias × 2 MΩ ≤ ~20 µV.**
- **Sample rate.** 1 S/s captured the published phenomena; 10 S/s (as in the fungal biohybrid work, below) gives margin for the ~0.4 mV fast spikes. **Functional threshold: ≥1 S/s per channel sustained, ≥10 S/s preferred; anti-alias with the logger's 50/60 Hz rejection or an analog RC.**
- **Endurance: gapless ≥178 h.** Requires stable software, storage, and power (Block 17).

**(d) Instrument classes:**

- *Commercial DC data logger (recommended baseline):* Pico ADC-24 class — 8 differential/16 single-ended, 24-bit, galvanically isolated USB, ~€1,000–1,300. Equivalent classes: any 24-bit multichannel USB DAQ with true differential DC inputs and per-channel programmable range (e.g. LabJack T7-PRO with front-end caveats, National Instruments/MCC 24-bit DAQ). Look for "true differential," "DC," "galvanic isolation."
- *DIY:* a delta-sigma ADC board (ADS1256 8-ch 24-bit, or ADS131M0x) on a Raspberry Pi/microcontroller, €30–120, preceded by an instrumentation-amplifier/electrometer buffer per channel (Block 1b). This is the channel-count-per-euro winner and the path to 4× replication.
- *Precision multimeter/SMU* (Keithley DMM6500, Keysight) as a single-channel gold reference for cross-checks, €1,500+.

**(e) Substitutions and what is lost:** Using an AC-coupled biopotential system (Intan/OpenBCI) loses the entire slow-wave finding — unacceptable as the primary instrument, though such systems remain useful for the fast-spike band only if you explicitly accept losing everything below their cutoff. Using the ADC-24 without buffers into 1–2 MΩ moss loses ~50% of signal amplitude and injects bias-current error; you lose quantitative amplitude accuracy.

**(f) Verification:** Before any moss, record the phantom (Block 3) and a shorted input. Confirm noise floor ≤ spec in µV RMS over 24 h; confirm a known injected 250 µV step from the calibrator reads correctly on all channels; confirm no channel drifts more than a few tens of µV/h on a stable resistor network.

---

### Block 1b — High-impedance per-channel buffering (electrometer front end)

**(a)** Unity-gain (or modest-gain) buffer between each 1–2 MΩ electrode pair and the logger.

**(b)** Enables Blocks 1, 4, 11 (spatial multiplexing). Direct analog multiplexing into megohm sources is unusable: MUX charge injection and finite settling time into 2 MΩ produce channel crosstalk and errors, so each channel needs its own buffer before any mux.

**(c) Specs:** Input bias current ≤ 10 pA (target ≤ 1 pA); input impedance ≥ 1 GΩ (ideally ≥ 1 TΩ); offset voltage ≤ 100 µV and offset drift ≤ 1–5 µV/°C; settling to 0.01% into the logger within one sample period. Reasoning: at 2 MΩ source, 1 pA bias → 2 µV error. The pH-probe analogy is instructive: a 3 nA-bias op-amp on a 10 MΩ source produces 30 mV of error, versus 30 nV with a femtoamp part. For four-terminal impedance work (Block 12) the same buffers need ~pA bias and guard rings — a documented four-contact impedance rig used a ~TΩ-input AD8642 (<1 pA bias) with guard rings positioned at the voltage electrodes.

**(d) Classes:** Electrometer op-amps — Analog Devices ADA4530-1 (femtoamp bias, integrated guard buffer), TI LMP7721 (3 fA), TI OPA928, or JFET-input AD8642 (≤1 pA) for lower cost. DIY per-channel board ~€5–25/channel; guard rings and PTFE standoffs essential. Commercial electrometer/source-measure units cost far more per channel.

**(e) Substitutions:** A cheaper CMOS op-amp (nA bias) loses DC accuracy into megohm sources. Skipping guard rings loses low-fA performance to PCB leakage (especially at high humidity — your chamber is deliberately humid). Conformal-coat or pot the front end.

**(f) Verification:** With input open (guarded), output should sit at offset; inject via a 1 GΩ resistor from a known voltage and confirm the divider ratio matches the buffer's true Z_in. Measure drift over a 24 h dummy-load run.

---

### Block 2 — Electrode interface and reference system

**(a)** Non-polarizable recording electrodes + stable reference, fabricated and maintained in-house.

**(b)** Serves all recording gaps, especially gap 3 (stimulus-response, where baseline stability matters) and gap 1 (multi-day hydration).

**(c) Specs and reasoning:**

- **Type: Ag/AgCl strongly preferred over stainless/Ir/Pt for DC.** Polarizable metals (the stainless/iridium needles in the anchor study) develop drifting half-cell potentials and pass no steady DC without polarizing; Ag/AgCl is "essentially non-polarizable." For a 30 mV signal over 6.2 h you can tolerate a drift budget of maybe ~0.5–1 mV over the event (≤~3% of amplitude), i.e. **electrode-pair differential drift ≤ ~100–150 µV/h.** Well-made Ag/AgCl references drift <1 mV/day once equilibrated; ultra-low-noise Ag/AgCl pairs need "at least 48 hours" to reach a stable low offset.
- **Half-cell matching:** use matched pairs from the same chloriding batch; expect and log an initial equilibration transient (first ~48 h).
- **Contact:** for a cushion moss, fine Ag/AgCl wire or pellet in agar/KCl bridge contact avoids wounding; needle geometry (as in anchor) is acceptable but wounds tissue (a confound for gap 3 wounding experiments).

**(d) Classes:** DIY chlorided silver wire (silver wire €5–20 + FeCl₃ or electrolytic chloriding), Ag/AgCl pellet electrodes (€5–15 each), commercial sintered Ag/AgCl (Warner, WPI, €30–120). Agar-KCl salt bridges for non-invasive contact. Keep the anchor-style iridium/stainless subdermal needles (Spes Medica class, €2–5/needle) as a documented comparison arm.

**(e) Substitutions:** Stainless/Pt needles are cheaper and rugged but you lose DC baseline stability and add polarization; acceptable only if you run them against Ag/AgCl and the phantom to bound the artifact. Bare silver (un-chlorided) loses non-polarizability.

**(f) Verification:** Immerse an electrode pair in saline, short via the solution, and record the offset and drift for 24–48 h; a good pair settles to <0.1 mV offset drifting <100 µV/h. Monitor per-electrode impedance (Block 12) before/after runs.

---

### Block 3 — Calibration and validation instrumentation (mandatory, was missing before)

**(a)** A traceable microvolt-level source and a passive RC "phantom" moss model.

**(b)** Serves every gap indirectly by proving the rig — you cannot claim 0.25 mV features are biological unless you have shown your rig's noise floor and drift on a non-biological load of the same impedance. This is the antidote to the anchor study's missing controls.

**(c) Specs:**

- **Microvolt calibrator:** inject known steps from ~10 µV to ~50 mV with ≤1% accuracy, plus slow ramps mimicking a 30 mV/6 h wave. To validate resolution of 0.25 mV you need a source an order of magnitude finer (~25 µV steps). A precision voltage reference + resistive divider (e.g. 10,000:1) with a calibrated DMM readback achieves this cheaply.
- **Phantom/dummy moss:** an RC network matching moss scale — series/parallel resistances ~1–2 MΩ with ~nF shunt capacitance per "electrode," arranged tetrapolar for impedance validation. Characterizes the true noise floor/drift into a megohm source and lets you inject synthetic "spikes" and "waves" to validate detection software (Block 18).

**(d) Classes:** DIY precision divider (Vishay 0.01% resistors, LM399/ADR1000 reference) €20–60; commercial calibrator (Time Electronics, Fluke 5xxx) €1,000s. Phantom: metal-film R + C0G/film C on a guarded board, €10–30.

**(e) Substitutions:** A 1.5 V cell through a big divider works for a rough step but loses traceability; you lose the ability to state absolute accuracy. No phantom = no defensible noise-floor claim.

**(f) Verification:** The calibrator is self-verifying against a calibrated DMM. The phantom run IS the verification procedure for Blocks 1/1b.

---

### Block 4 — Signal integrity infrastructure (shielding, grounding, isolation, vibration)

**(a)** Faraday shielding, single-point grounding, galvanic isolation, and vibration damping.

**(b)** Serves all recording gaps; essential at µV levels and megohm impedances.

**(c) Specs:**

- **Faraday cage:** copper mesh around specimen + front end. The fungal biohybrid work (Mishra et al. 2024, below) used a 1×1×1.5 m copper-mesh cage; scale to your rig. Mesh holes ≪ wavelength of interest; bond to a single ground.
- **Galvanic isolation:** the recording front end must be isolated from mains-referenced computer/actuators. This prevents ground loops (which inject 50 Hz and DC offsets) and is a safety barrier so no mains fault can reach the specimen/experimenter. The ADC-24 provides USB galvanic isolation to the PC internally; a DIY ADS1256 rig needs an isolated USB/SPI barrier (digital isolator + isolated DC-DC) and/or battery power for the front end. For closed-loop actuation, isolate the actuator drive (Block 14).
- **Vibration damping:** sand-filled base or sorbothane; the fungal biohybrid rig used sand-filled vibration damping. Microphonic and triboelectric artifacts on high-Z cables mimic slow drifts.
- **Cabling:** twisted differential pairs (as in anchor), guarded/driven shields for the high-Z segment, minimize cable movement.

**(d) Classes:** DIY copper mesh/foil cage €30–150; commercial RF enclosure €300+. Isolated USB (ADuM-class module) €15–40. Sand box/sorbothane €10–50.

**(e) Substitutions:** Aluminium-foil box loses some low-frequency magnetic shielding but is fine for E-field. Skipping isolation risks 50 Hz pickup and is a safety hazard once mains actuators are added — do not skip once closed-loop.

**(f) Verification:** Record the phantom inside the closed cage; 50 Hz line component should drop by ≥20–40 dB versus open bench. Tap the bench and confirm no artifact appears on high-Z channels.

---

### Block 5 — Environmental measurement (T, RH, light)

**(a)** Continuous logging of temperature, relative humidity, and photobiologically-correct light.

**(b)** Directly closes the anchor study's "environment not monitored" gap; essential for gap 1 (hydration cycle) and gap 3 (light/thermal stimuli). Temperature co-varies with electrode drift, load-cell drift, and biology, so it must be logged to deconvolve artifacts from signal.

**(c) Specs:**

- **Temperature:** resolution ≤ 0.1 °C, accuracy ≤ ±0.3 °C, logged ≥1/min. Electrode and amplifier drift are temperature-dependent; you need to correlate.
- **RH:** ≤ ±2–3% RH accuracy, calibrated against saturated-salt points (Block 6). At the chamber's high humidity, near-condensation behavior matters.
- **Light in PAR/PPFD (µmol m⁻² s⁻¹), NOT lux.** Lux is weighted to human photopic vision and under-counts red/blue that drive photosynthesis; the lux→PPFD conversion is source-spectrum dependent and impossible to do accurately without the spectral power distribution. The anchor's "~5 lux" is photobiologically ambiguous — for a white LED (~54–67 lux per µmol m⁻² s⁻¹), ~5 lux is on the order of ~0.07–0.1 µmol m⁻² s⁻¹, i.e. extremely dim, essentially below the light-compensation point. Report light as PPFD and the spectrum.

**(d) Classes:** DIY: SHT35/BME280 (T/RH, €5–15), calibrated thermistor/PT100 (€5–20); light — a quantum/PAR sensor is the correct instrument (Apogee SQ-series, €200–350) or a spectrometer for spectrum; a lux meter or smartphone app is a rough proxy only. Commercial data-logging T/RH (€50–200).

**(e) Substitutions:** A lux meter is acceptable ONLY if you also record the LED spectrum and publish the conversion factor and its uncertainty; otherwise you lose comparability with photobiology literature. A consumer hygrometer loses accuracy near saturation.

**(f) Verification:** Calibrate RH at two saturated-salt points (MgCl₂ ≈ 33% RH, NaCl ≈ 75% RH at 20 °C); calibrate T against ice/boiling or a reference thermometer; PAR sensor against sun or a calibrated source.

---

### Block 6 — Environmental control (humidity chamber, wetting, airflow, thermal)

**(a)** Set and hold RH set-points, deliver controlled wetting/misting, control airflow/drying rate, stabilize temperature.

**(b)** Core to gap 1 (full hydration cycle — wetting AND drying) and gap 3 (thermal stimulus). Desiccation rate determines bryophyte survival, so drying must be controlled, not incidental.

**(c) Specs:**

- **RH set-points via saturated salt solutions** in a sealed chamber (Greenspan/ASTM E104 fixed points): LiCl ≈ 11%, MgCl₂ ≈ 33%, K₂CO₃ ≈ 43%, NaBr ≈ 59%, NaCl ≈ 75%, KCl ≈ 85%, K₂SO₄ ≈ 97% RH (values shift a few % with temperature; humidities >90% risk condensation). This gives cheap, stable fixed RH points to define hydration states. Note the desiccation-tolerance benchmark: a water content of ~0.1 g H₂O g⁻¹ dry mass corresponds to equilibrium with ~50% RH at 20 °C, the accepted threshold for a plant to count as desiccation-tolerant.
- **Controlled drying rate:** adjustable airflow (small fan + valve) to set slow vs rapid desiccation, since bryophyte survival depends on drying rate.
- **Wetting delivery:** metered misting/perfusion to rehydrate reproducibly.
- **Thermal stability:** hold ± ~0.5 °C to separate biological signals from thermal drift; a foam/insulated enclosure or a TEC-controlled box.

**(d) Classes:** DIY sealed food-grade containers + salt slurries (€10–30 total), aquarium fan + PWM, ultrasonic mister (€10–30), insulated box + heater/TEC + PID (€30–100). Commercial climate chamber (€1,000s).

**(e) Substitutions:** A simple closed humid container (as in anchor) is the minimum; you lose defined RH set-points and controlled drying — exactly gap 1's requirements. Salt-slurry chambers recover this cheaply.

**(f) Verification:** Log RH in each salt chamber to confirm it reaches the tabulated set-point; measure the drying curve (mass vs time, Block 7) at each airflow setting.

---

### Block 7 — Hydration-state measurement (gravimetric water content)

**(a)** Continuous or periodic measurement of moss water content.

**(b)** Gap 1 is fundamentally about mapping electrical activity vs hydration; this is the independent variable.

**(c) Specs:**

- **Relative water content / water content per dry mass.** Bryophyte convention: water content = (fresh mass − dry mass)/dry mass (g H₂O g⁻¹ DM); dry mass from oven-drying. Mosses are largely ectohydric (external water), so tissue vs surface water must be distinguished.
- **Continuous gravimetry:** a load cell under the specimen. **Caveat/spec:** strain-gauge load cells and their bridge amplifiers drift with temperature, which co-varies with your experimental variable — so require mass resolution ≤ ~10 mg on a ~10–100 g specimen, temperature-compensated or temperature-logged, with periodic re-zero against a precision balance. Thermal drift of the load cell can otherwise masquerade as water-content change.
- **Precision balance for dry mass:** ≤1 mg readability (0.001 g), for the RWC denominator.
- **Drying oven:** 60–105 °C to constant mass for dry-mass determination.

**(d) Classes:** DIY HX711 + load cell (€5–15, check drift), or periodic removal to a jeweler's/lab balance. Precision balance 0.001 g (€100–400). Lab oven or controlled toaster-oven + thermocouple (€30–200). Capacitive substrate moisture sensors (€3–10) as a continuous proxy for substrate wetness (not tissue RWC).

**(e) Substitutions:** Periodic weighing loses time resolution during fast drying; capacitive sensors measure substrate not tissue (lose direct RWC). HX711 without temperature compensation loses accuracy.

**(f) Verification:** Cross-check load-cell reading against the precision balance; establish a reproducible drying curve; verify the oven reaches constant mass.

---

### Block 8 — Viability and death verification (for negative controls)

**(a)** Independent confirmation that "dead"/"autoclaved" moss is actually dead, and by how much.

**(b)** Gap 2 (negative controls) is worthless if the "dead" control is partly alive or recolonized. The anchor study had NO dead controls; this block is a headline deliverable.

**(c) Specs and what each proves:**

- **Chlorophyll fluorescence Fv/Fm** — fast, non-destructive viability gold standard. Healthy PSII gives Fv/Fm ≈ 0.79–0.84 (unstressed leaves typically >0.79; moss values in this range when healthy); dead/severely stressed tissue → toward 0. Proves photosynthetic death. Standard practice is a ~10-min dark adaptation before measurement.
- **Electrolyte leakage by conductivity** — dead cells leak ions; measure the conductivity rise in the bathing water. Proves membrane-integrity loss. Cheap conductivity meter (€30–150).
- **Vital staining** (Evans blue for dead cells; tetrazolium/TTC for respiration) — microscope-based, proves cell-level death.
- **Contamination checks** (Block 9) confirm the "dead" tissue is not just a bacterial mat generating its own signals.
- **Sterility timeline:** an autoclaved/dead cushion on wet substrate will be microbially recolonized within ~48 h over a 178 h run, so viability/sterility must be re-verified during, not just before, recording.

**(d) Classes:** Fv/Fm: budget continuous-excitation fluorometer/DIY (€100–500) to commercial (Hansatech Handy PEA, Walz PAM; €2,000–15,000). Conductivity meter €30–150. Staining reagents + existing microscope (Block 16).

**(e) Substitutions:** If you can afford only one, choose Fv/Fm (fast, non-destructive, quantitative) plus conductivity (cheap, orthogonal). Skipping all viability tests means your controls prove nothing — the single worst economy in the build.

**(f) Verification:** Establish Fv/Fm and leakage baselines on healthy moss; kill a sample (autoclave/freeze/heat) and confirm Fv/Fm→~0 and a leakage spike; re-measure at 48 h.

---

### Block 9 — Sterility and contamination control

**(a)** Autoclave-equivalent sterilization, clean handling, and contamination monitoring.

**(b)** Gaps 2 and 8: distinguishing moss signals from microbial/anodophile signals. Bombelli et al. 2016 (*R. Soc. Open Sci.* 3(10):160249) found non-sterile *P. patens* bryoMFCs "delivered over an order of magnitude higher peak power output (2.6 ± 0.6 µW m⁻²) than bryoMFCs kept in near-sterile conditions (0.2 ± 0.1 µW m⁻²)," and environmental (non-sterile) moss samples "reached 6.7 ± 0.6 mW m⁻²" — microbes dominate electrical behavior and can generate their own signals.

**(c) Specs:**

- **Sterilization:** a pressure cooker reaches ~121 °C at ~1 bar gauge — the standard autoclave condition — a legitimate DIY autoclave alternative. Hold ≥20 min.
- **Clean handling:** a still-air box or DIY laminar-flow (HEPA fan) for transfers.
- **Contamination verification:** streak bathing water/tissue onto nutrient agar (LB, PDA) and incubate; count CFU over the run. Proves whether "sterile" stays sterile.
- **Metabolic inhibitors** (optional arm): antibiotics to suppress microbial contribution and isolate the plant signal; sodium azide is effective but toxic/restricted (see Block 19 procurement).

**(d) Classes:** Pressure cooker (€30–80); HEPA still-air box DIY (€30–100); agar plates + reagents (€20–60); autoclave tape/indicators (€10).

**(e) Substitutions:** Chemical sterilization (bleach/ethanol) for heat-sensitive substrate, losing some efficacy. Boiling (100 °C) instead of pressure cooking loses spore-killing — recolonization is faster.

**(f) Verification:** Autoclave-tape/spore indicator confirms the cycle; plate a known-contaminated and a sterilized sample and compare CFU.

---

### Block 10 — Stimulus delivery

**(a)** Deliver controlled light, thermal, mechanical, and chemical stimuli with timed triggers synchronized to the recording.

**(b)** Gap 3 (deliberate stimulus-response: light, cold/heat, wounding, glutamate, ROS/H₂O₂).

**(c) Specs:**

- **Light:** spectrally defined LED (known SPD), intensity controllable and measured in PPFD (Block 5), with ms-precise trigger. Light-induced action-potential-like depolarizations are documented in moss.
- **Thermal:** Peltier or resistive element for calibrated cold/heat steps, with T logged at the tissue.
- **Mechanical wounding:** reproducible cut/crush; note this wounds tissue and interacts with electrode insertion. Arabidopsis surface-potential wound protocols (e.g. Farmer-lab guides using non-invasive surface electrodes) are the methodological template, including running parallel recordings from multiple plants.
- **Chemical microdelivery:** micropipette/perfusion of glutamate (wound-signal agonist; in vascular plants, glutamate/cation application to wounds gives hyperpolarizations of ~10–60 mV lasting 4–30 min) and H₂O₂/ROS. Volumes µL-scale (Block 16).
- **Trigger synchronization:** a common TTL/timestamp shared with the DAQ so stimulus onset is logged on a data channel. Without hardware-logged triggers you cannot align cause and effect at the required time resolution.

**(d) Classes:** LED + driver (€10–50), Peltier + controller (€20–80), micromanipulator + pipette (Block 16), Arduino for triggering (€10–30). Perfusion: DIY syringe pump (€30–100).

**(e) Substitutions:** Manual stimulus with a logged wall-clock event loses timing precision; acceptable for slow (hour-scale) waves, poor for ~0.4 mV fast spikes. Broadband white light instead of a defined spectrum loses mechanistic interpretability.

**(f) Verification:** Confirm the trigger channel records stimulus onset; measure actual delivered PPFD/temperature at the specimen; test glutamate delivery volume/positioning on a dye phantom.

---

### Block 11 — Spatial / multichannel expansion

**(a)** Scale beyond 8 differential pairs to a 2-D electrode array, and synchronize multiple rigs.

**(b)** Gap 4 (spatial mapping) and the replication imperative (running 4 preparations at once).

**(c) Specs:** Buffered multiplexing only — each electrode buffered (Block 1b) before any analog MUX to avoid charge-injection/settling errors into megohm sources. Channel-count target ≥16–32 for a spatial map; inter-rig time synchronization to ≤1 s (shared clock/NTP-disciplined timestamp, or a common trigger line). Propagation velocities are inferred from inter-electrode timing, so timebase alignment across channels/rigs is essential.

**(d) Classes:** Multiple ADC-24s on one PC (Pico supports this) €1,000+ each; or DIY ADS1256 boards (8 ch each) ganged with a shared SPI clock and sync line, €30–120/board. Buffered MUX (ADG-series + electrometer buffers).

**(e) Substitutions:** Time-division multiplexing without per-channel buffers loses accuracy into moss impedance. Free-running separate loggers without a shared timebase lose cross-rig timing (fine for independent replication, not for spatial propagation across rigs).

**(f) Verification:** Inject a common synthetic wave into all channels/rigs via the phantom and confirm timestamp alignment ≤ spec.

---

### Block 12 — Electrical characterization (impedance spectroscopy, four-terminal, I-V)

**(a)** Measure moss complex impedance/dielectric spectrum and I-V/memristance-like behavior vs moisture.

**(b)** Gap 5 (impedance/dielectric characterization — the moisture-response / memristance analogue).

**(c) Specs and reasoning:**

- **Frequency range extends well below 1 kHz, down to sub-Hz.** Tissue α-dispersion and water-binding in a poikilohydric organism live at low frequencies. **Functional requirement: sweep ~0.1 Hz (or lower) to ~100 kHz.**
- **Four-electrode (tetrapolar) measurement is effectively mandatory for tissue.** Two-electrode methods suffer large electrode-polarization errors at low frequency: published tissue work shows the four-electrode method accurate below ~50 kHz while two-electrode is only trustworthy above ~10 kHz–200 kHz (two-electrode conductivity deviates rapidly below ~10 kHz due to polarization). Since your band of interest is sub-kHz, tetrapolar is required. The voltage-sense electrodes must feed high-impedance buffers (≥100 MΩ, ideally TΩ, ~pA bias) so no current flows through them.
- **Single-chip converters (AD5933 class) are inadequate:** the AD5933 works ~1–100 kHz on its internal 16.776 MHz clock; below 1 kHz its 1024-point buffer "cannot store a full period," so error grows and an external slower clock is required — and even then it is a two-electrode device, unusable for tissue below ~10 kHz without heroics.
- **Excitation limits:** keep applied voltage low (chemical/biological EIS uses <2 Vpp; for tissue keep well below electrolysis threshold, ≲50–100 mV across the tissue) and current in the sub-µA to few-µA range to avoid electrolysis/tissue damage. Current sensitivity to ~nA.

**(d) Classes:**

- *DIY sub-Hz:* software lock-in — a DAC generates the excitation sine, a high-resolution ADC samples voltage and current, and software multiplies by reference sin/cos to extract magnitude/phase. Ideal for sub-Hz to ~kHz where no commercial chip works well. ~€30–100 on hardware you may already have.
- *DIY audio band:* sound-card lock-in (e.g. Daqarta-style, which turns a sound card into an LCR/impedance meter) covers ~20 Hz–20 kHz cheaply.
- *Chip-based:* AD5933/PmodIA (€30–60) for ≥1 kHz only, two-electrode — use only for the high-frequency tail.
- *Commercial:* benchtop LCR meter or potentiostat/impedance analyzer with four-terminal capability (€1,000s+).

**(e) Substitutions:** AD5933 alone loses the entire sub-kHz tissue-relevant band and four-terminal capability — the two things you actually need. Sound-card lock-in loses DC–20 Hz. A software lock-in via your existing DAC+ADC is the best-value path to the full band.

**(f) Verification:** Measure known RC networks (the phantom) across the full sweep and confirm ≤ few-% magnitude and ≤1° phase error; confirm the four-terminal reading is independent of electrode-contact impedance by deliberately degrading a contact.

---

### Block 13 — Electrical stimulation for reservoir computing

**(a)** Inject controlled electrical stimuli through separate stimulating electrodes for reservoir-computing input.

**(b)** Gap 7 (reservoir-computing benchmark) and gap 6 (closed loop).

**(c) Specs and reasoning:**

- **Separate stimulating electrodes** (not the recording pair).
- **Charge-balanced biphasic waveforms** — net-zero charge prevents electrode corrosion and tissue electrochemical damage; DC injection through Ag/AgCl consumes the chloride layer and destroys the electrode. Charge-balance error should be small (commercial/integrated stimulators achieve <0.13% LSB; aim ≪1%).
- **Isolated constant-current output** — current-mode (not voltage) so delivered charge is defined regardless of tissue-impedance changes; galvanically isolated so the stimulus cannot create ground loops into the sensitive recorder.
- **Current range & compliance:** tissue-appropriate currents from ~µA to low mA; compliance high enough to drive current through 1–2 MΩ — 1 µA into 2 MΩ needs 2 V, 10 µA needs 20 V, so **compliance of ~±20–40 V** is realistic (commercial constant-current stimulators offer ±25 V expandable to ±70 V; DIY FES designs reach hundreds of V). Pulse widths µs–s.

**(d) Classes:** DIY isolated current source (op-amp Howland + isolated supply + DAC, STM32/Arduino control) €30–150; commercial isolated biphasic constant-current stimulator (Digitimer DS3/DS8R class, A-M Systems, NPI) €1,000s.

**(e) Substitutions:** Voltage-mode stimulation loses charge control as impedance drifts (moss hydration changes Z constantly). Non-isolated stimulation loses ground-loop immunity and corrupts the recorder. Monophasic DC destroys Ag/AgCl.

**(f) Verification:** Into the phantom, confirm delivered current is constant vs load, measure charge-balance error on a series sense resistor, confirm isolation (no continuity front-end↔mains).

---

### Block 14 — Actuation and closed-loop control

**(a)** Read moss signal in real time and drive an actuator, safely and unattended.

**(b)** Gap 6 (closed-loop system).

**(c) Specs:** Real-time streaming access to the DAQ (Python API/driver). The template is Mishra et al. 2024, "Sensorimotor control of robots mediated by electrophysiological measurements of fungal mycelia," *Sci. Robot.* 9(93):eadk8019 (Cornell Organic Robotics Lab; lead author Anand Kumar Mishra, senior author Robert F. Shepherd; king oyster mushroom *Pleurotus eryngii*), which used a PicoLog ADC-24 at 10 S/s on ±39 mV with no preamp, stainless subdermal needles of 1.4–2.2 MΩ DC resistance, Python/SciPy `find_peaks` + Savitzky–Golay smoothing for spike detection, and an Arduino via pyFirmata driving actuators. Add a **galvanic isolation barrier** between mains-referenced actuator power and the front end, and a **watchdog timer** to fail-safe the actuator if the control loop hangs during an unattended multi-day run.

**(d) Classes:** Arduino/ESP32 (€5–20), pyFirmata/Firmata, opto-isolated relay/MOSFET driver boards (€5–20), isolated DC-DC (€5–15). Streaming: Pico SDK or ADS1256 driver.

**(e) Substitutions:** Polling instead of streaming loses latency/throughput; acceptable for hour-scale control, not for spike-triggered actuation. No watchdog risks a stuck actuator over days.

**(f) Verification:** Closed-loop test on the phantom driving a synthetic "spike"→actuator; pull the USB mid-run to confirm the watchdog safes the actuator.

---

### Block 15 — Bioelectrochemistry / microbial fuel cell

**(a)** Build a bryophyte MFC anode/cathode and log its output while simultaneously recording electrophysiology.

**(b)** Gap 8 (does a moss carrying an anode still signal normally?).

**(c) Specs:**

- **Anode:** high-surface-area carbon — Bombelli's bryoMFC used a 3-D carbon-fibre anodic matrix (a paper:carbon-fibre pulp blended with deionized water, with a stainless-steel bolt as electron collector) chosen for biocompatibility, water retention, and low electric resistance. Carbon felt/cloth/fibre.
- **Cathode:** carbon with catalyst / air cathode.
- **Reference electrode:** Ag/AgCl for half-cell potentials.
- **Variable load** for polarization/power curves (decade resistance box, µA–mA).
- **Isolated cell-voltage logging** on a separate isolated channel so the MFC circuit does not corrupt the electrophysiology front end. Outputs are small: 6.7 ± 0.6 mW m⁻² (environmental non-sterile) down to 0.2 ± 0.1 µW m⁻² (near-sterile), so the microbial contribution dominates and must be controlled/measured.

**(d) Classes:** Carbon felt/fibre (€10–40), decade resistance box (€15–60), extra isolated ADC channel, Ag/AgCl reference (Block 2).

**(e) Substitutions:** Metal anodes are possible but change biocompatibility/chemistry; you lose comparability to the bryoMFC literature. Sharing a ground between MFC and electrophysiology loses isolation.

**(f) Verification:** Polarization curve on the assembled cell; confirm electrophysiology noise floor is unchanged with the MFC connected vs disconnected (the key control for gap 8).

---

### Block 16 — Specimen handling and micromanipulation

**(a)** Inspect, position electrodes, and deliver µL reagents under magnification.

**(b)** Supports gaps 3, 4, 5 (precise electrode/stimulus placement on a small cushion).

**(c) Specs:** Stereo microscope ~10–40× (working distance for electrode access); micromanipulator with ~µm-scale movement for electrode/pipette placement; micropipettes covering ~0.5–1000 µL for reagent delivery.

**(d) Classes:** Stereo microscope (€100–400 used), DIY/entry micromanipulator (€50–300; lab-grade €1,000s), adjustable micropipette set (€30–150).

**(e) Substitutions:** A loupe/USB microscope loses stereo depth for manipulation. Manual electrode placement loses reproducibility of inter-electrode spacing (which sets propagation-velocity accuracy).

**(f) Verification:** Confirm pipette-delivered volumes gravimetrically on the balance; confirm micromanipulator resolution against a graticule.

---

### Block 17 — Data infrastructure

**(a)** Gapless multi-day logging, robust storage, UPS, redundancy, timebase.

**(b)** Underpins every recording gap; the anchor's value came from 178 h of unbroken data.

**(c) Specs:** Gapless acquisition ≥178 h; open, self-describing storage (CSV for small, HDF5/Parquet for large/multichannel); UPS to ride through mains dips (a single blackout ends a 7-day run); redundant copies (local + offsite); a disciplined timebase (NTP) with a hardware trigger channel for stimulus alignment. At 10 S/s × 32 ch × 7 days ≈ 200 M samples — plan storage and format accordingly.

**(d) Classes:** Mini-PC/Raspberry Pi (€50–150), UPS (€60–200), NAS/cloud backup, PicoLog or custom Python logger.

**(e) Substitutions:** A laptop on battery is a poor UPS (sleep/updates kill runs). CSV at high channel counts loses efficiency; use HDF5.

**(f) Verification:** Do a 24 h dry run on the phantom, then simulate a power cut and confirm data integrity and auto-restart.

---

### Block 18 — Analysis software stack (all open source)

**(a)** Spike detection, complexity/entropy measures, and reservoir-computing benchmarks.

**(b)** Gaps 3, 5, 7.

**(c) Specs:** Python/NumPy/SciPy (`find_peaks`, Savitzky–Golay — the Mishra/Shepherd method), pandas, plus reservoir-computing libraries (e.g. reservoirpy) and complexity measures (Lempel–Ziv, entropy). Reproducibility requires open, scriptable tools and archived analysis code. For gap 7 you must benchmark moss against a plain sensor-array baseline (same electrodes into inert substrate) to prove the moss adds computational value — the substrate-independent reservoir-characterization framework (Dale et al.) and the plant-as-reservoir precedent (strawberry, 8 leaf-thickness sensors) show how to structure separability/memory-capacity/nonlinearity metrics.

**(d) Classes:** All free/open source.

**(e) Substitutions:** Proprietary GUI-only tools lose reproducibility.

**(f) Verification:** Run detectors on the phantom's synthetic spikes/waves with known ground truth; confirm detection precision/recall; confirm the reservoir benchmark distinguishes moss from the inert baseline only when it should.

---

### Block 19 — Consumables, biological material, and reagent sourcing in Germany

**(a)** Moss, substrates, media, reagents, and legal routes to obtain them privately.

**(b)** Enables all biology; gaps 2/3/8 need specific reagents.

**(c) Specs and legal notes:**

- **Moss species:** For comparability, use *Brachythecium rutabulum* (the anchor species; common "rough-stalked feather-moss") and/or the model *Physcomitrium/Physcomitrella patens* (used in bryoMFC and molecular work; obtainable from culture collections/labs). Collect wild moss under the German **Handstrauß rule (§ 39 Abs. 3 BNatSchG)**: anyone may take wild mosses "in geringen Mengen für den persönlichen Bedarf" from places without an access ban; the customary "small amount" is what fits between thumb and forefinger, and you should take no more than about half a cushion to allow regeneration. **But** specially/strictly protected species (besonders/streng geschützte Arten per BArtSchV, FFH Annex IV — checkable in the BfN WISIA database) may not be taken, and nature reserves/national parks/private land are excluded; commercial-scale harvesting of moss carpets is explicitly not covered ("Verwüstung"). Fines range roughly €50 to tens of thousands of euros depending on Land and offense.
- **Substrates/media:** deionized water, agar, Knop's/BCD medium for *P. patens*, KCl for salt bridges.
- **Reagents:** L-glutamate, H₂O₂ (ROS), staining dyes, salts for RH chambers.
- **German chemical procurement for private individuals:** the **Chemikalien-Verbotsverordnung (ChemVerbotsV)** plus EU REACH Annex XVII and precursor rules restrict sale to the public. Substances bearing **GHS06 (skull-and-crossbones — toxic, e.g. sodium azide)** may NOT be shipped to private individuals — only to resellers, professional users, or public research/teaching institutions. Restricted substances such as **H₂O₂ >12%, HNO₃ >3%, H₂SO₄ >15%** are barred to the public above those thresholds. Many chemicals can still be sold to private buyers on **local pickup (Abholung)** from specialist lab shops that explicitly serve Privatpersonen on-site (e.g. Laborfachgeschäft-type dealers offering 10 g–1 L pack sizes to private customers over the counter), and via pharmacies with a documented Abgabegespräch. Practical route: buy low-concentration/unrestricted grades online; buy restricted items in person; for GHS06/toxic reagents partner with a hackerspace/makerspace registered as an institution, or substitute.

**(d) Classes:** Free (wild moss), culture collections for model species, lab suppliers (Carl Roth, VWR, Merck via institutional or pickup routes), pharmacies.

**(e) Substitutions:** For toxic metabolic inhibitors (azide) unobtainable privately, substitute cold/autoclave killing for controls and antibiotics for microbial suppression where legal. You lose some specificity but stay legal.

**(f) Verification:** Confirm species ID (microscopy/keys) before comparability claims; keep purchase records.

---

### Block 20 — Safety, legal, and procurement summary

**(a)** Electrical safety, chemical handling, and legal compliance.

**(b)** Cross-cutting.

**(c) Specs:** Mains isolation and RCD protection for any mains-powered actuator near wet specimens; the recording front end galvanically isolated (Block 4); charge-balanced isolated stimulation (Block 13) is also a safety feature. Chemical: ventilation for H₂O₂/reagents, PPE, SDS on file. Legal: obey BNatSchG collection rules and ChemVerbotsV procurement rules (Block 19).

**(d–f)** Verify isolation with a continuity/insulation test before each new configuration; maintain an SDS binder; document collection sites and amounts.

## Recommendations — Prioritized Acquisition Sequence (by capability)

**Stage 0 — Prove the rig before touching moss (must exist first).**

1. DC acquisition (Block 1) + high-impedance buffers (Block 1b) + calibrator & phantom (Block 3) + basic shielding/isolation (Block 4) + data infra/UPS (Block 17) + analysis stack (Block 18).

- *Benchmark to proceed:* on the RC phantom, demonstrate noise floor ≤ ~10 µV RMS, quantization ≤ 25 µV, drift ≤ ~100 µV/h over 24 h, correct recovery of an injected 250 µV step and a synthetic 30 mV/6 h ramp, and ≥20 dB line-noise suppression in the cage. If you cannot resolve the phantom, you cannot trust moss data.

**Stage 1 — First credible biology (closes the worst anchor gaps).**
2. Non-polarizable electrodes (Block 2) + environmental measurement in correct units (Block 5) + hydration gravimetry (Block 7) + viability/death (Block 8) + sterility (Block 9) + moss/reagents (Block 19), run as **four simultaneous preparations** (living / freshly dead / autoclaved / inert substrate) — replication over bit-depth.

- *Benchmark:* reproduce the anchor's signal classes in living moss AND show they are absent/different in the three controls, with logged T/RH/PPFD and RWC. This is the publishable "controls + environment" result.

**Stage 2 — Perturbation and mechanism.**
3. Environmental control chamber (Block 6) + stimulus delivery with triggers (Block 10) + specimen handling (Block 16).

- *Benchmark:* reproducible stimulus-locked responses (light/thermal/wound/glutamate/H₂O₂) with logged triggers; map activity across a full wetting→drying→rewetting cycle.

**Stage 3 — Characterization and scaling.**
4. Impedance/four-terminal (Block 12) + spatial/multichannel expansion (Block 11).

- *Benchmark:* sub-Hz–100 kHz tetrapolar spectrum vs hydration; 2-D propagation maps with a verified timebase.

**Stage 4 — Computing and hybrids (optional/advanced).**
5. Isolated stimulation (Block 13) + closed-loop actuation (Block 14) + reservoir benchmark (Block 18) + bryoMFC integration (Block 15).

- *Benchmark:* moss reservoir beats the inert-substrate baseline on a defined task; MFC-anode-bearing moss still produces its normal signal repertoire (gap 8 answered).

**Thresholds that change the plan:** If the phantom shows a DIY ADC-24 alternative cannot hit ≤25 µV quantization, buy the commercial ADC-24. If electrode drift exceeds ~150 µV/h, switch from stainless/Ir needles to Ag/AgCl. If controls reproduce the "moss" signals, stop and treat the phenomenon as instrumental until resolved.

## Caveats

- **The anchor result is n=1 with no controls and unmonitored environment** — its own author states: "No dedicated control recordings … were performed … and environmental variables, such as humidity and temperature, were not continuously monitored." Treat all reported amplitudes/periodicities as provisional targets for your specs, not established biology. Your lab's primary scientific value is supplying exactly the controls, replication, and environmental logging that are missing.
- **"Effective resolution" ≠ nominal bits.** A "24-bit" logger delivers ~19–20 noise-free bits (Pico's own tables); specify by noise floor in µV and noise-free bits, not marketing bit-depth. Pico's figures are peak-to-peak noise-free bits, not RMS ENOB, and no offset temperature-drift figure is published (accuracy guaranteed only 20–30 °C).
- **DIY high-impedance work is defeated by humidity and leakage** — the very humid chamber you need for moss attacks the fA front end. Guard rings, PTFE, conformal coating, and periodic phantom checks are not optional.
- **Lux is not a photobiology unit.** Any comparison to the anchor's "~5 lux" must be restated in PPFD with the source spectrum; the conversion is spectrum-dependent and otherwise meaningless.
- **Legal constraints are real:** collect moss only under the Handstrauß rule and avoid protected species/areas; several useful reagents (notably toxic inhibitors like sodium azide, GHS06) cannot be shipped to private individuals under ChemVerbotsV — plan institutional/pickup routes or substitutions.
- **Load-cell and electrode drift co-vary with temperature, which co-varies with the biology.** Without logged temperature you cannot separate artifact from signal — which is why Block 5 is Stage-1, not optional.
- Price bands are indicative ranges for the DIY/hackerspace tier and will vary with the German used-equipment and surplus market; verify current pricing at purchase.
