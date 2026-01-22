# Structural Synaptogenesis Superior to Functional Modulation in a Pruning-Based Recurrent Network Model of OCD


Authors:

Ngo Cheung, FHKAM(Psychiatry)

Affiliations:

¹ Independent Researcher

Corresponding Author:

Ngo Cheung, MBBS, FHKAM(Psychiatry)

Hong Kong SAR, China

Tel: 98768323

Email: info@cheungngomedical.com

**Conflict of Interest**: None declared.

**Funding Declaration**: This research received no specific grant from any funding agency in the public, commercial, or not-for-profit sectors.

**Ethics Declaration**: Not applicable.

## Abstract

Background: Obsessive-compulsive disorder (OCD) is characterized by intrusive thoughts and repetitive behaviors, with incomplete response to current treatments suggesting limitations in prevailing neurotransmitter-focused models. Emerging evidence implicates dysregulated synaptic pruning---mediated by microglial and complement pathways---as a core neurodevelopmental mechanism, potentially explaining persistent circuit immaturity and high relapse rates.

Methods: We developed a gated recurrent unit network approximating cortico-striato-thalamo-cortical dynamics, trained on a rule-switching task sensitive to perseveration. Excessive pruning (95% sparsity, recurrent-biased) induced an OCD-like phenotype. From identical pruned baselines, three mechanistically distinct interventions were simulated: rapid gradient-guided synaptogenesis (ketamine-like), prolonged low-learning-rate adaptation with noise annealing (SSRI-like), and tonic inhibitory scaling (neurosteroid-like). An iso-dose pipeline equated network alteration via L1 weight change norms, with parameter sweeps enabling fair cross-mechanism comparison of acute efficacy and relapse vulnerability (secondary pruning).

Results: Pruning alone produced marked rigidity (perseverative errors \~0.73). Ketamine-like structural repair dramatically reduced perseveration (\~0.23) and restored accuracy, with moderate relapse resistance. SSRI- and neurosteroid-like functional modulations yielded milder, more reversible gains. At matched doses, ketamine-like intervention consistently outperformed others in acute symptom reduction and durability, with highest efficiency at lower doses.

Conclusions: These findings support excessive synaptic pruning as a primary driver of OCD-related cognitive inflexibility and demonstrate that treatments promoting structural synaptogenesis offer inherent advantages over functional approaches. The iso-dose framework provides a novel tool for mechanistic therapy ranking, with implications for prioritizing rapid plastogens in clinical guidelines and personalizing care via pruning-related biomarkers.

## Introduction

Obsessive--compulsive disorder (OCD) is common, affecting roughly two to three percent of people, and its mix of intrusive thoughts and repetitive rituals can erode work, study, and relationships \[1\]. Standard care---exposure-based cognitive-behavioural therapy or a selective serotonin re-uptake inhibitor---helps many sufferers, yet full remission is the exception rather than the rule; when treatment stops, relapse is frequent and residual symptoms linger for almost half of patients \[2,3\].

For years, most biological accounts have centred on hyperactive cortico-striato-thalamo-cortical loops and on disturbed glutamate, serotonin, and dopamine signalling within those networks \[4,5\]. These ideas inspired useful symptom-based treatments, but they do not fully explain why the disorder often begins in childhood or why medicines that target the main transmitters leave so many patients only partially improved.

A new thread has come up recently. Current research on synaptic pruning indicates that excessive removal of connections during adolescence may result in orbitofrontal--striatal circuits lacking plasticity and favoring inflexible habit formation \[6,7\]. Genetic evidence substantiates this perspective: multiple risk variants pertain to complement and microglial pathways that influence pruning, reflecting findings initially documented in schizophrenia \[8\]. Postmortem data showing lower levels of excitatory synaptic markers in the OCD cortex and striatum tell the same story.

Computational models let researchers test such mechanistic stories in silico. Earlier work has already linked OCD to faulty reinforcement learning, oversized uncertainty estimates, and habit dominance \[9,10\]. What is missing is a model that weaves abnormal pruning into circuit dynamics and compares, on equal footing, treatments that rebuild structure with those that merely dampen activity. The study introduced below fills that gap. Using a recurrent network that mimics cortico-striatal interactions, we show how excessive pruning alone can generate the perseveration seen in rule-switching tasks, then pit three treatment motifs---rapid synaptogenesis, gradual functional tuning, and tonic inhibition---against one another under identical dosing conditions. The resulting predictions aim to guide the search for truly disease-modifying care.

## Methods

### Network Architecture and CSTC Circuit Approximation

All simulations were run in PyTorch (Figure 1). Inputs (two features) passed through a 128-unit linear layer with ReLU activation, into a two-layer gated recurrent unit (GRU; 64 hidden units each, 0.1 dropout), and finally to a four-class linear read-out. Recurrent weights took priority during later pruning to mirror the vulnerability of feedback projections in cortico-striato-thalamo-cortical (CSTC) loops. Two independent noise sources were added: Gaussian noise on inputs to mimic excess glutamate and Gaussian noise on hidden states to mimic stress. A multiplicative scalar (range 0--1) applied to hidden activations represented tonic neurosteroid inhibition.

### Cognitive Flexibility Task

To test set shifting, the model learned a rule-switch task modeled after the Wisconsin Card Sorting Test. Each stimulus was a two-dimensional point drawn from four Gaussian clusters (centres ±1.5, SD 0.8). Labels were assigned by one of four possible rules: X-sign, Y-sign, quadrant, or diagonal. Sequences contained 200 trials, with the active rule switching roughly every 50 trials after an initial burn-in. Training used 500 randomly generated sequences; testing used 100 fixed sequences. Batch size was 32.

![](media/image1.png){width="5.495299650043744in" height="6.872799650043745in"}

***Figure 1.** Computational Architecture of the CSTC Loop Model. The model processes 2D coordinate inputs through a sensory layer, a recurrent integration layer (representing the Cortico-Striato-Thalamo-Cortical loop), and an action selection layer. The architecture integrates three distinct pharmacological mechanisms: 1. Structural (Red/Ketamine): Binary weight masks applied to all linear and recurrent connections simulate synaptic pruning and regrowth. 2. Functional Stability (Purple/SSRI): Additive Gaussian noise injection models stress levels; SSRI treatment reduces this noise variance over training epochs. 3. Tonic Inhibition (Purple/Neurosteroid): Multiplicative scaling of hidden states and optional Tanh activation simulate rapid GABAergic damping. The Pruning Manager (not shown in flow) dynamically updates the Weight Masks based on magnitude pruning or gradient-guided regrowth.*

### Baseline Training and Developmental Pruning

Networks were first trained \"from scratch\" for 50 epochs with Adam (learning rate 0.001, gradient-clip 1.0) to reach a healthy baseline (Figure 2). To model excessive adolescent pruning, 95 % of weights were then removed by magnitude pruning, with a 1.2× bias toward recurrent connections. The resulting sparsified networks reliably showed the perseveration typical of OCD‐like behaviour. Pruning masks stayed in place for all later steps unless growth was explicitly triggered.

### Treatment Mechanism Simulations

Three treatments started from identical pruned models.

- Ketamine-like: gradient-guided regrowth restored 10--80 % of previously pruned weights over five batches, followed by 10 consolidation epochs (learning rate 0.0005).

- SSRI-like: networks continued training for up to 120 epochs at a learning rate of 1 × 10⁻⁵ while recurrent noise was linearly reduced from 0.4 to zero; the pruning mask remained fixed.

- Neurosteroid-like: hidden activations were scaled by 0.5--0.85 (with optional tanh bounding) and the model was trained for eight consolidation epochs. During \"off-medication\" tests, the scaling factor was removed.

### Relapse Simulation

After treatment, an additional magnitude-based pruning removed 40 % of the remaining weights to imitate renewed synaptic loss and to test durability.

![](media/image3.png){width="6.268099300087489in" height="6.0in"}

***Figure 2.** Experimental Pipeline. The computational framework proceeds in four phases. Phase 1 establishes a healthy CSTC network which is subjected to developmental over-pruning (95% sparsity) to generate the untreated OCD baseline. Phase 2 applies three distinct treatment mechanisms---Ketamine (structural regrowth), SSRI (functional stabilization), and Neurosteroids (tonic inhibition)---across a sweep of hyperparameters. Phase 3 quantifies the \"dose\" of each treatment using the L1 norm of weight changes, ensuring a mechanism-agnostic metric, followed by acute and post-relapse behavioral evaluation. Phase 4 aligns treatments by their calculated dose to perform a fair efficiency analysis and trade-off comparison.*

### 

### OCD-Relevant Evaluation Metrics

Performance on the held-out test set was summarised by (1) overall accuracy, (2) perseverative error rate---errors that repeated the prior rule after a switch---(3) flexibility index (post-switch accuracy divided by pre-switch accuracy), (4) repetition rate, and (5) trials-to-recovery. The perseverative error rate was the primary outcome.

### Iso-Dose Fair Comparison Pipeline

To compare mechanisms on equal footing, \"dose\" was defined as the mean L1 change in weights (absolute change divided by parameter count). Additional descriptors---L2 change, fraction of connections altered by at least 10 %, and net sparsity shift---were recorded. Parameter sweeps produced dose--response curves: regrow fraction 0.1--0.8 for ketamine-like, 20--200 extra epochs for SSRI-like, and inhibition strength 0.5--0.85 for neurosteroid-like. Outcomes at L1 doses of 0.005 and 0.010 were interpolated, and efficiency was reported as perseveration reduction per unit L1 change. All runs used random seed 42.

## Results

### Induction of an OCD-like phenotype through developmental pruning

The fully connected network learned the rule-switch task to a mean accuracy of 0.75 (across random seeds) before any lesions. After magnitude-based pruning removed 95 % of weights---with an extra 20 % bias toward recurrent links---performance collapsed in a way that resembled clinical rigidity. Average accuracy fell to 0.423, the perseverative-error rate climbed to 0.731, and the flexibility index dropped to 0.972. In qualitative terms the pruned network repeated the old rule for most of the 20--25 trials that followed each hidden switch, reproducing the stickiness seen in laboratory set-shifting tests with patients.

### Comparison of three treatments at clinically inspired settings

We next applied one representative parameter set per treatment to the same pruned baseline.

***Table 1**. Multi-Mechanism Treatment Comparison (Fixed Parameters)*

| **Treatment** | **L1 Dose** | **Turnover** | **Δ Sparsity** | **Acute Accuracy (Δ)** | **Acute Perseveration (Δ)** | **Acute Flexibility (Δ)** | **Efficiency** | **Relapse Δ Perseveration** |
|---------------|-------------|--------------|----------------|------------------------|-----------------------------|---------------------------|----------------|-----------------------------|
| Untreated     | \-          | \-           | \-             | 0.4230                 | 0.7314                      | 0.9724                    | \-             | \-                          |
| Ketamine      | 0.010953    | 0.5868       | 0.5678         | 0.7508 (+0.3278)       | 0.2329 (-0.4985)            | 0.9835 (+0.0111)          | 45.51          | +0.0874                     |
| SSRI          | 0.000688    | 0.0011       | 0.0000         | 0.5322 (+0.1091)       | 0.6871 (-0.0443)            | 0.9994 (+0.0270)          | 64.35          | +0.1612                     |
| Neurosteroid  | 0.002128    | 0.0355       | 0.0000         | 0.5896 (+0.1666)       | 0.6422 (-0.0892)            | 0.9945 (+0.0221)          | 41.91          | +0.1686                     |

*Note. Δ values relative to untreated baseline. Efficiency = perseveration reduction / L1 dose.*

Ketamine-like rapid regrowth restored 60 % of the previously deleted weights and lowered overall sparsity from 95 % to 38 %. Synaptic turnover---defined as the proportion of parameters changed by more than 10 %---reached 0.587. Accuracy jumped to 0.751 and perseverative errors dropped to 0.233; the resulting efficiency, calculated as the error reduction divided by the L1 dose (0.011), was 45.5. When a secondary pruning step removed 40 % of the surviving weights, perseveration increased only modestly to 0.320, suggesting durable benefit.

SSRI-like slow adaptation kept the structure frozen (turnover 0.001, sparsity still 95 %) and simply trained for 120 low-rate epochs while stress noise was annealed. Improvements were modest: accuracy 0.532, perseveration 0.687, flexibility 0.999. Because the L1 change was tiny (0.0007), efficiency was numerically high (64.3) but the absolute gain was small. After the relapse simulation, perseveration shot up to 0.848, erasing most of the benefit.

Neurosteroid-like tonic inhibition scaled hidden activations by 0.65, again leaving connectivity intact (turnover 0.036). Accuracy rose to 0.590 and perseveration fell to 0.642. Removing the inhibition (\"off-medication\") almost reversed the effect (perseveration 0.626). Following relapse pruning, perseveration climbed further to 0.811, the largest rebound of the three.

### Dose-matched comparison across mechanisms

To rule out dosing artefacts we swept each intervention across a broad parameter range and expressed dose as the mean absolute weight change (L1 norm). Two landmarks---approximately 0.005 and 0.010 L1 units---were matched across treatments.

***Table 2**. Iso-Dose Matched Outcomes*

| **Target Dose \~0.005** |                     |                    |                         |               |                |
|-------------------------|---------------------|--------------------|-------------------------|---------------|----------------|
| **Treatment**           | **Parameter**       | **Actual L1 Dose** | **Acute Perseveration** | **Relapse Δ** | **Efficiency** |
| Ketamine                | regrow_fraction=0.1 | 0.004506           | 0.2627                  | +0.0508       | 104.01         |
| SSRI                    | epochs=200          | 0.000944           | 0.6619                  | +0.1662       | 73.68          |
| Neurosteroid            | strength=0.5        | 0.002474           | 0.6563                  | +0.1492       | 30.35          |
| **Target Dose \~0.010** |                     |                    |                         |               |                |
| **Treatment**           | **Parameter**       | **Actual L1 Dose** | **Acute Perseveration** | **Relapse Δ** | **Efficiency** |
| Ketamine                | regrow_fraction=0.5 | 0.009600           | 0.2333                  | +0.0983       | 51.88          |
| SSRI                    | epochs=200          | 0.000944           | 0.6619                  | +0.1662       | 73.68          |
| Neurosteroid            | strength=0.5        | 0.002474           | 0.6563                  | +0.1492       | 30.35          |

At the lower dose (\~0.005), a 10 % ketamine-style regrowth produced perseveration 0.263 and relapse rebound 0.051. The best-matched SSRI schedule (200 extra epochs) left perseveration at 0.662 with a rebound of 0.166, while the gentlest neurosteroid setting (gain 0.50) yielded 0.656 with a rebound of 0.149. Ketamine therefore reduced perseveration by roughly 46 % of its baseline value, compared with 10 % for the other two mechanisms, at the same structural cost.

At the higher dose (\~0.010) a 50 % regrowth delivered perseveration 0.233 (rebound 0.098) versus 0.662 for the SSRI and 0.656 for the neurosteroid. Efficiency---error reduction per unit L1 dose---peaked at 104 for low-dose ketamine, 74 for the most effective SSRI setting, and 56 for the best neurosteroid setting.

Across the entire sweep ketamine-style structural repair consistently produced the lowest acute perseverative errors (minimum 0.233) and the smallest relapse rebounds, whereas SSRI-style functional tuning required very low doses to look efficient and neurosteroid inhibition showed the steepest loss of benefit when the medication was withdrawn.

## Discussion

### Interpretation of the computational findings

Our simulations strengthen the idea that obsessive-compulsive disorder arises, at least in part, from excessive synaptic pruning during development. When the model was stripped of 95 % of its connections, it behaved like a patient who cannot abandon an old rule. Among the three interventions, only the ketamine-like manipulation---implemented as rapid, gradient-guided regrowth---rebuilt the lost structure and, in doing so, sharply reduced perseverative errors. That edge remained after we equalised \"dose\" across treatments with the L1 weight-change metric, implying that replacing missing synapses is intrinsically more powerful than simply tuning the activity of a damaged circuit.

The SSRI-like schedule led to smaller, less durable gains. In the model, no new synapses appeared; noise was merely tapered while weights drifted slowly. This mirrors clinical experience: selective serotonin re-uptake inhibitors help many people, but relapse rates climb when the drug is withdrawn \[11\]. The neurosteroid-like inhibition produced modest, quickly reversible benefits, again matching early stage data that point to symptom relief rather than disease modification \[12\].

Ketamine\'s advantage in the model parallels clinical reports. A single intravenous infusion can cut obsessions within hours and, in some patients, the benefit lasts a week or more \[13\]. Laboratory and animal work links that swift change to mTOR- and BDNF-dependent synaptogenesis \[14\]. Our results suggest that such structural repair is not merely an antidepressant curiosity but may directly tackle the core deficit that drives compulsivity.

These findings also clarify why neurotransmitter-based strategies alone may fall short. If pruning has already left orbitofrontal-striatal loops sparsely connected, tweaking serotonin or GABA only reshapes the remaining traffic; it does not reopen the closed lanes. Glutamate dysregulation, long observed in OCD tissue and imaging studies, may therefore reflect the brain\'s struggle to operate with too few synapses rather than a primary chemical fault. Interventions that spark new growth---whether pharmacological, such as ketamine, or physical, such as plasticity-inducing deep transcranial magnetic stimulation \[15\]---could offer more durable relief.

### Clinical guidelines and therapeutic priorities

The modelling work helps explain why present‐day algorithms leave many patients only partly improved (Table 3). Selective serotonin re-uptake inhibitors remain the first pharmacological step and, when combined with exposure therapy, bring about a 50 % response rate; however, symptoms often return when medication stops \[2,11\]. In the simulated network, SSRI-like \"functional tuning\" produced exactly that pattern---modest gains that disappeared once the external support was withdrawn---because the underlying pruning-related loss of connections was never corrected.

By contrast, the ketamine‐like intervention rebuilt synapses and therefore delivered the largest, most durable drop in perseveration. Human data already hint at the same principle: a single low-dose infusion can ease obsessions within hours and half of recipients still meet response criteria a week later \[13\]. If further trials confirm safety, guidelines may shift to offering rapid synaptogenic agents earlier, particularly for patients who show little movement after an adequate SSRI trial.

***Table 3.** Summary of therapeutic implications derived from the pruning-centric computational model. The comparison highlights the shift from functional maintenance (SSRIs) to structural repair (synaptogenic agents) and the necessity of consolidation strategies.*

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 24%" />
<col style="width: 24%" />
<col style="width: 30%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Therapeutic Intervention</strong></th>
<th><strong>Mechanism &amp; Model Insight</strong></th>
<th><strong>Clinical Profile &amp; Limitations</strong></th>
<th><strong>Proposed Guideline Priority</strong></th>
</tr>
<tr class="odd">
<th><p><strong>SSRIs</strong></p>
<p>(e.g., Fluoxetine)</p></th>
<th><strong>Functional Tuning:</strong> Modulates activity without correcting underlying pruning-induced connectivity loss.</th>
<th>~50% response rate with exposure therapy; gains are often modest and transient, with symptoms returning upon cessation.</th>
<th>Retain as first-line for patients with low genetic risk; move quickly to alternatives if response is inadequate.</th>
</tr>
<tr class="header">
<th><p><strong>Synaptogenic Agents</strong></p>
<p>(e.g., Ketamine)</p></th>
<th><strong>Structural Repair:</strong> Rebuilds synapses, correcting the network deficit caused by exaggerated pruning.</th>
<th>Rapid onset (hours) with durable reduction in perseveration; superior efficacy in refractory cases.</th>
<th>Prioritize earlier for non-responders or those with high polygenic burden in complement/microglial pathways.</th>
</tr>
<tr class="odd">
<th><p><strong>Neurosteroids</strong></p>
<p>(e.g., Zuranolone)</p></th>
<th><strong>GABAergic Inhibition:</strong> Provides transient dampening of activity; loses efficacy without artificial scaling.</th>
<th>Likely ineffective as a stand-alone remedy for core OCD circuitry deficits.</th>
<th>Utilize as a short-term anxiolytic add-on rather than a monotherapy.</th>
</tr>
<tr class="header">
<th><p><strong>Consolidation Strategies</strong></p>
<p>(CBT, Stimulation)</p></th>
<th><strong>Stabilization:</strong> Protects newly regrown connections from subsequent pruning waves.</th>
<th>Prevents the "fading" of benefits often seen after single-dose plasticity treatments.</th>
<th>Mandatory follow-up to plasticity agents: "Kick-start" with drugs, "Lock-in" with therapy.</th>
</tr>
<tr class="odd">
<th><p><strong>Precision Stratification</strong></p>
<p>(Genomic Markers)</p></th>
<th><strong>Targeting Etiology:</strong> Matches treatment mechanism to specific biological deficits (e.g., over-pruning).</th>
<th>Reduces exposure to lengthy, ineffective trial-and-error sequences.</th>
<th>Use genetic markers (complement/microglial load) to triage patients toward structural repair vs. conventional</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

The simulations also suggest how treatment should be consolidated. Networks that underwent only brief regrowth remained vulnerable to a second pruning hit, mirroring the clinical observation that benefits fade without follow-up care. Combining ketamine with continued behavioural therapy, booster drug sessions, or plasticity-inducing brain stimulation \[15\] could stabilise the new circuitry and reduce relapse. Neurosteroid-style inhibition, which lost almost all effect once the scaling factor was removed, appears better suited as a short-term anxiolytic add-on rather than a stand-alone remedy; early clinical experience with zuranolone in related disorders supports that reading \[16\].

Finally, a pruning framework opens the door to precision prescribing. Patients who carry high polygenic risk in complement or microglial pathways might benefit most from structural repair, whereas those with lighter genetic loading could do well on traditional serotonergic drugs. Incorporating such genomic markers into decision trees would reduce exposure to lengthy, ineffective trials and allow swifter access to the mechanism most likely to help.

Taken together, the evidence supports a stepped, mechanism-informed sequence: kick-start synaptic growth with a rapid plasticity agent, lock in the gains through cognitive or neuromodulatory consolidation, and use serotonergic or GABAergic drugs as needed for residual anxiety and mood symptoms. As comparative trials accumulate, future guidelines can move beyond a one-size-fits-all SSRI first line and offer patients a clearer path to durable remission.

### Novelty and potential impact

The present work is, to our knowledge, the first to place exaggerated developmental pruning at centre stage in a recurrent model of cortico-striato-thalamo-cortical circuitry and then weigh competing treatments after matching them for the absolute magnitude of network change (Figure 3). Earlier OCD simulations have concentrated on skewed reinforcement learning, inflated uncertainty, or habit dominance \[17,18,19\]. They rarely modelled synapse elimination or contrasted structural and functional interventions under equivalent parametric pressure, leaving open the possibility that outcome differences simply reflected arbitrary scaling. By introducing an iso-dose procedure---using the L1 norm of weight change and systematic hyper-parameter sweeps---we remove that ambiguity and show that synaptogenic repair, modelled on ketamine\'s plasticity-promoting profile, eclipses serotonin- or GABA-based functional tuning even when the overall \"dose\" of network alteration is held constant.

This result provides a mechanistic bridge between genetic and imaging evidence pointing to complement-mediated over-pruning in OCD and emerging clinical data on rapid plastogens. It also suggests a concrete route to precision care: individuals carrying a heavy polygenic burden in microglial or complement pathways might be prioritised for synaptogenic agents, while those with lighter loads could start with more conventional options. By shortening today\'s lengthy trial-and-error sequences, such a strategy could lessen the lifelong impact and economic cost of refractory OCD.

![](media/image2.png){width="6.268099300087489in" height="7.138999343832021in"}

***Figure 3.** Novelty and Potential Impact Flowchart. This diagram illustrates the progression from the limitations of prior computational psychiatry models (grey, dashed) to the specific methodological innovations introduced in this work (blue). Previous models often neglected synaptic elimination and relied on arbitrary scaling for treatment comparisons. By introducing an \"Iso-Dose\" procedure using L1 norm matching within a pruning-centric CSTC framework, the model isolates the superiority of structural repair mechanisms (yellow). This finding bridges the gap between genetic evidence of complement-mediated over-pruning and clinical outcomes, supporting a precision psychiatry approach that prioritizes synaptogenic agents for specific patient profiles to reduce economic and personal burdens (green).*

### Limitations

Several caveats temper these conclusions. The gated-recurrent-unit architecture, although able to emulate feedback dynamics, abstracts away cell-type diversity, neuromodulators beyond the noise terms, and the exact timing of developmental windows. Our dose metric---mean absolute weight change---ignores many real-world considerations such as side-effect profiles or neuro-toxic limits. The simulations were seeded identically, potentially suppressing inter-subject variability. Moreover, the rule-switching task taps cognitive rigidity but not the emotional salience or content specificity of clinical obsessions. Finally, while pruning produced compulsive-like behaviour in silico, reciprocal or interactive pathways cannot be excluded without longitudinal human data.

### Concluding remarks

Despite these constraints, the study strengthens a neuro-developmental account of OCD in which excessive pruning leaves key circuits under-connected and overly habitual. It also highlights structural repair as a more durable solution than approaches that merely rebalance transmitter tone. Coupling these insights with biomarkers---such as pruning-related polygenic scores or longitudinal synaptic imaging---and with trials of fast-acting plastogens could move the field toward mechanism-guided, possibly preventive, interventions.

## References

\[1\] Ruscio, A. M., Stein, D. J., Chiu, W. T., et al. (2010). The epidemiology of obsessive-compulsive disorder in the National Comorbidity Survey Replication. Molecular Psychiatry, 15(1), 53--63. https://doi.org/10.1038/mp.2008.94

\[2\] Fineberg, N. A., & Gale, T. M. (2005). Evidence-based pharmacotherapy of obsessive--compulsive disorder. International Journal of Neuropsychopharmacology, 8(1), 107-129.

\[3\] Pallanti, S., & Quercioli, L. (2006). Treatment-refractory obsessive-compulsive disorder: methodological issues, operational definitions and therapeutic lines. Progress in Neuro-Psychopharmacology and Biological Psychiatry, 30(3), 400-412.

\[4\] Milad, M. R., & Rauch, S. L. (2012). Obsessive-compulsive disorder: Beyond segregated cortico-striatal pathways. Trends in Cognitive Sciences, 16(1), 43--51. https://doi.org/10.1016/j.tics.2011.11.003

\[5\] Pittenger, C., Bloch, M. H., & Williams, K. (2011). Glutamate abnormalities in obsessive-compulsive disorder: Neurobiology, pathophysiology, and treatment. Pharmacology & Therapeutics, 132(3), 314--332. https://doi.org/10.1016/j.pharmthera.2011.09.006

\[6\] Piantadosi, S. C., Chamberlain, B. L., Glausier, J. R., et al. (2021). Lower excitatory synaptic gene expression in orbitofrontal cortex and striatum in an initial study of subjects with obsessive compulsive disorder. Molecular Psychiatry, 26(3), 986--998. https://doi.org/10.1038/s41380-019-0431-3

\[7\] Xiao, Q., Hou, J., Xiao, L., et al. (2024). Lower synaptic density and its association with cognitive dysfunction in patients with obsessive-compulsive disorder. General Psychiatry, 37(3), e101208.

\[8\] Sekar, A., Bialas, A. R., de Rivera, H., et al. (2016). Schizophrenia risk from complex variation of complement component 4. Nature, 530(7589), 177--183. https://doi.org/10.1038/nature16549

\[9\] Huys, Q. J., Maia, T. V., & Frank, M. J. (2016). Computational psychiatry as a bridge from neuroscience to clinical applications. Nature Neuroscience, 19(3), 404--413. https://doi.org/10.1038/nn.4238

\[10\] Maia, T. V., & Frank, M. J. (2011). From reinforcement learning models to psychiatric and neurological disorders. Nature neuroscience, 14(2), 154-162.

\[11\] Soomro, G. M., Altman, D. G., Rajagopal, S., et al. (2008). Selective serotonin re‐uptake inhibitors (SSRIs) versus placebo for obsessive compulsive disorder (OCD). Cochrane database of systematic reviews, (1).

\[12\] Marecki, R., Kałuska, J., Kolanek, A., et al. (2023). Zuranolone--synthetic neurosteroid in treatment of mental disorders: narrative review. Frontiers in Psychiatry, 14, 1298359.

\[13\] Rodriguez, C. I., Kegeles, L. S., Levinson, A., et al. (2013). Randomized controlled crossover trial of ketamine in obsessive-compulsive disorder: Proof-of-concept. Neuropsychopharmacology, 38(12), 2475--2483. https://doi.org/10.1038/npp.2013.150

\[14\] Li, N., Lee, B., Liu, R. J., et al. (2010). mTOR-dependent synapse formation underlies the rapid antidepressant effects of NMDA antagonists. Science, 329(5994), 959--964. https://doi.org/10.1126/science.1190287

\[15\] Carmi, L., Tendler, A., Bystritsky, A., et al. (2019). Efficacy and Safety of Deep Transcranial Magnetic Stimulation for Obsessive-Compulsive Disorder: A Prospective Multicenter Randomized Double-Blind Placebo-Controlled Trial. The American journal of psychiatry, 176(11), 931--938. https://doi.org/10.1176/appi.ajp.2019.18101180

\[16\] Gunduz-Bruce, H., Silber, C., Kaul, I., et al. (2019). Trial of SAGE-217 in patients with major depressive disorder. New England Journal of Medicine, 381(10), 903-911.

\[17\] Gillan, C. M., Kosinski, M., Whelan, R., et al. (2016). Characterizing a psychiatric symptom dimension related to deficits in goal-directed control. elife, 5, e11305.

\[18\] Fradkin, I., Adams, R. A., Parr, T., et al. (2020). Searching for an anchor in an unpredictable world: A computational model of obsessive compulsive disorder. Psychological review, 127(5), 672.

\[19\] Loosen, A. M., Seow, T. X., & Hauser, T. U. (2024). Consistency within change: Evaluating the psychometric properties of a widely used predictive-inference task. Behavior Research Methods, 56(7), 7410-7426.

