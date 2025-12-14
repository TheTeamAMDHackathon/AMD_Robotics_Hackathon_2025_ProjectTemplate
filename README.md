# AMD_Robotics_Hackathon_2025_LeBobLeBuilder

## Team Information

This document constitutes the formal submission for the AMD Open Robotics Hackathon held in Paris, December 12-14, 2025. Our team is proud to present "LeBobLeBuilder," a project that demonstrates rapid, AI-driven robotics development by leveraging AMD AI solutions from edge to cloud and the Hugging Face LeRobot framework, directly aligning with the hackathon's core vision.

**Team:**

Number: 12
Name: TheTeam
Members: 
- Adrien Bedel
- Alexandre Fugier
- Pierre Sudron
- Xavier Bureau

What follows is a comprehensive overview of our project, detailing its mission, technical implementation, and innovative approach.

**Summary:**

This summary encapsulates our project's core vision and the tangible outcome we achieved. Our team's initial ambition was the creation of "Le Playmate," an interactive robotic game companion. However, to deliver a robust result within the hackathon's demanding timeframe, we executed a strategic pivot to focus on a foundational and challenging robotics task: precision construction. This decision allowed us to concentrate our efforts on mastering the core imitation learning pipeline and delivering a polished, demonstrable result.

The final project, "LeBobLeBuilder," successfully trains a Wowrobot SO-101 robotic arm to autonomously build a three-floor Kapla tower. The robot methodically picks individual blocks from a designated feeder and precisely stacks them, showcasing a high degree of learned dexterity and spatial awareness.

[Insert a compelling image or GIF of the final robot successfully building the Kapla tower here]

This report breaks down our submission, from the real-world applications of our mission to the technical architecture that brought it to life.

*< Images or video demonstrating your project >*

## Submission Details

### 1. Mission Description

The task of building a Kapla tower was designed to test the limits of robotic perception, planning, and manipulation. This section analyzes how our solution serves as a proof-of-concept for real-world automation, directly addressing the hackathon's "Real-world use case" criterion.

The precision and delicate handling required to stack Kapla blocks are directly transferable to a wide range of industrial and commercial applications, including automated modular assembly, delicate material handling in logistics and pharmaceuticals, and advancements in construction automation. "LeBobLeBuilder" demonstrates that sophisticated, AI-driven robotic dexterity can be achieved with affordable hardware and open-source frameworks. Crucially, by running real-time inference on the provided Dell Pro Max 16 laptop with its Ryzen™ AI 9 HX 370 processor, our project proves that such sophisticated automation is viable on accessible edge devices, not just on proprietary, large-scale industrial machinery.

This real-world potential is enabled by the creative and innovative approach our team adopted for data collection and system design.

### 2. Creativity

This section details the novel aspects of our methodology, engineered to satisfy the "Creativity" judging criterion. Our innovation lies not in a single component but in a holistic, data-centric strategy designed for maximum training efficiency and policy robustness.

Our most unique strategic choice was the modular dataset approach. Instead of attempting to capture the entire complex construction task in single, lengthy demonstrations, we deconstructed it. We created distinct, focused datasets for each of the three floors and, critically, for common error recovery scenarios. This method provided decisive advantages in training efficiency and model robustness by allowing us to generate high-quality, targeted data for each sub-task.

This data-centric strategy was complemented by a three-camera setup, providing the policy with rich, multi-perspective visual data streams essential for resolving spatial ambiguities that a single-camera setup cannot overcome.

* Top Camera: Provided global awareness of the workspace, allowing the model to understand the overall layout of the feeder, the construction zone, and the robot's position.
* Side Camera: Offered precise altitude awareness, which was critical for accurately tracking the current construction floor and ensuring correct vertical placement of each block.
* Grip Camera: Enabled fine-grained control for the most delicate parts of the task, such as precisely aligning with a block, confirming a successful grasp, and making micro-adjustments during placement.

Together, these innovations in data strategy and sensory input create a highly effective and scalable foundation that can be extended to more complex manipulation tasks.

### 3. Technical implementations

This section provides a comprehensive, step-by-step breakdown of our technical architecture and workflow. We engineered our solution on the integrated hardware and software ecosystem provided, ensuring a seamless and rapid development pipeline from cloud-based training to real-time edge inference.

#### 3.1 Hardware & Software Stack

We leveraged the full end-to-end stack provided by AMD and its partners, enabling a seamless development pipeline from data capture to autonomous execution.

| Component	| Description |
| --- | --- |
| Robotics Kit | Wowrobot SO-101 fully assembled kit, used as the robotic arm. |
| Edge Compute	| Dell Pro Max 16 Laptop with a Ryzen™ AI 9 HX 370 processor for running the final inference application. |
| Cloud Training | AMD Developer Cloud, providing access to high-performance AMD Instinct™ MI300X GPUs for model training. |
| Core Framework | Hugging Face LeRobot, the primary framework for implementing the imitation learning pipeline. |
| ML Library | PyTorch, as the foundational deep learning library used by LeRobot. |
| GPU Software	| AMD ROCm™, the open-software platform enabling GPU-accelerated training. |
| Experiment Tracking | Weights & Biases, used to log and monitor training runs, with results included in the submission. |

#### 3.2 Teleoperation & Dataset Capture

The foundation of our imitation learning approach was high-quality demonstration data. We used the SO-101 leader arm in a teleoperation setup to manually control the follower arm, recording its movements and the multi-camera video feeds.

As detailed in our innovation strategy, we captured separate, modular datasets for each sub-task. These were later merged into a single, comprehensive dataset for training. The final merged dataset, [BobLeBuilder_MergedDataset](https://huggingface.co/datasets/LeTeamAMDHackhaton/BobLeBuilder_MergedDataset).
To allow merging those through LeRobot framework, we optimized camera recording resolution and framerate.

[Insert image or short video of the teleoperation/dataset capture process here]

#### 3.3 Training

The model training phase was conducted on the AMD Developer Cloud. We leveraged the computational power of the AMD Instinct™ MI300X GPUs to accelerate the training process, allowing us to iterate quickly and fine-tune our policy using the merged dataset.
While we tried smolVLA, we settled for ACT as the base model as it offered the best ROI between training time and task accuracy.
For full reproducibility, the complete training logs are provided in the wandb directory of our code repository.

#### 3.4 Inference

We deployed the final trained policy to the AMD Ryzen™ AI-powered Dell laptop. This edge device performed real-time inference, processing the live feeds from the three cameras and sending control commands to the SO-101 robot. This enabled the robot to autonomously execute the full three-floor Kapla construction task, demonstrating a complete and efficient "cloud-to-edge" workflow.
Thanks to being built on the relatively small ACT as well as decoupling the logic between client and server as to infer in batch, it performed smoothly without junky movements.

[Insert image or short video of the robot performing inference and stacking blocks here]

### 4. Ease of Use & Generalizability

Our solution is engineered for adaptability, directly targeting the "Ease of Use" criterion. The modular dataset architecture is designed to help generalize beyond this specific task.

The system is not hard-coded to build one structure.
Each layer was trained with multiple starting positions to allow interpolation and thus handling of various configurations.
Furthermore, to teach the robot a new task—such as building a four-floor tower or stacking different objects—a user would simply need to record a new set of modular demonstration datasets for that task. These new modules could then be used to fine-tune the model for the new skill without retraining the entire system from scratch.

From a user-interaction perspective, the solution is designed for simplicity. Once the environment is set up and the inference script is initiated, the robot operates fully autonomously. It requires no complex commands or manual interventions to complete its construction task, making it highly accessible.

Our project successfully combines a creative, scalable architecture with a straightforward user experience, demonstrating a powerful and adaptable solution.

### 5 Project Artefacts & Additional Links

This section provides direct access to all key project components, ensuring that the judges and the wider community can fully explore and reproduce our work. Our code, datasets, and models are hosted on Hugging Face, complemented by a video demonstration of the final result.

* Video Demonstration: [Link to a video of the robot performing the full 3-floor construction task]
* Hugging Face Datasets: 
    * [Final dataset BobLeBuilder_MergedDataset](https://huggingface.co/datasets/LeTeamAMDHackhaton/BobLeBuilder_MergedDataset)
    * [Layer 2 dataset](https://huggingface.co/datasets/LeTeamAMDHackhaton/BobLeBuilder_Subset2)
    * [Layer 1 dataset](https://huggingface.co/datasets/LeTeamAMDHackhaton/BobLeBuilder_Subset1)
    * [Layer 0 dataset](https://huggingface.co/datasets/LeTeamAMDHackhaton/BobLeBuilder_Subset0)
* [LeBobLeBuilder Trained Policy](https://huggingface.co/LeTeamAMDHackhaton/BobLeBuilder)

The following section details the structure of our code submission repository.

### 6 Code submission

This is the directory tree of this repo, you need to fill in the `mission` directory with your submission details.

```terminal
AMD_Robotics_Hackathon_2025_ProjectTemplate-main/
├── README.md
└── mission
    ├── code
    │   └── <code and script>
    └── wandb
        └── <latest run directory copied from wandb of your training job>
```

**NOTES**

1. The `latest-run` is the soft link, please make sure to copy the real target directory it linked with all sub dirs and files.
2. Only provide (upload) the wandb of your last success pre-trained model for the Mission.
