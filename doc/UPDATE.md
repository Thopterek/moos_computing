# Every claim below has to be read and checked

The main advantage are the gaps that should be tested first

## Moss Computing

A working bibliography for a project that would run a system on living moss — as a sensor, a signal-propagating substrate, an energy source, or some combination. Four separate literatures converge here and are usually not cited by each other: bryophyte electrophysiology (ion channels and action potentials, studied mostly in the model moss *Physcomitrium patens*), unconventional computing with living substrates (Adamatzky's fungal and plant work, slime-mould computing, reservoir computing), moss bioenergy (microbial fuel cells, where the current comes from associated microbes rather than the moss itself), and the applied moss literature (cultivation, bioreceptive facades, microbiome). Each source below appears exactly once, under its best-fitting heading. A fifth strand runs underneath all of them: the negative results, failed replications and artifact explanations from adjacent fields, which is where the methodology for a credible moss experiment actually comes from.

## Sources

### Seed paper

* [Multi-scale electrical activity and signal propagation in the moss Brachythecium rutabulum](https://royalsocietypublishing.org/rsos/article/13/7/252341/482568/Multi-scale-electrical-activity-and-signal) — the anchor: 178 h of eight-channel differential recording on a wild moss cushion, reporting fast spikes, ~1 h rhythms, slow 30 mV depolarization waves, and lag-based evidence of propagation.
* [Towards intelligent living facades: On electrical activity of ordinary moss Brachythecium rutabulum](https://www.biorxiv.org/content/10.1101/2024.04.28.591491) — the earlier preprint, with full electrode and logger methods and the living-facade motivation spelled out more explicitly.
* [Scientists Listened to Moss for a Week and Found Strange Electrical Pulses Moving Through It](https://www.zmescience.com/science/biology/scientists-listened-to-moss-for-a-week-and-found-strange-electrical-pulses-moving-through-it/) — the one piece of coverage that states the study's limitations plainly: no dead-moss control, unmonitored temperature and humidity, possible electrode drift.

### Moss and bryophyte electrophysiology

* [Cation-permeable vacuolar ion channels in the moss Physcomitrella patens: a patch-clamp study](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3722460/) — single-channel characterization of the tonoplast SV channel, the molecular hardware behind moss excitability.
* [Glutamate-Induced Electrical and Calcium Signals in the Moss Physcomitrella patens](https://academic.oup.com/pcp/article/61/10/1807/5893958) — the key result for system design: electrical signals travel long distances in moss while calcium transients stay local.
* [Long-Distance Electrical and Calcium Signals Evoked by Hydrogen Peroxide in Physcomitrella](https://academic.oup.com/pcp/article/64/8/880/7180321) — extends systemic electrical signalling to ROS, calcium-dependent and inhibitor-sensitive. (paywalled)
* [Rapid Propagation of Ca2+ Waves and Electrical Signals in the Liverwort Marchantia polymorpha](https://doi.org/10.1093/pcp/pcad159) — wound-induced fast waves in the closest well-studied non-vascular relative, same methodological lineage. (paywalled)
* [GLR-dependent calcium and electrical signals are not coupled to systemic, oxylipin-based wound-induced gene expression in Marchantia polymorpha](https://nph.onlinelibrary.wiley.com/doi/10.1111/nph.19803) — shows a single glutamate-receptor-like channel is required for long-distance signalling, i.e. a concrete genetic knob.
* [Wound-induced signals in Marchantia — commentary on the MpGLR result](https://doi.org/10.1111/nph.19927) — short companion piece, useful for orientation on what the MpGLR finding does and does not establish. (paywalled)
* [GLUTAMATE RECEPTOR-LIKE channels are essential for chemotaxis and reproduction in mosses](https://doi.org/10.1038/nature23478) — moss GLRs tie the signalling machinery to development, identifying knockout targets. (paywalled)

### Control and stimulation

* [Light- and dark-induced action potentials in Physcomitrella patens](https://www.tandfonline.com/doi/full/10.4161/psb.3.1.4884) — the same moss cells fire on both illumination and darkening; light is the cheapest and most reproducible input available.
* [Calcium-dependent membrane depolarisation activated by phytochrome in the moss Physcomitrella patens](https://link.springer.com/article/10.1007/BF00195726) — voltage-clamp dissection of red-light depolarization and the ion fluxes at its peak. (paywalled)
* [Effect of cold and menthol on membrane potential in plants](https://www.researchgate.net/publication/49736965_Effect_of_cold_and_menthol_on_membrane_potential_in_plants) — temperature and TRP-like chemical agonists as controllable stimuli.
* [Cold- and menthol-evoked membrane potential changes in the moss Physcomitrella patens](https://doi.org/10.1111/ppl.12918) — the moss-specific follow-up, with ion-channel inhibitor and phytohormone dissection. (paywalled)
* [Automated Phytosensing: Ozone Exposure Classification Based on Plant Electrical Signals](https://arxiv.org/abs/2412.13312) — the closest existing template for turning plant electrophysiology into a trained classifier of an environmental input.

### Plant electrophysiology foundations

* [Long-Range Signals Built upon Plant Structural Continuity](https://www.annualreviews.org/content/journals/10.1146/annurev-arplant-070225-024248) — current synthesis of how electrical, hydraulic and chemical long-range signals are carried. (paywalled)
* [Electrical signals and their physiological significance in plants](https://doi.org/10.1111/j.1365-3040.2006.01614.x) — the standard entry point on action potentials and variation potentials. (paywalled)
* [Electrical Signaling of Plants under Abiotic Stressors: Transmission of Stimulus-Specific Information](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8509212/) — argues signal shape encodes which stimulus occurred, the theoretical basis for moss-as-sensor.

### Unconventional computing with living substrates

* [Towards plant wires](https://arxiv.org/abs/1401.4396) — treats living plant tissue as a circuit component and measures its transfer function and noise immunity.
* [Towards fungal computer](https://doi.org/10.1098/rsfs.2018.0029) — the programmatic statement of computing with a living mycelial substrate.
* [On spiking behaviour of oyster fungi Pleurotus djamor](https://doi.org/10.1038/s41598-018-26007-1) — the origin paper for the whole electrode-in-living-tissue research line.
* [Language of fungi derived from their electrical spiking activity](https://doi.org/10.1098/rsos.211926) — the spike-word and complexity analysis that the moss paper reuses almost directly.
* [Does electrical activity in fungi function as a language?](https://www.sciencedirect.com/science/article/abs/pii/S1754504823001034) — the formal rebuttal; read next to the above, since the critique applies to moss too. (paywalled)
* [Fungal electronics](https://research-portal.uu.nl/en/publications/fungal-electronics) — defines living devices that change impedance and spike under external control; the template for a "moss electronics" framing.
* [Electrical frequency discrimination by fungi Pleurotus ostreatus](https://arxiv.org/abs/2210.01775) — a living substrate distinguishing input frequencies, about the simplest real computation demonstrated in this line.
* [Propagation of electrical signals by fungi](https://arxiv.org/abs/2304.10675) — how far and how faithfully information can travel through living tissue.
* [Electrical activity of fungi: Spikes detection and complexity analysis](https://arxiv.org/abs/2008.10276) — the spike-detection and information-theoretic pipeline used on moss.
* [Fungal photosensors](https://arxiv.org/abs/2003.07825) — light as input to a living substrate, the closest fungal analogue of light-triggered moss signalling.
* [Fungi anaesthesia](https://arxiv.org/abs/2106.09007) — chemical suppression of spiking, a useful negative-control concept for proving signals are biological.
* [Logics in fungal mycelium networks](https://arxiv.org/abs/2112.07236) — attempts to map Boolean functions onto a living network's spiking.
* [Maze-solving by an amoeboid organism](https://www.nature.com/articles/35035159) — the founding demonstration that a non-neural organism can solve a spatial problem.
* [Rules for biologically inspired adaptive network design](https://www.science.org/doi/10.1126/science.1177894) — slime mould reproducing the Tokyo rail network; the canonical "living substrate computes something useful" result. (paywalled)
* [Leveraging plant physiological dynamics using physical reservoir computing](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9307625/) — the most transferable precedent: plants work as reservoirs for eco-physiological tasks but not general computation.

### Labs, projects and venues

* [Andrew Adamatzky — UWE Bristol staff profile](https://people.uwe.ac.uk/Person/AndrewAdamatzky) — director of the Unconventional Computing Laboratory, the group behind the seed paper and nearly all the fungal work above.
* [Andrew Adamatzky — Google Scholar](https://scholar.google.com/citations?user=suo5D8wAAAAJ&hl=en) — fastest way to track forward citations and new preprints in this area.
* [Andrew Adamatzky — Wikipedia](https://en.wikipedia.org/wiki/Andrew_Adamatzky) — orientation on the wider programme (reaction-diffusion computing, Physarum machines, *Fungal Machines*) and the journals he edits.
* [Fungal Architectures (FUNGAR) — CORDIS project record](https://cordis.europa.eu/project/id/858132) — €2.86M FET-Open project on a combined structural and computational living substrate for architecture; the direct institutional precedent for a moss facade.
* [Fungal Architectures — Royal Danish Academy (CITA) project page](https://royaldanishacademy.com/en/case/fungal-architectures) — the consortium view: architects, computer scientists, mycologists, industry.
* [Fungal architecture — position paper](https://arxiv.org/abs/1912.13262) — the consortium's opening argument for growing sentient buildings from a living substrate.
* [WatchPlant project](https://watchplantproject.eu/) — EU project using living plants as self-powered urban air-quality sensors; the "phytosensing" programme.
* [WatchPlant — Cyber-physical Systems, University of Konstanz](https://www.cps.uni-konstanz.de/watchplant/) — group page covering the technical and AI side of the phytosensor network.
* [WATCHPLANT — CORDIS project record](https://cordis.europa.eu/project/id/101017899) — objectives, budget and consortium; useful for finding deliverables and hardware reports.
* [International Journal of Unconventional Computing](https://www.oldcitypublishing.com/journals/ijuc-home/) — the field's dedicated venue, where much of this work appears outside the large journals.

### Biohybrid systems and robotics

* [Sensorimotor control of robots mediated by electrophysiological measurements of fungal mycelia](https://www.science.org/doi/10.1126/scirobotics.adk8019) — the state of the art and the best engineering blueprint: shielded interface, spike detection, CPG controller, UV light as modulating input. (paywalled)
* [Flora robotica — an architectural system combining living natural plants and distributed robots](https://arxiv.org/abs/1709.04291) — plant-robot hybrid architecture including onboard electrophysiology and impedance sensing hardware.
* [Autonomously shaping natural climbing plants: a bio-hybrid approach](https://doi.org/10.1098/rsos.180296) — closed-loop control of plant growth by robots, i.e. actuating on a living substrate rather than only reading from it.
* [Constructing living buildings: a review of relevant technologies for a novel application of biohybrid robotics](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6685033/) — connects living facades to biohybrid robotics; the review to cite when framing the application.

### Plant bioelectronic interfaces

* [Electronic plants](https://doi.org/10.1126/sciadv.1501136) — PEDOT conducting wires and logic self-assembled inside living plant tissue; the materials route to wiring a plant.
* [Benchmarking organic electrochemical transistors for plant electrophysiology](https://doi.org/10.3389/fpls.2022.916120) — practical OECT recording of plant action potentials, a higher-gain alternative to needle electrodes.
* [Plant electrophysiology with conformable organic electronics: deciphering the propagation of Venus flytrap action potentials](https://www.science.org/doi/10.1126/sciadv.adh4443) — what high-density, well-controlled plant recording actually looks like, and the benchmark the moss work is far from meeting. (paywalled)

### Moss bioenergy

* [Electrical output of bryophyte microbial fuel cell systems is sufficient to power a radio or an environmental sensor](https://royalsocietypublishing.org/rsos/article/3/10/160249/36599/Electrical-output-of-bryophyte-microbial-fuel-cell) — 6.7 mW/m² from environmental moss on a 3D anode, enough to run a radio; note the current comes from anodophilic microbes, not moss action potentials.

### Moss biology, genetics and cultivation

* [The Physcomitrella Genome Reveals Evolutionary Insights into the Conquest of Land by Plants](https://www.science.org/doi/10.1126/science.1150646) — the genome that made moss a tractable model organism. (paywalled)
* [Insights into Land Plant Evolution Garnered from the Marchantia polymorpha Genome](https://www.sciencedirect.com/science/article/pii/S0092867417311248) — the liverwort counterpart, notable for low genetic redundancy in regulatory pathways.
* [Moss-made pharmaceuticals: from bench to bedside](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4736463/) — the moss bioreactor programme: GMP cultivation, 500 L wave reactors, cryopreservation; the best evidence that moss can be grown to spec.
* [Glyco-engineering for biopharmaceutical production in moss bioreactors](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4089626/) — the genetic-engineering toolkit, including the high homologous-recombination advantage.
* [Process Engineering of Biopharmaceutical Production in Moss Bioreactors](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8891706/) — growth kinetics and phytohormone control of moss culture; directly useful for keeping a substrate alive and reproducible.

### Moss water relations and desiccation tolerance

* [Desiccation Tolerance in Moss and Liverwort: Insights into the Evolutionary Mechanisms of Terrestrialization](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12787291/) — current review of drying and rehydration; sets the hydration operating window and recovery timescales any deployed system must respect.

### Moss microbiome

* [Reviewing bryophyte-microorganism association: insights into environmental optimization](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11211263/) — overview of the bacterial and fungal endophytes sharing the tissue you would be recording from.
* [Biotic and abiotic controls of nitrogen fixation in cyanobacteria-moss associations](https://nph.onlinelibrary.wiley.com/doi/10.1111/nph.18264) — the moss-cyanobacteria relationship and how conditions move it along the mutualism-parasitism continuum. (paywalled)
* [Revealing the transfer pathways of cyanobacterial-fixed N into the boreal forest through the feather-moss microbiome](https://www.frontiersin.org/journals/plant-science/articles/10.3389/fpls.2022.1036258/full) — NanoSIMS evidence of active nitrogen exchange between moss, cyanobacteria and associated microbes.
* [Transfer of fixed-N from N2-fixing cyanobacteria associated with the moss Sphagnum riparium results in enhanced growth of the moss](https://link.springer.com/article/10.1007/s11104-012-1278-4) — quantifies the transfer; relevant to feeding a long-running moss substrate. (paywalled)
* [New insights into the drivers of moss-associated nitrogen fixation and cyanobacterial biomass](https://besjournals.onlinelibrary.wiley.com/doi/abs/10.1111/1365-2745.13881) — which environmental variables drive the microbial component, i.e. the confound in any long recording. (paywalled)

### Moss in architecture and built environments

* [Bioreceptivity evaluation of cementitious materials designed to stimulate biological growth](https://doi.org/10.1016/j.scitotenv.2014.02.059) — the founding bioreceptive-concrete paper; low-pH cementitious substrate for moss and lichen colonization. (paywalled)
* [Making bioreceptive concrete: formulation and testing of bioreceptive concrete mixtures](https://www.sciencedirect.com/science/article/pii/S2352710221004022) — four practical mix strategies (expanded clay, bone ash, particle size, porosity) for water retention and colonization.
* [Moss species for bioreceptive concrete: A survey of epilithic urban moss communities and their dynamics](https://www.sciencedirect.com/science/article/pii/S0925857424003276) — which species actually colonize fresh substrate and persist; species selection is the first decision in a facade system. (paywalled)
* [Growing moss on bioreceptive concrete using a novel two-step approach: The effects of light, water, and species selection](https://www.sciencedirect.com/science/article/pii/S0925857425003295) — the survivability problem: indoor-grown panels often fail outdoors without environmental hardening. (paywalled)
* [Bioreceptivity of living walls: Interactions between building materials and substrates, and effect on plant growth](https://www.sciencedirect.com/science/article/abs/pii/S1618866723000833) — magnesia-composite panels at neutral pH, plus the environmental conditions moss needs on a wall. (paywalled)
* [Moss as a Multifunctional Material for Technological Greenery Systems](https://www.theplanjournal.com/article/moss-multifunctional-material-technological-greenery-systems) — the design-side history, including poikilohydric wall prototypes and installed panel pilots.

### Negative results, critiques and failed claims

* [Electrode insertion generates slow propagating electric potentials in Myriophyllum aquaticum plants](https://pmc.ncbi.nlm.nih.gov/articles/PMC7194371/) — the single most important paper here: inserting the electrode itself produces slow, propagating potentials with no external stimulus, which is exactly the class of signal the moss paper reports.
* [Drift Removal in Plant Electrical Signals via IIR Filtering Using Wavelet Energy](https://arxiv.org/abs/1611.09766) — states plainly that no framework exists for identifying artifacts in plant electrical signals, and that pre-stimulus drift is routine.
* [Plant neurobiology: no brain, no gain?](https://pubmed.ncbi.nlm.nih.gov/17368081/) — 36 plant physiologists arguing the neural framing outran the evidence; the template for every subsequent "is this really computation" dispute. (paywalled)
* [Response to Alpi et al.: plant neurobiology — all metaphors have value](https://pubmed.ncbi.nlm.nih.gov/17499006/) — the reply, for the other side of the argument. (paywalled)
* [Lack of evidence for associative learning in pea plants](https://elifesciences.org/articles/57614) — a high-profile plant learning result that failed to replicate at larger sample size with a tightened control.
* [Comment on 'Lack of evidence for associative learning in pea plants'](https://elifesciences.org/articles/61141) — the original authors' rebuttal, arguing the replication protocol was unsuitable.
* [Response to comment on 'Lack of evidence for associative learning in pea plants'](https://elifesciences.org/articles/61689) — the closing exchange; the whole dispute is a case study in how under-specified protocols make a claim unfalsifiable.
* [When the path is never shortest: a reality check on shortest path biocomputation](https://arxiv.org/abs/1712.03139) — self-critique from inside the slime-mould field, showing the famous result only counts as computation under generous interpretation.
* [Thirty eight things to do with live slime mould](https://arxiv.org/abs/1512.08230) — Adamatzky's own candid assessment that unconventional computing is "an art of interpretation" and will not beat silicon.
* [Material Approximation of Data Smoothing and Spline Curves Inspired by Slime Mould](https://arxiv.org/abs/1503.03264) — its opening section is a frank list of why living substrates fail in practice: slow, environmentally fragile, unpredictable, imprecise.
* [The Missing Memristor has Not been Found](https://www.nature.com/articles/srep11657) — how a field can attach a prestigious label to ordinary behaviour and then defend the label rather than the claim.
* [The case for rejecting the memristor as a fundamental circuit element](https://doi.org/10.1038/s41598-018-29394-7) — the follow-up argument; relevant because "memristive" claims are routinely made for living tissue including plants and slime mould.
* [Reservoir Computing Benchmarks: a tutorial review and critique](https://arxiv.org/abs/2405.06561) — from the York in-materio group, on how physical reservoir results are evaluated badly and what a fair benchmark requires.
* [Plant microbial fuel cells: a comprehensive review of influential factors, innovative configurations, diverse applications, persistent challenges, and promising prospects](https://www.tandfonline.com/doi/full/10.1080/15435075.2024.2421325) — the honest accounting of the energy route: low power output, poor long-term durability, unresolved scaling and cost.
* [A Review of Recent Advances in Microbial Fuel Cells: Preparation, Operation, and Application](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9589990/) — why MFCs have not commercialised, on efficiency and operational stability grounds.

### Methods and hardware

* [Pico Technology ADC-20/ADC-24 high-resolution data logger](https://www.picotech.com/data-logger/adc-20-adc-24/precision-data-acquisition) — the logger used in the seed paper and the fungal work: 24-bit, ~1 Hz, ±156 mV range, cheap enough to replicate the recording immediately.
* [Equipment and protocol for measurement of extracellular electrical signals, gas exchange and turgor pressure in plants](https://pmc.ncbi.nlm.nih.gov/articles/PMC8374186/) — a full protocol with an explicit troubleshooting section on noise appearing across channels simultaneously.
* [A Detailed Guide to Recording and Analyzing Arabidopsis thaliana Leaf Surface Potential Dynamics Elicited by Mechanical Wounding](https://pmc.ncbi.nlm.nih.gov/articles/PMC11986695/) — agar bridges, proper grounding, and establishing a drift-free baseline before any stimulus; the discipline the moss work lacks.

## How this kind of experiment fails

Ordered roughly from most likely to least likely to sink a moss project.

**The electrode is itself a stimulus.** Inserting a needle wounds tissue, and wounding is one of the best-documented triggers of slow propagating potentials in plants. The Myriophyllum result shows this produces travelling signals with no experimenter-applied stimulus at all. A moss cushion with eight electrode pairs is eight wounds, and a lag between channels is exactly what a spreading wound response would look like. This is the leading alternative explanation for the seed paper's slow waves and its directed propagation, and nothing in that paper rules it out.

**Drift is indistinguishable from slow biology.** Two metal needles in a wet ionic substrate form a galvanic couple. Their offset changes as the local chemistry, moisture film, and corrosion products change, on a timescale of hours — the same timescale as the reported 6 h depolarizations and 18 h intervals. Plant electrophysiologists use Ag/AgCl electrodes with agar bridges precisely to avoid this, and note that the field has no agreed method for separating drift from signal. A 30 mV excursion is large for plant tissue and small for electrode chemistry.

**Evaporation and rehydration ramp the signal.** Moss is poikilohydric, so its conductivity, ion concentration, and membrane potential all track water content. In a closed humid container the water film still redistributes over hours. Any slow monotonic drying or wetting shows up as a slow depolarization-like ramp. Without continuously logged humidity, temperature, and mass, a hydration artifact and a physiological wave are the same trace.

**No dead control means no claim.** The cheapest decisive experiment is a parallel recording from autoclaved moss, or from wet substrate with no moss, using identical electrodes. If the spikes survive death, they were electrochemistry. This control was not run, and it is the first thing any reviewer or collaborator will ask for.

**The microbiome is a second, uncontrolled organism.** A moss cushion carries bacteria, cyanobacteria, fungi and algae, and sits on a substrate with its own redox gradients. Microbial metabolism generates real, slow, measurable potentials — that is precisely the mechanism the bryophyte fuel cell exploits. Attributing every fluctuation to moss cells is unwarranted unless sterile and non-sterile preparations are compared.

**Complexity measures do not prove biology.** Entropy, Lempel-Ziv, and multiscale entropy sit "between regular and random" for a very wide class of signals, including coloured noise, drifting instrumentation, and filtered thermal fluctuations. Reporting that a moss trace is neither periodic nor random is not evidence of computation. The missing step is surrogate testing: phase-randomised and amplitude-shuffled surrogates, plus the same analysis on dead-moss and no-moss recordings. If the complexity statistics look similar, the measure is describing the recording chain, not the organism.

**Interpretation does the work the experiment did not.** The slime-mould reality-check paper and Adamatzky's own "art of interpretation" remark are the honest version of this: a living substrate produces rich output, and computation is then read into it after the fact. The discipline that prevents this is committing in advance to what result would count as a failure — which input, which predicted output, which null.

**Under-specified protocols produce irreproducibility, then a dispute rather than a resolution.** The pea-plant exchange is the cleanest example: a replication fails, the original authors say the setup differed, and there is no agreed protocol to adjudicate. Publish electrode material and geometry, spacing, insertion depth, substrate, light spectrum and intensity, humidity, temperature logs, and raw traces, or the same thing happens.

**Terminology inflation invites the strongest available objection.** The memristor dispute shows what happens when a field attaches a high-status label to behaviour that has a duller explanation: the argument shifts from the data to the word. "Language", "neural network", "biocomputer" applied to moss will attract exactly that. Weaker, defensible claims — stimulus-specific electrical responses, a moisture-sensitive bioelectronic element — are more durable.

**Living substrates are slow, fragile and unrepeatable at the system level.** Even where slime-mould and fungal computing worked, the devices took hours to days, needed tight temperature, light and humidity windows, and varied between specimens. Moss adds desiccation cycles and seasonal variation on top. Budget for specimen-to-specimen variance being larger than your effect.

**The energy route has its own well-documented ceiling.** If the project pivots to moss as a power source rather than a signal source, the constraints are low power density, internal resistance, biofouling, and long-term durability — the reasons plant microbial fuel cells have stayed in the lab for two decades despite working demonstrations.

### Controls worth running before anything else

* Dead moss (autoclaved) and bare wet substrate, recorded in parallel with identical hardware.
* Continuous temperature, relative humidity, and substrate mass logging alongside every voltage channel.
* Ag/AgCl electrodes with agar bridges compared against the steel needles, on the same sample.
* A sham electrode pair inserted and left unstimulated, to quantify the wound response decay.
* Surrogate-data tests on every complexity statistic reported.
* At least three independent cushions, plus a repeat on the same cushion after a week, before any claim about signal classes.

## Gaps

* No study maps moss electrical activity systematically across the hydration cycle, despite moss being poikilohydric — the largest scientific hole and the largest engineering risk.
* No negative-control recordings published for moss: dead moss, autoclaved moss, or an inert wet substrate with the same electrodes.
* No deliberate stimulus-response study on a wild moss species; the light, cold, glutamate and ROS work is almost entirely intracellular work on *Physcomitrium patens*.
* No multi-electrode array or spatial mapping of moss activity beyond eight differential pairs.
* No moss impedance or dielectric characterization — nothing equivalent to the fungal memristance and moisture-response literature.
* No closed-loop moss system: nothing reads a moss signal and drives an actuator, as the mycelium-robot work does for fungi.
* No reservoir-computing benchmark on moss, so its computational capacity relative to a plain sensor array is unknown.
* The electrophysiology and microbial-fuel-cell literatures on moss barely cite each other; nobody has measured whether a moss carrying an anode still signals normally.

## Notes on scope

* The seed paper is a scoping study: one sample, one author, no environmental controls, no applied stimuli. Its "biocomputing substrate" framing is a forward-looking hypothesis, amplified further by popular coverage.
* Two unrelated things get called "electrical moss": ion-channel action potentials in the microvolt-to-millivolt range, and microbial fuel cell current driven by photosynthate-fed bacteria. Different physics, different electrodes, different literatures — be explicit about which one a system exploits.
* The "language" and "computation" claims in this line of work are contested in print. The rebuttal is listed above, and the same skepticism applies more strongly to moss, where there is far less data.
* Mechanistic knowledge comes from *Physcomitrium patens*, *Conocephalum conicum* and *Marchantia polymorpha*. The seed paper uses *Brachythecium rutabulum*, which has essentially no prior electrophysiology, so cross-species extrapolation is provisional.
* A few known-relevant works were left out rather than linked from memory — notably the Moss FM and Moss Table demonstrators, Proctor's 2007 bryophyte desiccation review, Fernando and Sojakka's "Pattern Recognition in a Bucket", and the in-materio reservoir computing series. All are findable by title.
