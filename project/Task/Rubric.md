Use this project rubric to understand and assess the project criteria.

Model Setup
Criteria	Submission Requirements
Configure the LoRA model with valid parameters.

The submission includes code that applies LoRA (e.g., via PEFT) to the chosen base LLM.
LoRA hyperparameters are explicitly set and valid
`lora_rank` one of 8, 16, 32, 64, 128
target modules includes `q_proj`, `k_proj`, `v_proj`, `o_proj` and/or `gate_proj`, `up_proj`, `down_proj`
The configuration is used to successfully instantiate a trainable model without runtime errors prior to training.
Baseline Prompting
Criteria	Submission Requirements
Establish a baseline solution using prompting only (CoT with one example).

The submission demonstrates a single-example Chain-of-Thought (CoT) prompt attempting the letter-counting task without additional fine-tuning.
At least one concrete input–output example (one-show) is shown, with the model’s reasoning (step-by-step) visible in the output.
Reward Design & Validation
Criteria	Submission Requirements
Implement and validate reward functions for the task.

The submission defines rewards that are positive for correct/desired behavior and negative for incorrect/undesired behavior.
Rewards cover, at minimum, the following aspects: numbering, spelling, counting, formatting, correctness
The validation at the ends of the cells demonstrates that the reward(s) have a higher value for a correct sample and a lower value for an incorrect sample.
Training & Monitoring
Criteria	Submission Requirements
Execute a longer training session and monitor progress.

A longer training run is executed (more steps/epochs than the quick pass).
The submission reports mean correctness reward over time (table, log, or plot).
The mean correctness reward shows an increasing trend over training (as required in the instructions), or the learner explains anomalies with evidence (e.g., unstable runs, GPU limits) and attempts a remedial configuration.
Final Comparison
Criteria	Submission Requirements
Demonstrate the baseline model and the fine-tuned model on the letter-counting task.

The submission shows both the fine-tuned and the original (pretrained) model on at least one example from the project dataset.
