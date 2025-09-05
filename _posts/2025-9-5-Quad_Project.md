---
layout: post
title: "End to End Ballistic Interception and Emergent Atheleticsm of Quadrupedal Robot"
---

### Abstract
This project presents a learning-based solution for the predictive ballistic interception task ("catching"/colliding with a thrown ball) for a quadruped (Unitree Go2). We utilize a multi-stage curriculum, progressing from the baseline interception task to learning environments designed to induce more "athleticism". The result is a control policy that exhibits certain emergent capabilities not explicitly enforced through reward design, and a conversion methodology we'll call "Ghosting" to repurpose this control policy into a locomotion controller.

---

<!-- HOW-TO: Go to your YouTube video, click "Share" -> "Embed", and copy the src="..." URL here. -->
<div class="video-grid">
    <!-- Left Video: The Interception Skill -->
    <div class="video-item">
        <div class="video-container">
            <iframe 
                src="https://www.youtube.com/embed/-F0L2Z_LisA?&autoplay=1&mute=1&loop=1&playlist=-F0L2Z_LisA&playsinline=1" 
                title="Interception Skills Showcase" 
                frameborder="0" 
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                allowfullscreen>
            </iframe>
        </div>
        <p style="text-align: center; font-size: 0.9em; color: #666;"><strong>Left:</strong> General and specialized interception skill. </p>
    </div>
    <!-- Right Video: The "Ghosting" Application -->
    <div class="video-item">
        <div class="video-container">
            <iframe 
                src="https://www.youtube.com/embed/Y5Rdwf-AbWo?&autoplay=1&mute=1&loop=1&playlist=Y5Rdwf-AbWo&playsinline=1" 
                title="Ghosting Locomotion Demo" 
                frameborder="0" 
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                allowfullscreen>
            </iframe>
        </div>
        <p style="text-align: center; font-size: 0.9em; color: #666;"><strong>Right:</strong> The repurposed "Ghosting" controller for locomotion.</p>
    </div>
</div>

---

## Control Policy
For brevity, we focus on the best control policy for this section. We detail the problem formulation and behavioral insights discovered during the process. The learning problem is defined as this follows,

### The Problem Formulation

The control problem is formulated as a standard reinforcement learning task. The policy is trained to map observations from a 52-dimensional space to actions in a 12-dimensional space.

<div class="compact-table" markdown="1">
    
| Component | Description | Dimension |
| :--- | :--- | :---: |
| **Observation Space** | | **52** |
| `lin_vel_b` | The robot's linear velocity in the body frame (`x, y, z`). | 3 |
| `ang_vel_b` | The robot's angular velocity in the body frame (`roll, pitch, yaw`). | 3 |
| `projected_gravity_b` | The projected gravity vector in the body frame | 3 |
| `joint_pos_delta` | The deviation of each joint from the default/neutral pose. | 12 |
| `joint_vel` | The joint velocity of each joint. | 12 |
| `previous_actions` | The action taken in the previous timestep. | 12 |
| `ball_pos_b` | The ball's position relative to the robot's body frame (`x, y, z`). | 3 |
| `ball_vel_b` | The ball's velocity relative to the robot's body frame (`x, y, z`). | 3 |
| `catch_state` | A binary flag indicating if the catch event is active. | 1 |
| | | |
| **Action Space** | | **12** |
| `joint_pos_delta` | The policy outputs a vector of target deviations from the default pose. | 12 |
| | | |
| **Reward Structure** | *Note: Terms are modulated across curriculum stages.* | |
| _Task-Specific Rewards_ | | |
| `catch_reward` | A large positive reward for successful contact between the ball and the upper head. | |
| `proximity_reward` | A reward for near-misses. | |
| `feet_air_time` | Canonical locomotion reward. | |
| `xy_distance_reward` | A privileged reward on the 2D distance to the predicted catch location. | |
| _Standard Motion Penalties_ | | |
| `action_rate_pen` | Penalizes the difference between consecutive actions to promote smoothness in outputs. | |
| `projected_gravity_pen` | Penalizes deviation from a stable posture. | |
| `undesired_contact_pen` | Penalizes contact between the ground and any part of the robot other than the feet. | |
| `joint_torque_pen` | Creating "cost" for utilizing torque | |
| `joint_accel_pen` | Creating "cost" for joint accelerations. | |

</div>


### Curriculum Progression

The earlier stages of the learning environments focuses on general omni-directional locomotion for intercepting the thrown ball. We utilize canonical locomotion rewards available in public domain, the privileged reward `xy_distance_reward`, and `proximity_reward` to facilitate this stage of learning (solving the sparse learning signal problem). Later stages of learning environments prioritize "atheleticism", and thus removes these reward terms and rely on the main `catch_reward` and the long-episode construction to implicitly teach stability.

#### General "Catch" Capability

First, we evaluate the performance of our policy for the general catch task: intercepting/catching a single throw in the forward facing semi circle.

<figure style="text-align: center;">
  <img src="{{ site.baseurl }}/images/action_scale_0.5_curriculum_progression_scatter.png" alt="Curriculum Progression Scatter Plot">
  <figcaption style="font-size: 0.9em; color: #666; margin-top: 0.5rem;">
    Fig 1: This scatter compares the control policy's performance envelope through the curriculum progression for the <code>action_scale = 0.5</code> policy. Throws &lt;2.2m and &gt;3.2m are out-of-sample.
  </figcaption>
</figure>

<div class="video-container">
  <iframe src="https://www.youtube.com/embed/TXPPXhezn3E?&autoplay=1&mute=1&loop=1&playlist=TXPPXhezn3E&playsinline=1" title="General Catch (2m-3m)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  <p style="text-align: center; font-size: 0.9em; color: #666;"><strong>Left:</strong> General Catch (0.5 Action Scale) </p>
</div>

#### Athleticism & Learned Bias

Next, we evaluate the policy on specialized drills where the ball is thrown continously (after a catch/miss event). We also note a behavioral artifact induced by our learning environment. 

*   **Fig 2 (Ego-centric Drills):** When drills are performed in an ego-centric frame (throws are sampled relative to the robot's current orientation), the policy exhibits healthy learning progression across the curriculum stages.
*   **Fig 3 (World-frame Drills):** However, when the *same drills* are performed in a world-frame (throws are fixed, regardless of robot orientation), we see a significant performance drop, in particular for the "left box drill."

<figure style="text-align: center;">
  <img src="{{ site.baseurl }}/images/action_scale_0.5_curriculum_progression_eval3.png" alt="Curriculum Progression Drills Chart (Body Frame)">
  <figcaption style="font-size: 0.9em; color: #666; margin-top: 0.5rem;">
    Fig 2: This chart shows the control policy's "atheletic" capabilities through the curriculum progression for the <code>action_scale = 0.5</code> policy. 
  </figcaption>
</figure>


<figure style="text-align: center;">
  <img src="{{ site.baseurl }}/images/action_scale_0.5_curriculum_progression_eval2.png" alt="Curriculum Progression Drills Chart (Body Frame)">
  <figcaption style="font-size: 0.9em; color: #666; margin-top: 0.5rem;">
    Fig 3: This chart shows drop in capability when evaluated with fixed world frame throws. 
  </figcaption>
</figure>

The performance difference between Fig 2. and Fig 3. reveals a learned yaw bias. Because excess yaw motion was never explicitly penalized in the learning environments, the policy developed an asymmetric ready position (likely to speed up pushoff/acceleration). This bias then causes an arcing interception trajectory (more acute in shorter distance throws), which further causes the robot's end orientation to yaw to the right. Within a world frame evaluation framework, this pushes subsequent throws outside the performance envelope of the control policy, causing failure. The likely cause for this artifact is due to the ego-centric nature of throw sampling within the learning environments. 

We show two video examples of this below. The first example is the artifact in action during the initial stabilization phase (after the asset spawns within the simulation), this is the `action_scale=0.75` (we found the most extreme version of this) and a scatter plot of the policy under the "General Catch" evaluation framework. The yaw bias active in the initial stabilization phase of the episode angles the robot to the right, and thus causes failure for throws coming from the left. The second example, going back our `action_scale = 0.5` policy, focuses on shorter distance throws within the specialized drills evaluation framework (World Frame). We show this yaw bias throughout the catch task: the initial pushoff, the arcing path, and the resulting orientation after the catch.

<!-- HOW-TO: Go to your YouTube video, click "Share" -> "Embed", and copy the src="..." URL here. -->
<div class="video-grid">
    <!-- Left Video: The Interception Skill -->
    <div class="video-item">
        <div class="video-container">
            <iframe 
                src="https://www.youtube.com/embed/9RqDOknEkl8?&autoplay=1&mute=1&loop=1&playlist=9RqDOknEkl8&playsinline=1" 
                title="Yaw Bias in Stabilization Phase (0.75 Action Scale)" 
                frameborder="0" 
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                allowfullscreen>
            </iframe>
        </div>
        <p style="text-align: center; font-size: 0.9em; color: #666;"><strong>Left:</strong> Yaw Aritifact in Stabilization Phase (0.75 Action Scale) </p>
    </div>
    <!-- Right Video: The "Ghosting" Application -->
    <div class="video-item">
        <div class="video-container">
            <iframe 
                src="https://www.youtube.com/embed/FOjMmsoMRs4?&autoplay=1&mute=1&loop=1&playlist=FOjMmsoMRs4&playsinline=1" 
                title="World Frame Evaluation Failure (0.5 Action Scale)" 
                frameborder="0" 
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                allowfullscreen>
            </iframe>
        </div>
        <p style="text-align: center; font-size: 0.9em; color: #666;"><strong>Right:</strong> Failure in Specialized Drills (World Frame) for 0.5 Action Scale Policy </p>
    </div>
</div>

<figure style="text-align: center;">
  <img src="{{ site.baseurl }}/images/action_scale_0.75_scatter.png" alt="General Catch (Action Scale Comparison) Scatter Plot">
  <figcaption style="font-size: 0.9em; color: #666; margin-top: 0.5rem;">
    Fig 4: An extreme version of the artifact (<code>action_scale=0.75</code>). The yaw bias during the initial stabilization angles the robot to the right, causing it to fail on throws from the left.
  </figcaption>
</figure>



#### Expressiveness vs Stability

Here we look at the performance/tradeoffs of increasing action_scale.

[scatter of action_scale cross section]

[plot of eval2, world frame evaluation showing learned bias again across the action_scales]

<div class="video-container">
  <iframe src="https://www.youtube.com/embed/YOUR_TRAILER_VIDEO_ID" title="Project Trailer" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

---

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


