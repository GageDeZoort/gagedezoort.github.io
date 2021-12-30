---
title: 'Correcting the Di-Tau Mass Spectrum'
date: 2021-12-19 20:56:00
featured_image: '/images/demo/demo-square.jpg'
excerpt: Higgs boson decays to two tau leptons (a di-tau system) are critical to understanding how the Higgs boson interacts with fermionic matter. However, taus are difficult to measure because they produce neutrinos, which are invisible to particle detectors at the Large Hadron Collider, when they decay. This post focuses on how detector measurements and tau decay physics can be used to infer the hidden properties of a di-tau system.
katex: true
---

## Table of Contents
 1. [Introduction](https://gagedezoort.github.io/blog/svfit-fastmtt#Introduction)
 2. [Tau Decay Physics](https://gagedezoort.github.io/blog/svfit-fastmtt#Tau-Decay-Physics)
 3. [Di-Tau Systems](https://gagedezoort.github.io/blog/svfit-fastmtt#Di-Tau-Systems)
 4. [MET Constraints on Invisible Systems](https://gagedezoort.github.io/blog/svfit-fastmtt#MET-Constraints-on-Invisible-Systems)
 5. [Correcting the Di-Tau Mass Spectrum](https://gagedezoort.github.io/blog/svfit-fastmtt#Correcting-the-Di-Tau-Mass-Spectrum)
## Introduction
Taus are challenging particles to measure at the Large Hadron Collider (LHC) because their decays always produce neutrinos, which are invisible to LHC detectors. For this reason, taus are typically "undermeasured" at the LHC, in the sense that we will never measure the full set of particles produced when a tau decays. Rather, we only have access to the particles "visible" to our detectors - for example, electrons, photons, and hadrons - that give us a partial picture of the taus that produced them. It's worth addressing this challenge, though, because taus give us insight into many interesting physics processes, like the decay of the Standard Model Higgs boson to two taus ($$H\rightarrow\tau\tau$$). These decays are among the most important to study Higgs couplings to fermions because the ratio $$\Gamma(H\rightarrow\tau\tau)/\Gamma(H\rightarrow\mu\mu)\approx288$$ guarentees an abundence of $$H\rightarrow\tau\tau$$ events with respect to other leptons and $$H\rightarrow\tau\tau$$ signals are easier to separate from background than $$H\rightarrow b\bar{b}$$ signals, despite the larger branching ratio of $$H\rightarrow b\bar{b}$$ (see Table 1). 

| Decay | Branching Ratio ($\Gamma$) | Rel. Uncertainty |
| :-: | :-: | :-: |
| $H\rightarrow b\bar{b}$       | $0.582$$          | $^{+1.2\%}_{-1.3\%}$  | 
| $H\rightarrow \tau^+\tau^-$   | $0.0627$          | $\pm1.6\%$            |
| $H\rightarrow c\bar{c}$       | $0.0289$          | $^{5.5\%}_{-2.0\%}$   |
| $H\rightarrow \mu^+\mu^-$     | $0.000218$        | $\pm1.7\%$            | 
**Table 1**: The relative decay probabilities (branching ratios) of the most common Higgs boson decays to fermions, reproduced from the PDG Higgs Status Report [^1]. 

Di-tau systems, or systems of two taus produced by some upstream parent particle like the Higgs, are underconstrained by detector measurements because of the neutrinos produced in tau decays. In practice, this means that if we looked at the mass spectrum of the di-taus measured in the detector, it would undershoot its true value. Several algorithms have been developed to leverage detector measurements and knowledge of tau decay physics to correct this effect. This post focuses on two such algorithms common to analyses involving $$H\rightarrow\tau\tau$$ decays.

## Tau Decay Physics
Taus decay via the weak interaction as $$\tau^\pm\rightarrow W^\pm\nu_{\tau}$$. The $$W^\pm$$ is produced off-shell (note that $$m_\tau=1.77$$ GeV and $$m_W=80.4$$ GeV), decaying leptonically as $$W^\pm\rightarrow e^\pm\nu_e$$ or $$W^\pm\rightarrow\mu^\pm\nu_{\mu}$$, or hadronically as $$W^\pm\rightarrow q\bar{q}$$. Interestingly, taus are the only leptons heavy enough to produce hadronic showers. The set of tau decays and their corresponding branching fractions are listed in Table 2. 

<p>
| Decay Mode                                     | Label    | Branching Ratio ($$\Gamma$$)  |
| ---------------------------------------------- | -------- | --------------------------- |
| $$\tau^\pm\rightarrow e^\pm\nu_e\nu_\tau$$     | Leptonic | $$0.1782$$                  |
| $$\tau^\pm\rightarrow \mu^\pm\nu_\mu\nu_\tau$$ | Leptonic | $$0.1739$$                  |
| $$\tau^\pm\rightarrow \tau_h\nu_\tau$$         | Hadronic | $$0.6479$$                  |
**Table 2**: The decay modes of a single tau lepton and their probabilities as reported in the PDG tau properties table [^2]. The visible component of hadronic tau decays is usually called $$\tau_h$$, which accounts for varying cominations of charged and neutral hadrons (usually pions or kaons). It is also common to see the visible components of tau decays, electrons and muons, written as $$\tau_e$$ and $$\tau_\mu$$ respectively. 
</p>

It's important to notice that *tau decays always produce neutrinos*, which are invisible to detectors like CMS and ATLAS. For this reason, tau four-vectors can be written as the sum of a visible system of electrons, muons, or hadrons, and an invisible system of neutrinos:

<p>
\begin{align}
    p_\tau &= (E_\tau, \vec{p}_\tau) = p_\mathrm{vis} + p_\mathrm{inv}\\
           &= (E_\mathrm{vis} + E_\mathrm{inv}, \vec{p}_\mathrm{vis} + \vec{p}_\mathrm{inv})
\end{align}
</p>

Detector measurements give us $p_\mathrm{vis}$ but do not provide enough information to fully reconstruct $p_\mathrm{inv}$. Defining the ratio of the visible energy to the original tau energy as $x=E_\mathrm{vis}/E_\tau$, we can parametrize the invisible system with three additional quantities: 
- $m_{\nu\nu}$, the mass of the neutrino system; note that $m_{\nu\nu}=0$ for hadronic tau decays as they produce only a single neutrino
- $\theta_{GJ}$, the Gottfried-Jackson angle, describing the opening angle between $\vec{p}_\mathrm{vis}$ and $\vec{p}_\tau$
- $\phi$, the angle of $\vec{p}_\mathrm{vis}$ around a cone centered on $\vec{p}_\tau$ and with opening angle $\theta_{GJ}$

A quick calculation shows that $\theta_{GJ}$ is redundent, as it can be calculated from visible quantities, $x$ and $m_{\nu\nu}$ (see [^3]). That said, in addition to four measured quantities ($E_\mathrm{vis}, \vec{p}_\mathrm{vis}$), leptonic tau decays are specified by three additional parameters ($x$, $m_{\nu\nu}$, and $\phi$) and hadronic tau decays by two ($x$ and $\phi$.) Finally, note that the *visible mass* in a given tau decay is the mass of the lepton or hadronic shower appearing in the detector:

$$ m_\mathrm{vis}^2 = E_\mathrm{vis}^2 - \vert\vec{p}_\mathrm{vis}\vert^2$$ 

## Di-Tau Systems
A system of two taus is usually called a *di-tau* ($\tau_1\tau_2$) system.  Di-tau kinematics are determined by the individual decays of each tau; the full set of possibilities is listed in Table 3. In many cases, analyses target di-tau final states to measure a parent particle producing the taus, for example the Higgs boson. The parent would appear as a peak in the distribution of di-tau invariant mass, given by
\begin{align}
m_{\tau_1\tau_2}^2 = (p_{\tau_1}+ p_{\tau_2})^2
\end{align}
To measure $m_{\tau_1\tau_2}$, we need to reconstruct each tau’s four-vector:
\begin{align}
    p_{\tau_1} &= p_\mathrm{vis}^{(1)} + p_\mathrm{inv}^{(1)}\\
    p_{\tau_2} &= p_\mathrm{vis}^{(2)} + p_\mathrm{inv}^{(2)}
\end{align}
Since the visible quantities $p_\mathrm{vis}^{(1)}=(E_\mathrm{vis}^{(1)},\vec{p}_\mathrm{vis}^{(1)})$ and $p_\mathrm{vis}^{(2)} =(E_\mathrm{vis}^{(2)},\vec{p}_\mathrm{vis}^{(2)})$ are measured in the detector, the only remaining task is to parametrize the invisible system generated by the tau decay neutrinos. To do so, let's define a vector $\vec{a}$ containing the set of undetermined tau decay quantities and a vector $\vec{y}=( E_\mathrm{vis}^{(1)},\vec{p}_\mathrm{vis}^{(1)}, E_\mathrm{vis}^{(2)},\vec{p}_\mathrm{vis}^{(2)})$ containing the visible di-tau decay products. The components of $\vec{a}$ are shown for each di-tau decay mode in Table 3.

| Decay Mode ($l=e,\mu$) | Label | Branching Ratio ($\Gamma$) | Unmeasured Params | 
|------------------------|-------|----------------------------|-------------------|
| $\tau_1\tau_2\rightarrow (l_1\nu_{l_1}\nu_\tau)(l_2\nu_{l_2}\nu_\tau)$ | Fully-Leptonic | $0.032\ \ (l_1,l_2=e)$<br />$0.030\ \ (l_1, l_2=\mu)$<br />$0.062\ \ (l_1=e,\mu,\  l_2=\mu,e)$ | $(x^{(1)},m_{\nu\nu}^{(1)},\phi^{(1)},$ <br /> $\ x^{(2)},m_{\nu\nu}^{(2)},\phi^{(2)})$
| $\tau_1\tau_2\rightarrow (l\nu_l\nu_\tau)(\tau_h\nu_\tau)$             | Semi-Leptonic  | $0.231\ (l=e)$<br />$0.225\ \ (l=\mu)$   | $(x^{(1)},m_{\nu\nu}^{(1)},\phi^{(1)},$ <br /> $\ x^{(2)},\phi^{(2)})$                                                      |
| $\tau_1\tau_2\rightarrow (\tau_h\nu_\tau)(\tau_h\nu_\tau)$             | Fully-Hadronic | $0.420$  | $(x^{(1)},\phi^{(1)},$ <br /> $\ x^{(2)},\phi^{(2)})$ | 
**Table 3**: The di-tau decay branching ratios are calculated directly from the values in Table 2 by multiplying independent probabilities that each tau decays leptonically or hadronically. 

Unfortunately, we do not measure the components of $\vec{a}$ directly in LHC detectors. Therefore, the visible di-tau mass typically undershoots its true value:

$$m_{\tau_1\tau_2}^\mathrm{vis} = \sqrt{(p_\mathrm{vis}^{(1)} + p_\mathrm{vis}^{(2)})\cdot(p_\mathrm{vis}^{(1)} + p_\mathrm{vis}^{(2)})} < m_{\tau_1\tau_2}$$

## MET Constraints on Invisible Systems
All is not lost - because momentum is conserved in the transverse plane in collider events, the total transverse energy imbalance in an event 

\begin{align}
\vec{E}_T^\mathrm{miss} &= (E_x^\mathrm{miss}, E_y^\mathrm{miss}) = -\vec{E}_T^\mathrm{meas} = -\sum_{i=1}^{n_\mathrm{meas}} \vec{E}_T^{(i)} 
\end{align}

gives some indication of the invisible kinematics at play.  Here $i$ indexes every measured object in an event (for these purposes, particles reconstructed by the Particle Flow algorithm), and $\vec{E}_T^\mathrm{miss}$ is known as "missing $E_T$", or MET. Usually, we measure $\vert\vec{E}_T^\mathrm{miss}\vert$ and the polar angle of the MET, $\phi_{MET}$, to reconstruct the $x$ and $y$ MET components: 

\begin{align}
    E_x^\mathrm{miss} &= \vert\vec{E}_T^\mathrm{miss}\vert\cos{\phi_{E_T^\mathrm{miss}}}\\
    E_y^\mathrm{miss} &= \vert\vec{E}_T^\mathrm{miss}\vert\sin{\phi_{E_T^\mathrm{miss}}}\\
\end{align}

If MET is non-zero, then an event's transverse momentum has not been conserved because of either 1) mismeasurement or 2) invisible particles escaping the detector unmeasured. We can ask: how significant are these MET constraints? How can we know if we're looking at a real system of invisible particles or just noisy detector responses? Let's assume we've measured $n_\mathrm{meas}$ objects in the detector, each of which has a transverse energy vector $\vec{E}^{(i)}_T$. These measurements are imperfect because detectors have a finite resolution; let's call the true value of each measured object's transverse energy $\vec{e}^{(i)}_T$. The residual between these quantities, $\vec{\epsilon}_i=\vec{E}^{(i)}_T-\vec{e}^{(i)}_T$, is usually taken to be a 2D Gaussian with mean 0 and covariance matrix $V_i$:
\begin{align}
    p(\vec{\epsilon}_i \vert\vec{e}_T^{(i)}) \propto \exp\big(-\frac{1}{2}\vec{\epsilon}_i^TV_i^{-1}\vec{\epsilon}_i \big)
\end{align}

The Gaussian form of $\vec{\epsilon}_i$ only accounts for detector resolution effects and the covariance matrix $V_i$ is calculated from detailed studies of detector response vs. $p_T$ and $\eta$ against each particle type (see [^5]). We can generalize to an entire event by writing the total residual as a sum over all measured particles:
\begin{align}
\vec{\epsilon}_\mathrm{event} &= \sum_{i=1}^{n_\mathrm{meas}} \vec{\epsilon}^{(i)}\\
&= \sum_{i=1}^{n_\mathrm{meas}} \big(\vec{E}_T^{(i)} - \vec{e}_T^{(i)} \big)\\
&= \vec{E}_T^\mathrm{meas} - \vec{e}_T
\end{align}
With Gaussian probability models for each measured particle, the MET significance is defined as the log-likelihood ratio testing an event's consistency with the null hypothesis that $\vec{e}_T=\vec{0}$, i.e. that there is truly no MET in the event:
\begin{align}
    S &= \frac{\mathcal{L}(\vec{\epsilon}=\vec{\epsilon}_\mathrm{event} \mid \vec{e}=\vec{0})}{\mathcal{L}(\vec{\epsilon}=\vec{0} \mid \vec{e}_T=\vec{0})}\\
    &= (\vec{E}_T^\mathrm{meas})^T\bigg(\sum_{i=1}^{n_\mathrm{meas}}V_i\bigg)^{-1}(\vec{E}_T^\mathrm{meas})
\end{align}

Events with significant MET signal the presence of unmeasured particles like the neutrinos produced in tau decays. It should be possible to leverage MET information to constrain the invisible systems produced by tau decays; this observation forms the basis of several algorithms designed to correct the di-tau mass spectrum. 

## Correcting the Di-Tau Mass Spectrum

### Classic SVfit
One such approach is the original SVfit algorithm ("Classic SVfit") [^3], named because its authors originally sought to include secondary vertex information corresponding to the di-tau decay position. In essence, SVfit is a dynamical likelihood minimization (performed on an event-by-event basis) that incorporates relevant tau decay physics. Given a set of event data $\mathcal{D}$, which includes measured tau decay variables $\vec{y} = ( E_\mathrm{vis}^{(1)},\vec{p}_\mathrm{vis}^{(1)}, E_\mathrm{vis}^{(2)},\vec{p}_\mathrm{vis}^{(2)})$ and MET quantities $\vec{E}_T^\mathrm{miss}=(E_x^\mathrm{miss}, E_y^\mathrm{miss})$ and $V$ (the event's MET covariance), and unmeasured tau decay parameters $\vec{a}$, the likelihood of the di-tau invariant mass being $m_{test}$ is 

\begin{align}
\mathcal{L}(m_\mathrm{test}\mid\vec{y}, \vec{E}_T^\mathrm{miss}, V) &= \int \mathcal{L}(m_\mathrm{test}\mid \vec{y},\vec{E}_T^\mathrm{miss},V,\vec{a}) \delta(m(\vec{y}, \vec{a})-m_\mathrm{test})d\vec{a}
\end{align}

Here, we're integrating over the unknown quantities $\vec{a}$; each value of $\vec{a}$ in the integrand produces an invariant mass $m(\vec{y}, \vec{a})$ that must equal $m_{test}$, hence the delta function. The key here is that we're marginalizing over the variables parametrizing the invisible system, thereby finding the "most probable" di-tau decay configuration given a set of visible tau candidates and MET measurements. The integrand likelihood $\mathcal{L}(m_\mathrm{test}\mid \vec{y}, \vec{E}_T^\mathrm{miss}, V, \vec{a})$ is the product of several functions:

- The likelihoods of each tau's decay mode, which are taken to be proportional to the differential branching ratio, parametrized by the undetermined variables, for each tau decay,
\begin{align}
    \mathcal{L}(\tau\rightarrow l\nu_l\nu_\tau) &\propto \frac{d\Gamma}{dx\ d\phi\ dm_{\nu\nu}} = \frac{m_{\nu\nu}}{4m_\tau^2}\big((m_{\tau}^2+2m_{\nu\nu}^2)(m_\tau^2-m_{\nu\nu}^2) \big)\\
    \mathcal{L}(\tau\rightarrow \tau_h\nu_\tau) &\propto\frac{d\Gamma}{dx\ d\phi}=\frac{1}{2\pi}\bigg(\frac{1}{1-(\frac{m_\mathrm{vis}}{m_\tau})^2}\bigg)
\end{align}
It's important to note that the kinematics of each tau decay are constrained. Recall that $x=E_\mathrm{vis}/E_\tau$. In leptonic tau decays, $0<x<1$, meaning the visible system may inherit any fraction of the original tau energy. However, in hadronic tau decays, $(\frac{m_\mathrm{vis}}{m_\tau})^2<x<1$ due to the denominator of $\frac{d\Gamma}{dx\ d\phi}$.  

- The *MET Transfer Function* $\mathcal{L}(\vec{E}_T^\mathrm{miss}\vert V, \vec{a})$, describing the likelihood of an invisible system $\vec{a}$ producing a set of measured MET quantities $\vec{E}_T^\mathrm{miss}$. The key assumption here is that the tau decay neutrinos are the *only* source of MET in a given event. Given $\vec{a}$, we have a full description of the di-tau invisible system, including the exact MET it produces; let's call this value $\vec{e}_T = (e_x,e_y)$. In this case, the measured MET quantities $\vec{E}_T^\mathrm{miss}=(E_x^\mathrm{miss}, E_y^\mathrm{miss})$ should match $e_x$ and $e_y$ up to detector resolution. Assuming a Gaussian detector resolution, we take $\mathcal{L}(\vec{E}_T^\mathrm{miss}\vert V, \vec{a})$ to be the Gaussian likelihood of measuring residuals $\vec{\epsilon}=\vec{E}_T^\mathrm{miss}-\vec{e}_T$:
$$\mathcal{L}(\vec{E}_T^\mathrm{miss}\vert V, \vec{a}) = \frac{1}{2\pi\sqrt{\vert V\vert}}\exp\bigg(-\frac{1}{2}(\vec{\epsilon})^T \big(V^{-1}\big) (\vec{\epsilon})\bigg)$$
- Artificial regularization terms of the form $m_{\tau_1\tau_2}^{-\kappa}$ are included, where $\kappa$ is a tunable parameter that suppresses the likelihood of high di-tau mass preedictions.


In practice, the distriution of $\mathcal{L}(m_\mathrm{test} \mid \mathcal{D})$ is computed for a discretized set of mass hypotheses $m_\mathrm{test}^i$ and the likelihood integral determining $P(m_\mathrm{test}^i)$ is performed numerically. 

### FastMtt
The FastMtt algorithm was developed as a faster version of Classic SVfit. FastMtt makes several key simplifications:

- Empirically, it was shown that the MET transfer function $\mathcal{L}(\vec{E}^\mathrm{miss}_T \mid V, \vec{a})$ dominates the tau decay likelihoods in the integral yielding $\mathcal{L}(m_\mathrm{test} \mid \mathcal{D})$. For this reason, the authors neglect contributions due to the tau decay matrix elements. 
- Each visible tau decay product is assumed to move in the original tau direction; this is often called the *collinear approximation*. In this case, $\theta_{GJ}=0$ so that $\phi$ isn't well-defined. This assumption is justfied for highly-boosted taus, whose decay products are collimated in the original tau direction. The upshot is that the collinear approximation gives 
$$m_{\tau_1\tau_2}\approx \frac{m_{\tau_1\tau_2}^\mathrm{vis}}{\sqrt{x^{(1)}x^{(2)}}}$$ 
To see why, check out the calculation in [^6]. In this approximation, we only need to scan over $x^{(1)}$ and $x^{(2)}$ to specify the di-tau decay because $E_\mathrm{vis}^{(i)} \approx \vert\vec{p}_\mathrm{vis}^{(i)}\vert$ implies we can reconstruct the original tau four-vectors from visible measurements with just $x^{(1)}$ and $x^{(2)}$:
\begin{align}
    p_{\tau_i} &= (E_{\tau_i}, \vec{p}_{\tau_i}) \\ &\approx (\frac{E_\mathrm{vis}^{(i)}}{x_i}, \frac{\vec{p}_\mathrm{vis}^{(i)}}{x_i}) = \frac{1}{x_i}p_\mathrm{vis}^{(i)}
\end{align}

Basically, these simplifications mean that if we scan over values of $(x^{(1)}, x^{(2)})$, then the predicted four-vectors for each tau are $p_{\tau_1}=p_\mathrm{vis}^{(1)}/x^{(1)}$ and $p_{\tau_2}=p_\mathrm{vis}^{(2)}/x^{(2)}$, so the invisible system we're predicting is $p_{inv} = (p_{\tau_1} + p_{\tau_2}) - (p_\mathrm{vis}^{(1)}+p_\mathrm{vis}^{(2)})$. Since each $(x^{(1)}, x^{(2)})$ point specifies a test mass $m_\mathrm{test}$ via the collinear approximation, all we need to do is calculate the MET transfer likelihood of measuring $\vec{E}_T^\mathrm{miss}$ given $p_\mathrm{inv}$ and $V$, 
\begin{align}
    &\mathcal{L}(\vec{E}_T^\mathrm{miss}\mid p_\mathrm{inv}, V) = \frac{1}{2\pi\sqrt{\vert V\vert}}(\vec{\epsilon})^T\big(V\big)^{-1}(\vec{\epsilon})\\
    &\text{where} \ \vec{\epsilon} =  \begin{bmatrix} E^\mathrm{miss}_x - (p_\mathrm{inv})_x\\E^\mathrm{miss}_y - (p_\mathrm{inv})_y \end{bmatrix}
\end{align}
and then multiply it by the phase space volume possible for the invisible system specified by $(x^{(1)}, x^{(2)})$; this is the FastMtt likelihood function: 

\begin{align}
    \mathcal{L}(m_\mathrm{test}\mid\mathcal{D}) &= \mathcal{L}(\vec{E}_T^\mathrm{miss}\mid p_\mathrm{inv}, V)\int\delta(\frac{m^\mathrm{vis}_{\tau_1\tau_2}}{\sqrt{x^{(1)}x^{(2)}}} -m_\mathrm{test})d\vec{a}
\end{align}

Fortunately, the integral above (call it $\mathcal{V}$ since it's a phase space volume) can be calculated analytically for each di-tau decay configuration [^4], yielding $\mathcal{V}_{\tau_h\tau_h}$ for fully-hadronic di-tau decays, $\mathcal{V}_{\tau_l\tau_h}$ for semi-leptonic di-tau decays, and $\mathcal{V}_{\tau_l\tau_l}$ for fully-leptonic di-tau decays. These volumes tell us how much phase space is available for a given test mass. FastMtt scans over $(x^{(1)}, x^{(2)})$ space in a grid of $100\times100$ points covering the range $[0,1]\times[0,1]$. The algorithm also includes artificial regularization of the form $m_{\tau_1\tau_2}^{-\kappa}$. Despite its simplifying assumptions, FastMtt's physics performance is comparable to SVfit's. Notably, though, FastMtt is $~100\times$ faster than SVfit. 

[^1] [PDG Higgs Status Report](https://pdg.lbl.gov/2019/reviews/rpp2018-rev-higgs-boson.pdf)
[^2] [PDG Tau Properties Table](https://pdglive.lbl.gov/Particle.action?node=S035)
[^3] [Classic SVfit Algorithm](https://inspirehep.net/files/ff3326a782d43e44e94733a065eb9c31)
[^4] FastMTT Algorithm: CMS AN-19-032 (available to CERN users)
[^5] [Missing transverse energy performance of the CMS detector](https://arxiv.org/pdf/1106.5048.pdf)
[^6] In the collinear approximation, we assume that each tau is energetic enough to be considered massless (i.e. $E_{\tau_i}\approx \vert\vec{p}_{\tau_i}\vert$) and each tau's decay products are collinear with the original tau direction (i.e. $\vec{p}_{\tau_i}\ \vert\vert\ \vec{p}_\mathrm{vis}^{(i)}$). Then, $m_{\tau_1\tau_2}$ is related to $m^\mathrm{vis}_{\tau_1\tau_2}$ through the variables $x^{(1)}=E_\mathrm{vis}^{(1)}/E_{\tau_1}$ and $x^{(2)}=E_\mathrm{vis}^{(2)}/E_{\tau_2}$ as follows:
\begin{align}
    m_{\tau_1\tau_2}^2 &= (p_{\tau_1} + p_{\tau_2})^2 = (E_{\tau_1} + E_{\tau_2})^2 - (\vec{p}_{\tau_1} + \vec{p}_{\tau_2})^2\\
    &\approx 2E_{\tau_1}E_{\tau_2} - 2\vec{p}_{\tau_1}\cdot\vec{p}_{\tau_2}\\
    &= 2\frac{E_\mathrm{vis}^{(1)}E_\mathrm{vis}^{(2)}}{x^{(1)}x^{(2)}}\big(1-\cos\theta \big)\\
(m^\mathrm{vis}_{\tau_1\tau_2})^2 &= (p_\mathrm{vis} + p_\mathrm{inv})^2 = (E_\mathrm{vis}^{(1)} + E_\mathrm{vis}^{(2)})^2 - (\vec{p}_\mathrm{vis}^{(1)} + \vec{p}_\mathrm{vis}^{(2)})^2\\
    &\approx 2E_\mathrm{vis}^{(1)}E_\mathrm{vis}^{(2)} - 2\vec{p}_\mathrm{vis}^{(1)}\cdot\vec{p}_\mathrm{vis}^{(2)}\\
    &= 2E_\mathrm{vis}^{(1)}E_\mathrm{vis}^{(2)}\big(1-\cos\theta\big)\\
    \implies \ & \ m_{\tau_1\tau_2}^2 = (m^\mathrm{vis}_{\tau_1\tau_2})^2/(x^{(1)}x^{(2)})
\end{align}