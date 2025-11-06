<div align="center">

# Pepper

</div>

<h3 align="center">
Your Personal AI Assistant (<a href="https://rllm-project.com/post.html?post=pepper.md"><b>Blog</b></a>)
</h3>

Pepper is a personal AI assistant that proactively works for you. It connects to your Gmail to autonomously summarize important emails and surface critical updates that need your attention, delegating complex tasks to a swarm of workers that handle them seamlessly in the background.



## Setup

First clone the repo:
```bash
git clone --recurse-submodules https://github.com/agentica-org/pepper
```

**Prerequisites:** Install and run the Context Store locally before launching Pepper.


```bash
conda create -n pepper python=3.12 pip -y
conda activate pepper

# First, install our context store Episodic
cd episodic-sdk
pip install -e .[semantic]

# Then, install requirements for Pepper
cd ../pepper
pip install -r requirements.txt
```

followed by:
```bash
episodic serve --port 8000
```

Keep the process running, and open a new terminal for the following setup.

### Configure Environment
1. Copy the environment template:
   ```bash
   cd pepper
   cp env_var.example.sh env_var.sh
   ```

2. Set up Composio (required):
   - Sign in at [composio.dev](https://composio.dev)
   - Create an Project API key
   - Fill in it as `COMPOSIO_API_KEY`
   
3. Fill in your LLM API key (required):
   - For OpenAI (default): Set `OPENAI_API_KEY`
   - For Anthropic: Set `ANTHROPIC_API_KEY`

4. Configure LLM provider (optional):
   - By default, Pepper uses OpenAI models
   - To use Anthropic models:
     - Add to your `env_var.sh`: `export LLM_PROVIDER="anthropic"`
     - Update the `MODEL` variable in `pepper/agent/scheduler.py`, `pepper/agent/worker.py`, and `pepper/agent/workflow.py`
     - Example: `MODEL = "claude-sonnet-4-5-20250929"`

5. Load environment variables:
   ```bash
   source env_var.sh
   ```

6. Login your Gmail account and grant access [Only for the first time]:

   ```bash
   cd ~/pepper
   python -m pepper.services.email_service
   ```
   If you see the message "Please authorize Gmail by visiting: url", open the url in your browser and grant access.

   Once you've see the message "✅ Trigger subscribed successfully.", Ctrl+C to stop the process, you're good to go!

## Running Pepper

Launch pepper (The first time you run this, it'll take a while to build your profile, approximately 1 minute):
```bash
python -m pepper.launch_pepper
```

Open the UI in your browser as prompted:
```
http://localhost:5050/pepper/ui.html
```

If you're in the remote server, vscode should be able to port forward the correct port to your local machine automatically.

**Note:** Press Ctrl+C to stop all services.

## Acknowledgement
This work is done with the [Agentica](https://agentica-project.com/index.html) Team as part of [Berkeley Sky Computing Lab](https://sky.cs.berkeley.edu/). 
