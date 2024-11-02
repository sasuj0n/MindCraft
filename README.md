# Mindcraft 🧠⛏️

Crafting minds for Minecraft with Language Models and Mineflayer!

[Join the discord for support!](https://discord.gg/ZsrAAByEnr)

#### ‼️Warning‼️

This project allows an AI model to write/execute code on your computer that may be insecure, dangerous, and vulnerable to injection attacks on public servers. Code writing is disabled by default, you can enable it by setting `allow_insecure_coding` to `true` in `settings.js`. Enable only on local or private servers, **never** on public servers. Ye be warned.

## Requirements

- [Minecraft Java Edition](https://www.minecraft.net/en-us/store/minecraft-java-bedrock-edition-pc) (up to v1.21.1)
- [Node.js](https://nodejs.org/) (at least v14)
- One of these: [OpenAI API Key](https://openai.com/blog/openai-api) | [Gemini API Key](https://aistudio.google.com/app/apikey) |[Anthropic API Key](https://docs.anthropic.com/claude/docs/getting-access-to-claude) | [Replicate API Key](https://replicate.com/) | [Hugging Face API Key](https://huggingface.co/) | [Groq API Key](https://console.groq.com/keys) | [Ollama Installed](https://ollama.com/download) | [Novita AI API Key](https://novita.ai/settings?utm_source=github_mindcraft&utm_medium=github_readme&utm_campaign=link#key-management)

## Installation

Rename `keys.example.json` to `keys.json` and fill in your API keys, and you can set the desired model in `andy.json` or other profiles.
| API | Config Variable | Example Model name | Docs |
|------|------|------|------|
| OpenAI | `OPENAI_API_KEY` | `gpt-3.5-turbo` | [docs](https://platform.openai.com/docs/models) | (optionally add `OPENAI_ORG_ID`)
| Google | `GEMINI_API_KEY` | `gemini-pro` | [docs](https://ai.google.dev/gemini-api/docs/models/gemini) |
| Anthropic | `ANTHROPIC_API_KEY` | `claude-3-haiku-20240307` | [docs](https://docs.anthropic.com/claude/docs/models-overview) |
| Replicate | `REPLICATE_API_KEY` | `meta/meta-llama-3-70b-instruct` | [docs](https://replicate.com/collections/language-models) |
| Ollama (local) | n/a | `llama3` | [docs](https://ollama.com/library) |
| Groq | `GROQCLOUD_API_KEY` | `groq/mixtral-8x7b-32768` | [docs](https://console.groq.com/docs/models) |
| Hugging Face | `HUGGINGFACE_API_KEY` | `huggingface/mistralai/Mistral-Nemo-Instruct-2407` | [docs](https://huggingface.co/models) |
| Novita AI | `NOVITA_API_KEY` | `gryphe/mythomax-l2-13b` | [docs](https://novita.ai/model-api/product/llm-api?utm_source=github_mindcraft&utm_medium=github_readme&utm_campaign=link) |

If you use Ollama, to install the models used by default (generation and embedding), execute the following terminal command:
`ollama pull llama3 && ollama pull nomic-embed-text`

Then, clone/download this repository

Run `npm install` from the installed directory

Install the minecraft version specified in `settings.js`, currently supports up to 1.21.1

### Running Locally

Start a minecraft world and open it to LAN on localhost port `55916`

Run `node main.js`

You can configure the agent's name, model, and prompts in their profile like `andy.json`.

You can configure project details in `settings.js`. [See file for more details](settings.js)

### Run in docker to reduce some of the risks

If you intent to `allow_insecure_coding`, it might be a good idea to put the whole app into a docker container to reduce risks of running unknown code.

```bash
docker run -i -t --rm -v $(pwd):/app -w /app -p 3000-3003:3000-3003 node:latest node main.js
```
or simply
```bash
docker-compose up
```

When running in docker, if you want the bot to join your local minecraft server, you have to use a special host address `host.docker.internal` to call your localhost from inside your docker container. Put this into your [settings.js](settings.js):

```javascript
"host": "host.docker.internal", // instead of "localhost", to join your local minecraft from inside the docker container
```

To connect to an unsupported minecraft version, you can try to use [viaproxy](services/viaproxy/README.md)

### Online Servers
To connect to online servers your bot will need an official Microsoft/Minecraft account. You can use your own personal one, but will need another account if you want to connect with it. Here are example settings for this:
```javascript
"host": "111.222.333.444",
"port": 55920,
"auth": "microsoft",

// rest is same...
```
‼️ Please make sure your bot's name in the profile.json matches the account name! Otherwise the bot will spam talk to itself.

### Bot Profiles

Bot profiles are json files (such as `andy.json`) that define:

1. Bot backend LLMs to use for chat and embeddings.
2. Prompts used to influence the bot's behavior.
3. Examples help the bot perform tasks.

### Specifying Profiles via Command Line

By default, the program will use the profiles specified in `settings.js`. You can specify one or more agent profiles using the `--profiles` argument:

```bash
node main.js --profiles ./profiles/andy.json ./profiles/jill.json
```

### Model Specifications

LLM backends can be specified as simply as `"model": "gpt-3.5-turbo"`. However, for both the chat model and the embedding model, the bot profile can specify the below attributes:

```json
"model": {
  "api": "openai",
  "url": "https://api.openai.com/v1/",
  "model": "gpt-3.5-turbo"
},
"embedding": {
  "api": "openai",
  "url": "https://api.openai.com/v1/",
  "model": "text-embedding-ada-002"
}
```

The model parameter accepts either a string or object. If a string, it should specify the model to be used. The api and url will be assumed. If an object, the api field must be specified. Each api has a default model and url, so those fields are optional.

If the embedding field is not specified, then it will use the default embedding method for the chat model's api (Note that anthropic has no embedding model). The embedding parameter can also be a string or object. If a string, it should specify the embedding api and the default model and url will be used. If a valid embedding is not specified and cannot be assumed, then word overlap will be used to retrieve examples instead.

Thus, all the below specifications are equivalent to the above example:

```json
"model": "gpt-3.5-turbo"
```
```json
"model": {
  "api": "openai"
}
```
```json
"model": "gpt-3.5-turbo",
"embedding": "openai"
```

## Patches

Some of the node modules that we depend on have bugs in them. To add a patch, change your local node module file and run `npx patch-package [package-name]`
