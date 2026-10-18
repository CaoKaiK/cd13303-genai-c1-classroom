You have three options for setting up your environment to run this project. Please read through them and choose the one that works best for you.

Option 1: Provided Workspace (Recommended)
This is the most recommended option. The environment is pre-configured with a T4 Tesla GPU (16GB VRAM) and all necessary dependencies, significantly reducing setup time.

Accessing the Workspace
Follow these steps to access and run the notebook.

Access the Vocareum workspace from the cloud resources tab.
In the file explorer on the left, navigate to the following directory: project/starter/
Double-click the gen_ai_fundamentals_project_starter.ipynb file to open it.
Click "Select Kernel" in the top-right corner.
Select Jupyter Kernel from the dropdown menu options.
Select Python (venv2) from the list.
If the kernel is not listed, click the refresh icon at the top of the kernel menu.
You are now ready to run the code!

To execute terminal commands, activate the pre-configured virtual environment:

source /voc/data/venv2/bin/activate
If something goes wrong in the workspace
RuntimeError: Duplicate layer name — the model didn't fully unload from memory after a crash or interruption.

Click the Restart button in the notebook toolbar (circular arrow icon next to "Interrupt").
Wait for the kernel to come back up and check the top-right corner still shows Python (venv2).
Run all cells again from the top.
Click Restart, not End Lab — ending the lab closes your whole GPU session.

NotImplementedError: Unsloth cannot find any torch accelerator — the GPU is fine; the notebook is running in the wrong Python environment. Click "Select Kernel" → Jupyter Kernel → Python (venv2), then re-run the failing cell.

ModuleNotFoundError: No module named '...' — also the wrong kernel; switch to Python (venv2).

Don't see Python (venv2)? Click the refresh icon at the top of the kernel menu. If it's still missing, open a terminal and run:

source /voc/data/venv2/bin/activate
python -m ipykernel install --user --name=venv2 --display-name "Python (venv2)"
Then refresh the kernel list and select Python (venv2).

Restart vs. End Lab vs. Reset

Restart (notebook toolbar) clears the kernel's memory. Your files and training checkpoints stay.
End Lab stops the GPU session and your budget use. Your files are kept for your next session.
Reset erases your workspace and restores the original files. It cannot be undone — download your notebook first (right-click it in the file explorer → Download).
More help: the Workspace Setup and Troubleshooting page in Applications of Generative AI.

Option 2: Google Colab (Not Provided)
Google Colab offers a flexible and accessible option, available directly through a web browser. It provides access to a capable T4 GPU suitable for this project, and managing dependencies is straightforward.

The main considerations are the need to manually enable the T4 GPU, potential session limits or interruptions, and the possibility of resource unavailability during peak times.

For this option, you must download the project/starter notebook first. You will also need to:

Upload the notebook to your Google Drive or Colab.
Manually enable the T4 GPU via Runtime > Change runtime type > Hardware accelerator > T4 GPU.
Upload requirements.txt from the project/ folder (one level above starter/) next to the notebook, then install the pinned packages in a notebook cell with !pip install -r requirements.txt --no-deps. Keep --no-deps: without it, pip upgrades Triton to a version that crashes on the T4 GPU.
Option 3: Local or Cloud GPU (Not Provided)
This option provides the greatest control and customization but requires the most technical expertise. You have full control over your environment and resources, with no external session limits for local GPUs and scalability for cloud options.

The significant drawbacks include complex setup involving installing drivers (NVIDIA driver & CUDA) and dependencies, potential costs for cloud GPUs, and the need for troubleshooting environment issues. This project requires an NVIDIA GPU with at least 16GB of VRAM.

For this option, you will also need to download the project/starter notebook first.

Local Setup Steps
If you choose this option, follow these steps to set up your local environment:

Clone the Project:
   git clone https://github.com/udacity/cd13303-genai-c1-classroom.git
   cd cd13303-genai-c1-classroom/project
Install uv (Python Package Manager): The project uses uv for fast and reliable dependency management.
   curl -LsSf [https://astral.sh/uv/install.sh](https://astral.sh/uv/install.sh) | sh
(You may need to restart your terminal after installation.)

Install Python Headers:uv requires Python development headers.
   # This command is for Ubuntu/Debian. Use the equivalent for your OS.
   sudo apt update && sudo apt install python3-dev -y
Create Virtual Environment & Install Packages: From the project/ folder, run the following uv commands. This will create a .venv folder in your directory, pin the correct Python version, and install all packages from requirements.txt.
   uv python pin 3.12.3
   uv init
   uv sync
   uv add ipykernel pip
   uv pip install -r requirements.txt --no-deps
Launch Jupyter: Once installed, launch Jupyter Lab and open the project/starter/gen_ai_fundamentals_project_starter.ipynb notebook.
   jupyter lab
When prompted, select the .venv kernel.