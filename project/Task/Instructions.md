Your main task is to complete the project/starter/gen_ai_fundamentals_project_starter.ipynb notebook. Follow the steps below, which correspond to the "TODO" sections in the notebook. Section names in bold match the headings in the notebook.

Phase 1: Project Setup
Select the Python (venv2) kernel, then run the cells under Set up the notebook. They load ipython-autotime (to time each cell) and check your GPU memory with nvidia-smi. In the Vocareum workspace, every other package is already installed in venv2; on Colab or a local GPU, install them first from project/requirements.txt (see Environment Setup).
Task (first TODO cell — load the model): Load the Qwen2.5-3B-Instruct model.
Determine and set an appropriate lora_rank value. A good starting point is 64.
Choose the target_modules for LoRA. You should target all the key linear layers in the attention and MLP blocks ("q_proj", "k_proj", "v_proj", "o_proj", "gate_proj", "up_proj", "down_proj").
Explain your choices in the code comments.
Phase 2: Prompt Engineering Baseline
Under Try Prompt Engineering to Count Letters, run the first cell (blank system prompt) to see the model's (poor) baseline performance.
Task (the next TODO cell — SYSTEM_PROMPT****): Develop a new SYSTEM_PROMPT.
It must incorporate Chain-of-Thought (CoT) prompting, telling the model to think step-by-step.
It must include at least one few-shot example (like the "room" example) for the model to follow.
Run the cell again. You should see a much-improved (though still not perfect) response. This motivates the need for RL fine-tuning.
Phase 3: Dataset Creation
Under Create a letter-counting dataset:

Run the cells to create the ALL_WORDS list and the generate_records function.
Run the ds.map cell to format the dataset with your SYSTEM_PROMPT.
Run the final cell in this section to see how the (untuned) model performs on a sample from the dataset.
Phase 4: Building the Reward Functions (Core Task)
This is the most critical part of the project. You will implement the logic that teaches the model what a "good" answer is. Each function has its own heading under Create Reward Functions.

Task (Numbering reward function): numbering_reward_func
Complete the logic to reward in-order numbering (e.g., +0.5), penalize out-of-order numbering (e.g., -0.5), and penalize for continuing beyond the word's length (e.g., -1.0).
Task (Spelling reward function): spelling_reward_func
Complete the logic to:
Reward exactly correct spelling (e.g., +2.0).
Penalize for differences in length (e.g., -0.5 per letter).
Penalize for extra letters (e.g., -1.0 per letter).
Penalize for missing letters (e.g., -0.5 per letter).
Task (Counting reward function): counting_reward_func
Complete the if/else block to reward an accurate running total at each step (e.g., +1.0) and penalize an inaccurate one (e.g., -1.0).
Complete the final res.append(...) line to normalize and scale the reward.
Task (Formatting reward functions): format_reward_func
Complete the logic to:
Reward the model for using the correct <reasoning>...</reasoning><answer>...</answer> format (e.g., +0.5).
Reward the model if the extracted answer is a digit (e.g., +0.5).
Task (Task correctness reward function): correct_answer_reward_func
Complete the list comprehension to provide a strong positive reward for a correct final answer (e.g., 2.0) and a negative reward for an incorrect one (e.g., -1.0).
Phase 5: Model Training
Task (Train the model — COMMON_GRPO_TRAINING_PARAMS****): Fill in the GRPO parameters.
Fill in values for learning_rate, beta, per_device_train_batch_size, num_generations, and gradient_accumulation_steps.
Refer to the provided documentation links for guidance. (Good defaults are 10e-6, 0.0001, 16, 4, and 1).
Also change save_steps=250 to save_steps=20*****, and add save_total_limit=2, on the line below it.* With save_steps=250, no checkpoint is written during an 80–100-step run, so a dropped session loses the whole run. With these values, a checkpoint is saved in outputs/ every 20 steps and only the two most recent are kept (each can take around 1 GB of disk).
Task (Quick train): Run the Quick Train (5 steps).
Analyze the log table output. Are the reward functions working? You should see non-zero values.
Note how long it took — you'll use it to size the longer run.
Task (Slower train): Run the longer training.
Set max_steps for a longer run (e.g., 80 to 100 steps). Expect it to take about an hour on the workspace's T4 GPU; scale your Quick Train time up to estimate.
Execute the training and observe the log table. Over the run, the reward and rewards/correct_answer_reward_func/mean columns should trend upwards.
Run the cell below it to plot your training rewards.
Your rewards will be noisy — that's expected. With the small batches that fit on a T4, rewards jump around from step to step, and in an 80–100-step run the upward trend can be weak. Judge the overall direction, not individual steps. A rolling average makes it easier to see — add this below the plot:


log_df["rewards/correct_answer_reward_func/mean"].rolling(10).mean().plot()
If your trend is flat or unstable, you can still meet the rubric, which accepts a run where you explain the anomalies with evidence and attempt a remedial configuration. In a markdown cell after your plot:

Show the evidence: the plot or the log table of the correctness reward.
Explain what you think happened (for example: learning rate too high or too low, too few steps, or a reward function that never fires).
Change one setting, re-run, and report what changed. The remedial run can be short — about 20–30 steps is enough to show the effect; you don't need to repeat the full run.
If your session stops during the longer training
Checkpoints in outputs/ survive a kernel Restart and End Lab, but not Reset.

If the lab has ended, start it again, open the notebook and select Python (venv2).
Run every cell from the top down to and including the COMMON_GRPO_TRAINING_PARAMS cell. Skip the Quick Train.
In the Slower train cell, keep the same max_steps and change the last line to:

trainer_res = trainer.train(resume_from_checkpoint=True)
Run it. Training picks up from the most recent checkpoint, and the plot cell afterwards still shows the whole run.

Never click Reset without downloading your notebook first (right-click it in the file explorer → Download). Reset erases your work, and it can't be recovered.

Phase 6: View the Results
Under View the results, run the first two cells to save your LoRA adapter and define the compare_old_and_new_model function.
Task (Compare the old and new models on the letter-counting task):
Run the cell to load the first dataset item.
Observe the "OLD" (untuned) vs. "NEW" (your tuned model) output. The "NEW" model should now follow the reasoning steps and get the correct answer.
Task (Make sure the model did not forget basic facts): Check for Catastrophic Forgetting.
Write a general knowledge question (e.g., "What is the capital of the Philippines?").
Run the compare_old_and_new_model function with your question.
Verify that both the "OLD" and "NEW" models answer correctly, proving that your fine-tuning taught a new skill without erasing the model's existing knowledge.
Once all cells are run and all outputs are visible, you are ready to submit your project!