# hf-claude

extension for `hf` to launch Claude Code with [Hugging Face Inference Providers](https://huggingface.co/docs/inference-providers/en/index)

It lets you pick:
- model (from `https://router.huggingface.co/v1/models`)
- provider (`auto` or a concrete provider for the selected model)

Then it runs `claude --model <model[:provider]>` with the required env vars preconfigured.

## Requirements

- Claude Code CLI installed
- `curl`, `jq`, `bash`
- [`fzf`](https://github.com/junegunn/fzf#installation) *(optional)* — enables fuzzy model/provider search; without it the launcher falls back to an arrow-key menu (↑/↓ to move, Enter to select)
- A Hugging Face token, via either:

```bash
curl -LsSf https://hf.co/cli/install.sh | bash
hf auth login        
# or
export HF_TOKEN='hf_...'
```

## Install

```bash
hf extensions install hanouticelina/hf-claude #--force to reinstall the extension 
```

## Run

```bash
hf claude
# or
hf extensions exec claude
```

Forward extra args to Claude Code:

```bash
hf claude --help
hf extensions exec claude -- --help
```

## Web search on HF endpoints

On the HF router, Claude Code's native `WebSearch` tool doesn't return results — it calls Anthropic's search backend, which rejects the HF token used as `ANTHROPIC_API_KEY`. (Native `WebFetch` still works: it fetches URLs directly, no search backend involved.) `hf claude` therefore favors the [Exa MCP](https://exa.ai/mcp) for web search and fetching pages instead.

On first run, if Exa isn't already configured, you'll be prompted to install it — **free, no API key required** (rate-limited; [add your own key](https://dashboard.exa.ai/api-keys) to raise limits). In non-interactive contexts (piped stdin, CI) the prompt is skipped and the model keeps the native tools as a fallback.

To disable the Exa favoring entirely:

```bash
HF_CLAUDE_EXA=0 hf claude
```

