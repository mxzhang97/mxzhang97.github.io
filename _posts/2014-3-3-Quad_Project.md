---
layout: post
title: "End to End Ballistic Interception and Emergent Atheleticsm of Quadrupedal Robot"
---

### Abstract
This report and subsequent analysis focuses on a learning based solution for the predictive ballistic interception task ("catching"/colliding with a thrown ball) for a quadruped (Go2). We utilize a multi-stage curriculum, with each curriculum stage focusing on solving/developing a different dimension of the learning problem. The result is a control policy capable of emergent atheletic intelligence, and a conversion methodology we'll call "Ghosting" to repurpose this control policy into a locomotion controller.

---

<!-- HOW-TO: Go to your YouTube video, click "Share" -> "Embed", and copy the src="..." URL here. -->
<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TRAILER_VIDEO_ID" title="Project Trailer" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

---

## Control Policy
For brevity, we focus on the best control policy for this section. The learning problem is defined as this, and the key learning environments are structured like this.


#### Curriculum Stage Comparison
This scatter compares the control policy performance envelope across the learning environment cross section for `action_scale = 0.5` policy across a variety of throws (<2.2m, >3.2m are out of sample throws).

<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TRAILER_VIDEO_ID" title="Project Trailer" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

#### Atheletcism Comparison
This plot shows the capability progression through the multi-stage curriculum for `action_scale = 0.5`.

<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TRAILER_VIDEO_ID" title="Project Trailer" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

#### Learned Bias

Here we show the learning artifacts induced by an ego-centric learning environement (IE throws are sampled relative to the robot's orientation.)

<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TRAILER_VIDEO_ID" title="Project Trailer" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>


#### Expressiveness vs Stability

Here we look at the performance/tradeoffs of increasing action_scale.

[scatter of action_scale cross section]

[plot of eval2, world frame evaluation showing learned bias again across the action_scales]

<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TRAILER_VIDEO_ID" title="Project Trailer" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>


## Ghosting

#### Empirical Repurposing of Policy

We repurpose the control policy by mapping an empirical controller and passing in fake ball_state observations to the control policy.

<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TRAILER_VIDEO_ID" title="Project Trailer" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>


## Perception

#### LSTM Denoiser

Here we'll briefly talk about the DR on observation space to find the +/- 0.05 tolerance for the control policy as the target for the RNN denoiser. The training data is captured from simulation playback.

[table of performance metrics between ground truth, noisy observation, denoised observation]



---

## Project Assets

*   **[Read the Full Written Report (PDF)]({{ site.baseurl }}/assets/Project_Chimera_Report.pdf)** - A detailed, five-act narrative report written in the style of a "Friday check-in."
*   **[Explore the Code on GitHub](https://github.com/your-username/your-repo-name)** - The complete source code, analysis scripts, and trained models.
