# bgit - Version Control for AI Models

Git-native interface for [Bread SDK](https://github.com/Bread-Technologies/Bread-SDK-Bake-Repo) that makes AI model baking feel like regular software development.

## What is bgit?

**bgit** wraps the Bread SDK in a git repository structure, turning model training into a natural git workflow:

- 🎯 **One file = One model**: Your entire configuration in a YAML file
- 📝 **Commits = Versions**: Track every model iteration
- 🌿 **Branches = Experiments**: Test ideas safely in parallel
- 🚀 **Run = Bake**: Run operations manually with `bgit run`
- 📊 **README = History**: Auto-generated documentation
- 💬 **Chat = Test**: Chat with your baked models using `bgit chat`

Think of it as **version control for your AI's mind**.

---

## Installation

### Prerequisites

- Node.js 16+ and npm
- Git
- Bread API key ([contact Bread](mailto:contact@aibread.com))

### Install from source

```bash
# Clone this repository
git clone https://github.com/Bread-Technologies/bread_sdk_git_interface.git
cd bread_sdk_git_interface

# Install dependencies
npm install

# Build the project
npm run build

# Link globally (for development)
npm link
```

**Note**: The `bread-sdk-v1` dependency will be automatically installed from GitHub when you run `npm install`.

---

## Quick Start

### 1. Create Your First Model

```bash
bgit init yoda-model
```

You'll be prompted for:
- Your Bread API key (validated automatically)
- The API key will be saved to `~/.bgit/config.json` for future use

**What gets created:**

```
yoda-model/
├── yoda-model.yml    # Your model configuration (YAML format)
├── README.md         # Auto-generated documentation
├── .bread            # Internal Bread SDK mapping (auto-generated)
├── .gitignore        # Git ignores (Node.js + security)
└── .git/             # Git repository with pre-commit hook
```

### 2. Configure Your Model

```bash
cd yoda-model
```

Open `yoda-model.yml` in your editor and customize:

- `REPO.name`: Repository name on Bread platform
- `REPO.base_model`: Base model to use
- `PROMPT`: Teacher and student prompts
- `TARGET`: Training data generation configuration
- `BAKE`: Bake settings and datasets

### 3. Run Your First Bake

```bash
# Stage your changes
bgit add .

# Commit your configuration
bgit commit -m "Initial Yoda personality: inverted syntax, wise tone"

# Run the operations (stim → rollout → bake)
bgit run stim rollout bake
```

This will:
1. Update prompts, targets, and bakes on the Bread platform
2. Run stim operation (generate questions)
3. Run rollout operation (generate responses)
4. Run bake operation (train the model)
5. Update your YAML file with status and model names

Your terminal shows real-time progress. Baking takes 30-60 minutes.

### 4. Check Status

```bash
# Check operation status
bgit status
```

This shows the current status of stim, rollout, and bake operations.

### 5. View Outputs

```bash
# View stim outputs
bgit stim

# View stim outputs with limit
bgit stim --limit 10

# View rollout outputs
bgit rollout

# View rollout outputs with limit
bgit rollout --limit 10
```

### 6. Chat with Your Model

Once your bake is complete, you can chat with your baked model:

```bash
# Chat with a baked model (use the model name from bake status)
bgit chat <model-name>
```

For example:
```bash
bgit chat ayush27/wolfram/v1_46d6d9fc2dfc34d6/18
```

The chat interface supports:
- `/help` - Show available commands
- `/clear` - Clear conversation history
- `/system <prompt>` - Set system prompt
- `/status` - Show current settings
- `/quit` - Exit chat

---

## How It Works

### The Git Workflow

```
Edit YAML → Commit → Run Operations → Check Status → Chat
```

Every change to your YAML file represents a new model version. Your commit messages document what changed.

### What the YAML File Contains

```yaml
REPO:
  name: my_model
  base_model: Qwen/Qwen3-32B

PROMPT:
  teacher:
    messages:
      - role: system
        content: "You are Yoda. Speak like Yoda..."

TARGET:
  generators:
    - type: oneshot_qs
      model: claude-sonnet-4-5-20250929
      numq: 100

BAKE:
  name: v1
  datasets:
    - target: main_target
      weight: 1.0
```

The template includes a complete working example (Yoda personality).

---

## Available Commands

### `bgit init <model-name>`
Initialize a new Bread model repository. Creates the directory structure, YAML config, and git repository.

### `bgit add [files...]`
Stage files for commit (wraps `git add`). The pre-commit hook automatically updates `.bread` file when YAML files change.

### `bgit commit [options]`
Commit changes (wraps `git commit`). The pre-commit hook ensures `.bread` is up to date.

### `bgit status`
Show the current status of stim, rollout, and bake operations with progress indicators.

### `bgit run <operations...>`
Run one or more operations: `stim`, `rollout`, or `bake`. You can run them individually or together:
```bash
bgit run stim
bgit run rollout
bgit run bake
bgit run stim rollout bake  # Run all in sequence
```

### `bgit stim [--limit <number>]`
View stim outputs. Use `--limit` to limit the number of outputs displayed.

### `bgit rollout [--limit <number>]`
View rollout outputs. Use `--limit` to limit the number of outputs displayed.

### `bgit chat <model-name>`
Start an interactive chat session with a baked model. The model name is typically in the format: `username/repo/bake_name/model_id`.

---

## Workflow Examples

### Iterating on Your Model

```bash
# Make changes to your YAML file
vim yoda-model.yml  # or open in your IDE

# Review what changed
git diff yoda-model.yml

# Stage and commit
bgit add .
bgit commit -m "Increased temperature to 0.8 for more creativity"

# Run operations
bgit run stim rollout bake

# Check status
bgit status
```

Your commit message documents what changed. Be descriptive!

### Experimenting with Branches

```bash
# Create experiment branch
git checkout -b experiment/formal-tone

# Edit YAML for your experiment
vim yoda-model.yml

# Commit and run
bgit commit -am "Experiment: formal business tone"
bgit run stim rollout bake

# Push to remote
git push -u origin experiment/formal-tone
```

Compare experiments:

```bash
git diff main..experiment/formal-tone yoda-model.yml
```

Merge successful experiments:

```bash
git checkout main
git merge experiment/formal-tone
bgit run stim rollout bake
git push origin main
```

### Time Travel

```bash
# See all commits
git log --oneline

# Go back to a previous version
git checkout <commit-hash>

# Or checkout the file only
git checkout <commit-hash> yoda-model.yml
```

### Tagging Milestones

```bash
# Tag a successful production model
git tag -a v1.0 -m "Production: Yoda personality baseline"
git push --tags

# Roll back to a tag
git checkout v1.0
```

---

## Advanced Usage

### Multiple Models

Each model should be its own independent git repository:

```bash
cd ~/my-projects/

bgit init yoda-model
bgit init code-assistant
bgit init support-bot
```

Each has its own git history, branches, and configuration.

### Team Collaboration

Works like any git repository:

```bash
# Pull teammate's changes
git pull origin main

# See what they changed
git log

# Review their bake results
bgit status
```

### API Key Management

The API key is stored in `~/.bgit/config.json` or can be set via environment variable:

```bash
# Set via environment variable
export BREAD_API_KEY=your-api-key-here

# Or it will be saved automatically after first use
```

---

## Understanding the Template

The generated YAML file includes a complete Yoda personality example:

**Teacher Prompt**: What behavior to bake into the model
```yaml
PROMPT:
  teacher:
    messages:
      - role: system
        content: "You are Yoda. Speak like Yoda, use inverted syntax..."
```

**Student Prompt**: Trigger at inference (empty = always-on)
```yaml
PROMPT:
  student:
    messages:
      - role: system
        content: ""  # Empty = model ALWAYS acts like Yoda
```

**Generators**: How to create training questions
- `oneshot_qs`: AI-generated questions for diversity
- `hardcoded`: Specific test questions

**Result**: A model that speaks like Yoda without needing system prompts at inference (zero tokens!).

Customize this template for your use case.

---

## Troubleshooting

### `bgit: command not found`

The package isn't installed or linked. From this repo:

```bash
npm install
npm run build
npm link  # or use npx bgit
```

### `Invalid API key`

Check your API key:

```bash
# Check environment variable
echo $BREAD_API_KEY

# Check config file
cat ~/.bgit/config.json
```

Or re-run `bgit init` and enter a valid key when prompted.

### `.bread file not found`

The `.bread` file is auto-generated by the pre-commit hook. If it's missing:

```bash
# Manually update it
npx bgit update-bread $(pwd)
```

### Operations fail

Check the error output. Common issues:

- Invalid prompt names in YAML
- Incorrect repo name
- API rate limits
- Network connectivity issues

Check status with `bgit status` to see what failed.

### Chat not working

Make sure:
- The model name is correct (check with `bgit status` after a successful bake)
- The API key is valid
- You have network connectivity to the Bread API

---

## File Structure Reference

### In this repo (bgit package)

```
bread_sdk_git_interface/
├── package.json              # Node.js package definition
├── tsconfig.json            # TypeScript configuration
├── source/                  # Source code
│   ├── cli.tsx              # Main CLI entry point
│   ├── cli/
│   │   └── commands/        # Command implementations
│   │       ├── init.tsx     # Initialize repository
│   │       ├── git.ts       # Git wrappers (add, commit)
│   │       ├── run.tsx      # Run operations
│   │       ├── status.tsx   # Check status
│   │       ├── outputs.tsx  # View outputs
│   │       └── chat.tsx     # Chat with models
│   ├── config/              # Configuration management
│   ├── sdk/                 # Bread SDK client wrapper
│   ├── git/                 # Git utilities
│   └── templates/           # Template files
│       ├── my_repo.yml.template
│       ├── README.md.template
│       ├── .gitignore.template
│       ├── .bread.template
│       └── pre-commit.template
└── dist/                     # Compiled JavaScript (after build)
```

### In each model repo (after `bgit init`)

```
your-model/
├── your-model.yml    # Your model config (edit this)
├── README.md         # Auto-generated documentation
├── .bread            # Internal mapping (auto-generated)
├── .gitignore        # Git ignores
└── .git/             # Git repository
    └── hooks/
        └── pre-commit # Auto-updates .bread file
```

---

## Philosophy

### Why Git for AI Models?

AI model training has the same version control needs as software:

- Track changes over time
- Experiment safely
- Collaborate with teams
- Roll back mistakes
- Document decisions

Git already solves these problems perfectly. **bgit** makes git's power available for AI.

### Design Principles

1. **Minimal friction**: Install once, use standard git commands
2. **Transparency**: No hidden state, everything in git history
3. **Control**: Run operations manually when you're ready
4. **Familiarity**: Git workflows developers already know
5. **Independence**: Each model is its own repo with its own history

---

## Development

### Building

```bash
# Install dependencies
npm install

# Build TypeScript
npm run build

# Watch mode for development
npm run dev
```

### Testing

```bash
# Run tests
npm test

# Lint code
npm run lint

# Format code
npm run format
```

---

## Contributing

This is an early version. Feedback and contributions welcome!

**Found a bug?** Open an issue.
**Have an idea?** Open a pull request.
**Need help?** Check the [Bread SDK documentation](https://github.com/Bread-Technologies/Bread-SDK-Bake-Repo).

---

## Related Projects

- [Bread SDK](https://github.com/Bread-Technologies/Bread-SDK-Bake-Repo) - The underlying SDK
- [Bread Documentation](https://docs.bread.com.ai) - Official docs

---

## License

MIT

---

## Support

- **Email**: contact@aibread.com
- **GitHub Issues**: [Create an issue](../../issues)
- **Documentation**: [Bread SDK Repo](https://github.com/Bread-Technologies/Bread-SDK-Bake-Repo)