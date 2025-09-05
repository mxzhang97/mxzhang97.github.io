---
layout: post
title: "Project Chimera: A Novel Control Methodology for Legged Robots"
---

### Problem Description and Learning-Based Solution
This report and subsequent analysis focuses on a learning based solution for the predictive ballistic interception task ("catching"/colliding with a thrown ball) for a quadruped (Go2). We utilize a multi-stage curriculum, with each curriculum stage focusing on solving/developing a different dimension of the learning problem. The result is a control policy capable of emergent atheletic intelligence, and a conversion methodology we'll call "Ghosting" to repurpose this control policy into a locomotion controller.

---

<!-- HOW-TO: Go to your YouTube video, click "Share" -> "Embed", and copy the src="..." URL here. -->
<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TRAILER_VIDEO_ID" title="Project Trailer" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

---

## The Evidence: Key Results
The following plots were generated from the final, validated datasets. They provide quantitative proof of the "Ghosting" controller's effectiveness.

#### Outcome Scatter Plot
This plot shows a high success rate and robust performance envelope for the `skill_balanced_0.5` policy across a variety of simulated terrains and conditions.

<!-- HOW-TO: Create an 'images' folder in the root of your repository, upload your plot, and link it here. -->
![Outcome Scatter Plot]({{ site.baseurl }}/images/outcome_scatter_plot.png)
*Fig 1: A high density of successful outcomes (blue) demonstrates the controller's reliability.*

#### Aggregate Torque Stability Plot
This plot illustrates the smoothness of the control signals. The significantly lower torque variance when using the "Ghosting" controller is a direct indicator of improved stability and efficiency.

![Aggregate Torque Stability Plot]({{ site.baseurl }}/images/torque_stability_plot.png)
*Fig 2: The 'Ghosting' policy (green) shows markedly lower torque variance compared to the baseline controller.*

---

## Technical Showcase
This video provides a slow-motion, detailed look at the controller's performance across different gaits. Data overlays highlight key performance metrics in real-time.

<!-- HOW-TO: Get the embed URL for your second video here. -->
<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TECHNICAL_VIDEO_ID" title="Technical Showcase" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

---

## Project Assets

*   **[Read the Full Written Report (PDF)]({{ site.baseurl }}/assets/Project_Chimera_Report.pdf)** - A detailed, five-act narrative report written in the style of a "Friday check-in."
*   **[Explore the Code on GitHub](https://github.com/your-username/your-repo-name)** - The complete source code, analysis scripts, and trained models.
