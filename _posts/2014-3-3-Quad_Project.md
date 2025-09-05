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
For brevity, we focus on the best control policy for this section. The learning problem is defined as this follows,

---

### The Problem Formulation

The control problem is formulated as a standard reinforcement learning task. The policy is trained to map observations from a 52-dimensional space to actions in a 12-dimensional space to maximize a cumulative reward. The specific components are detailed below.

| Component | Description | Dimension |
| :--- | :--- | :---: |
| **Observation Space** | | **52** |
| Root Linear Velocity | The robot's linear velocity in the body frame (`x, y, z`). | 3 |
| Root Angular Velocity | The robot's angular velocity in the body frame (`roll, pitch, yaw`). | 3 |
| Projected Gravity | The gravity vector projected onto the robot's body frame, providing orientation. | 3 |
| Joint Position Error | The deviation of each joint from the default/neutral pose (`q - q_default`). | 12 |
| Joint Velocity | The angular velocity of each joint. | 12 |
| Previous Action | The action taken in the previous timestep, for temporal consistency. | 12 |
| Ball Position | The ball's position relative to the robot's body frame (`x, y, z`). | 3 |
| Ball Velocity | The ball's velocity relative to the robot's body frame (`x, y, z`). | 3 |
| Catch State | A binary flag indicating if a catch has been made in the current episode. | 1 |
| | | |
| **Action Space** | | **12** |
| Target Joint Position Offsets | The policy outputs a vector of target joint position *deviations* from the default pose. This is scaled and added to the default positions to produce the final PD target for the motors. | 12 |
| | | |
| **Reward Structure** | *Note: Terms are modulated across curriculum stages.* | |
| *Task-Specific Rewards* | | |
| `catch_reward` | A large positive reward for successful contact between the ball and the head link. | |
| `proximity_reward` | A shaped reward for near-misses, based on the minimum distance to the ball's trajectory. | |
| `feet_air_time` | Encourages dynamic, athletic motion by rewarding time spent with feet off the ground. | |
| `distance_to_catch_pen` | An exponential penalty on the XY distance to the predicted catch location, encouraging proactive movement towards the ball. | |
| *Standard Motion Penalties* | | |
| `action_rate_pen` | Penalizes the difference between consecutive actions to promote smoothness. | |
| `projected_gravity_pen` | Penalizes deviation from an upright posture to maintain stability. | |
| `undesired_contact_pen` | Penalizes contact between the ground and any part of the robot other than the feet. | |
| `joint_torque_pen` | Penalizes high joint torques to encourage energy efficiency and smoother motions. | |
| `joint_accel_pen` | Penalizes high joint accelerations to reduce jerky movements and wear. | |

#### Problem

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


