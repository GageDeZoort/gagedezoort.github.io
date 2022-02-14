---
title: 'Transverse Track Fitting in Conformal Space'
date: 2022-2-13 
featured_image: '/images/conformal_tracking/track_fit_uv_side-by-side.jpg'
excerpt: 
katex: yes
---

## Introduction
Particle tracking is the connecting-the-dots process of linking tracker hits to reconstruct particle trajectories. Trackers in collider experiments like CMS are typically built from coaxial cylindrical layers surrounding the beamline, taken to be the $z$-axis. They are immersed in a strong, axially-aligned magnetic field ($$\mathbf{B}=B\hat{\mathbf{z}}$$) so that charged particles follow helical trajectories as they traverse the tracker (see Fig. 1). Each helix is specified by five parameters; fitting these parameters is one goal of track reconstruction. Viewed head-on, helical trajectories are circles in the $x$-$y$, or transverse, plane. Two key properties of a track, namely its transverse momentum $$p_T^2 = p_x^2 + p_y^2$$ and transverse impact parameter $d_0$, can be measured using only the transverse view of the helix. The goal of this post is to discuss two strategies for transverse track fitting: circle fits in the $x$-$y$ plane ([Traditional Transverse Tracking](#traditional-transverse-tracking)) and linear/quadratic fits using a conformal mapping of the hit coordinates ([Conformal Tracking](#conformal-tracking)). 


<br><br>
![](/images/conformal_tracking/notation_diagram.png)
<br><br>

## Traditional Transverse Tracking 
This section summarizes a nice discussion of track fitting in Data Analysis Techniques for High-Energy Physics [^1]. Charged particles traversing a magnetic field experience a Lorentz force $\mathbf{F}=\kappa q(\mathbf{v}\times\mathbf{B})$, where $q=Q/|e|$ for an object of charge $Q$ and $\kappa$ is a dimensionful constant. Accordingly, tracks with $q>0$ rotate clockwise. In the transverse plane, and neglecting material effects and bremsstrahlung, the force on the particle is $F=\kappa qvB$, resulting in a constant centripital acceleration:

$$\begin{aligned}
    &\kappa |q|vB = m\frac{v^2}{R}\\
    &\implies p_T = \kappa |q|BR
\end{aligned}$$ 

In natural units, $$ \kappa = 0.3\ \big[\frac{\text{GeV}}{\text{T}\cdot\text{m}}\big]$$. For example, the CMS solenoid has strength $B=4\ \mathrm{T}$. A particle with $p_T = 25\ \mathrm{GeV}$ has a radius $R=333\ \mathrm{m}$, moving in nearly a straight line through the tracker. On the other hand, a particle with $p_T = 100\ \mathrm{MeV}$ moves with radius of $1.3\ \mathrm{m}$, which is significant compared to the tracker's geometric radius ($\sim 1\ \mathrm{m}$). In summary, to measure a track's transverse momentum, you just need to find its radius of curvature in the transverse plane. 

In tracking charged particles, one typically defines a reference point, for example a primary proton-proton interaction of interest (the primary vertex or PV). Prompt tracks eminate straight from the IP; in other words, the circular trajectory they follow passes through the origin. However, not all tracks are prompt. Some belong to secondary proton-proton interactions (pileup), others to the decay products of long-lived particles, which may have come from the PV. One important measure of the consistency of a track with the PV is its transverse impact parameter $d_0$, the distance of closest approach between a track's circular trajectory and the origin in the transverse plane. If you've fit a circle $C$ to your track, yielding a center $(x_c, y_c)$ and radius $R$, you can calculate the transverse impact parameter as:

$$\begin{aligned}
    d_0 = \big[R-\sqrt{x_c^2 + y_c^2}\big]
\end{aligned}$$

Notice that $d_0$ is signed! The interpretation is that if $\sqrt{x_c^2 + y_c^2}>R$, then the origin is not contained in $C$ and so $d_0<0$. On the other hand, if $\sqrt{x_c^2+y_c^2}<R$, then the origin *is* contained in $C$ and $d_0>0$. If the track comes directly from the origin, then $R^2 = x_c^2 + y_c^2$ and $d_0=0$. 



To illustrate these points, let's look at the following collection of tracks belonging to the TrackML dataset [^2]: 

<br><br>
![](/images/conformal_tracking/tracks_3d.jpg)
<br><br>

The $p_T$ values indicated in the plot legend are truth values. The beamline is indicated by the straight red line along the $z$-axis. The low energy tracks, green ($p_T=0.4\ \mathrm{GeV}$) and orange ($p_T=0.2\ \mathrm{GeV}$) have notably curved trajectories. On the other hand, the red track ($p_T=5.1\ \mathrm{GeV}$) is relatively straight. Next, we can fit these tracks using a simple cicle-fitting algorithm [^3]:

<br><br>
![](/images/conformal_tracking/track_fit_xy_side-by-side.jpg)
<br><br>

The left subplot shows the full circular fits - by eye, most look reasonable. As expected, the orange and green tracks have relatively small radii of curvature, while the others are appreciably straight on the scale of the plot. The right subplot zooms in to the origin, which is indicated with a black plus mark. Note that all track fits are consistent with this choice of origin except the orange track, which appears to be offset by $1\ \mathrm{cm}$. Using the above equations for $p_T$ and $d_0$, we can use these circle fits to estimate the transverse track parameters. For example, the fit to the orange track gave $R^\mathrm{blue}=0.4056\ \mathrm{m}$, $x_c^\mathrm{blue}=-0.273\ \mathrm{m}$, and $y_c^\mathrm{blue}= 0.3199\ \mathrm{m}$. Approximating the TrackML detector's magnetic field as a homogenous $B=2\ \mathrm{T}$, we calculate:

$$\begin{aligned}
    p_T^\mathrm{blue} &= \kappa BR^\mathrm{blue} \approx 0.243 \ \mathrm{GeV}\\
    d_0^\mathrm{blue} &= R^\mathrm{blue} - \sqrt{(x_c^\mathrm{blue})^2 + (y_c^\mathrm{blue})^2} \approx -15.05\ \mathrm{mm}
\end{aligned}$$

Repeating the same calculation for each track, we have: 

| Track  | $$p_T^\mathrm{true}$$ [GeV] | Measured $$p_T$$ [GeV] | Measured $$d_0$$ [mm] | 
|:------:|:---------------------------:|:----------------------:|:---------------------:|
| Blue   | $$0.227$$                   | $$0.243$$              | $$-15.05$$            |
| Orange | $$0.401$$                   | $$0.454$$              | $$1.891$$             |
| Green  | $$1.642$$                   | $$1.676$$              | $$-0.025$$            |
| Red    | $$3.362$$                   | $$3.698$$              | $$0.117$$             |
| Purple | $$5.126$$                   | $$5.292$$              | $$-0.004$$            | 

Clearly these aren't precision measurements, but there is a consistent picture between these values and visual inspection. The blue track is, indeed, significantly displaced in the transverse plane, while the other tracks are more-or-less consistent with the PV. The predicted $p_T$ values appear to slightly overshoot their true values, which indicates the presence of some systematic error. This may be due to the known inhomogeneities in the magnetic field of the TrackML dataset or bias in the fitting strategy. 

## Conformal Tracking 
In the previous section, we established that tracks follow circular trajectories in $x$-$y$ space. One of the major drawbacks of this approach is the need for a circle-fitting routine, which is typically an iterative algorithm. The authors of [^4] proposed a faster approach based on the following conformal map taking hit coordinates $(x, y)\rightarrow(u, v)$: 

$$\begin{aligned}
u = \frac{x}{x^2 + y^2}, \ \ v = \frac{y}{x^2 + y^2}
\end{aligned}$$

The upshot is that prompt tracks appear as straight lines in conformal space! To see this, consider a circle in $(x,y)$ space centered at some $(x_c, y_c)$:

$$\begin{aligned}
    (x-x_c)^2 + (y-y_c)^2 = R^2
\end{aligned}$$

Since the track is prompt, the circle should intersect with the origin, i.e. $R^2=x_c^2+y_c^2$. Transforming to conformal space, we have:

$$\begin{aligned}
    &(x-x_c)^2 + (y-y_c)^2 = x_c^2 + y_c^2\\
    &\rightarrow  \ \ x^2 + y^2 - 2x_cx - 2y_cy = 0\\
    &\ \ \ \ \ \ \ \ 1 - 2x_cu - 2y_cv = 0\\
    &\implies \ \ v = \frac{1}{2y_c} - \frac{x_c}{y_c}u
\end{aligned}$$ 

As promised, any hits on the $(x,y)$ circle appear along a line in $(u, v)$ space. To see this in action, let's take the tracks from the previous section and cast them into conformal space:

<br><br>
![](/images/conformal_tracking/track_fit_uv_line.jpg)
<br><br>

Each set of same-track hits was fit by a line via standard linear regression. Note that the units here are inverse meters - hits from the inner layers of the detector are farther from the origin than outer tracker hits in conformal space. Clearly, the linear fit is appropriate for all tracks except the blue track. As discussed in the previous section, the blue track has a non-negligable displacement of $\approx 1.5\ \mathrm{cm}$. It turns out that this displacement is directly responsible for this non-linearity. If a track has non-zero $d_0$ then $R^2 - x_c^2 - y_c^2 = \delta$ is non-zero, meaning that in $(x,y)$ space the particle's circular trajectory did not intersect with the origin. The authors in [^4] calculate a correction for this effect, a quadratic term relating displacement in $(x,y)$ space to curvature in $(u,v)$ space:

$$\begin{aligned}
    v = \frac{1}{2y_c} - \frac{x_c}{y_c}u - d_0\bigg(\frac{R}{y_c}\bigg)^3u^2
\end{aligned}$$ 

Given this correction, let's compare linear and quadratic fits in $(u, v)$ space:

<br><br>
![](/images/conformal_tracking/track_fit_uv_side-by-side.jpg)
<br><br>

Here, the quadratic fit was implemented via standard linear regression. Qualitatively speaking, the quadratic correction appears to have captured the non-linearity of the blue trajectory. Let's use the fit to calculate the displacement of the blue track: 

$$\begin{aligned}
    &\text{Fit:}\ \ \ v = 1.4917 + 0.8298u -0.0220u^2\\
    & y_c = \frac{1}{2\times 1.4917} \approx 0.3352 \ \mathrm{m}  \\
    & x_c = \frac{-0.8298}{2\times1.4917} \approx -0.2781\ \mathrm{m} \\
    & R\simeq \sqrt{x_c^2 + y_c^2} \approx 0.4355\ \mathrm{m} \\
    & p_T = 0.3\times 2\times R \approx 0.2613\ \mathrm{GeV} \\
    & d_0 = 0.0220\bigg(\frac{y_c}{R}\bigg)^3 \approx -10.04 \ \mathrm{mm}
\end{aligned}$$

Again, we have confirmation that the blue track does have a non-negligable transverse momentum parameter, though in $(u,v)$ space it is measured to be two-thirds the value measured in $(x,y)$ space. Repeating this calculation for all the tracks in $(u,v)$ space, we have:

| Track  | $$p_T^\mathrm{true}$$ [GeV] | Measured $$p_T$$ [GeV] | Measured $$d_0$$ [mm] | 
|:------:|:---------------------------:|:----------------------:|:---------------------:|
| Blue   | $$0.227$$                   | $$0.261$$              | $$-10.04$$            |
| Orange | $$0.401$$                   | $$0.433$$              | $$0.232$$             |
| Green  | $$1.642$$                   | $$1.718$$              | $$-0.016$$            |
| Red    | $$3.362$$                   | $$3.159$$              | $$-0.012$$            |
| Purple | $$5.126$$                   | $$5.455$$              | $$0.000$$             | 

The authors of [^4] include detailed error calculations for each fit parameter and, subsequently, $p_T$ and $d_0$. Notably the values calculated in $(x,y)$ space and $(u,v)$ space are not perfectly in sync. This is almost certainly due to inconsitencies in the fit results - careful tuning of the fitting strategy, in addition to a dedicated set of quality checks and re-fits, is necessary for a precise comparison. At the level of this blog post, though, these results are remarkably similar. One could imagine leveraging both fits to make a better measurement of the transverse track parameters, or perhaps using the results of a quadratic fit in $(u,v)$ space to seed a circle fit in $(x,y)$ space. If you'd like to try any of the code used to generate the plots and numbers in this post, please keep an eye on my GNN tracking repo [^5]. 

[^1]: Data Analysis Techniques for High-Energy Physics by Fruhwirth, Regler, Bock, Grote, and Notz
[^2]: [TrackML Dataset](https://www.kaggle.com/c/trackml-particle-identification)
[^3]: Circles were fit by minimizing the squared between a set of test circle centers and the set of track hits. Once the optimal center was identified, the average distance between the track hits and the optimal center was used as the circle radius. 
[^4]: [Fast Circle Fit With The Conformal Mapping Method](https://cds.cern.ch/record/192472/files/198904049.pdf)
[^5]: [https://github.com/GageDeZoort/gnns-for-tracking](https://github.com/GageDeZoort/gnns-for-tracking)