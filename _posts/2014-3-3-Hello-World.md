---
layout: post
title: "Project Chimera: A Novel Control Methodology for Legged Robots"
---

### Abstract
This project is the deliverable of a one-month, high-fidelity work simulation undertaken in response to direct feedback from a tech lead at Tesla Optimus. The core contribution is "Ghosting," a novel, general-purpose control methodology designed to enhance stability and robustness in legged robots. This page presents the undeniable body of evidence—video demonstrations and quantitative analysis—that proves the value of this approach and showcases a top-tier robotics engineering skill set.

---

## The Project Trailer
This video summarizes the project's narrative and showcases the final performance of the controller in a seamless, "GTA-style" driving demo.

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
