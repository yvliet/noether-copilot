# Copilot for Noether

Native AI assistant with local LLM integration, multi-provider BYOK, document context grounding, and interactive chat.

---

## 1. Overview & User Experience

**Copilot** is an intelligent assistant embedded directly inside your workspace. It connects to local offline inference runtimes (such as Ollama or LM Studio) or cloud AI providers (OpenAI, Anthropic, Google Gemini) to assist with drafting, summarizing, brainstorming, and querying your vault knowledge.

### Where It Lives in Noether
- **Right Sidebar Tab**: Click the sparkle icon in the right sidebar to open the conversational Copilot drawer.
- **Editor Inline Menu**: Highlight any text in your active note and press `/ai` to open the quick transformation popover.
- **Status Bar Indicator**: Shows active model status and token latency metrics.

## 2. Features & Step-by-Step Guide

### 1. Conversational Chat with Document Grounding
1. Click the **Copilot** icon in the right sidebar.
2. Ask any question about your notes or request writing assistance.
3. Click the **Pin Active Note** button in the chat input to automatically attach your currently open document as context for the prompt.

### 2. Configuring AI Providers
Open **Settings** (`Ctrl+,`) → **Copilot** to choose your provider:
- **Local (Ollama / LM Studio)**: Zero data leaves your machine. Set host to `http://localhost:11434` and choose models like `llama3.2`, `mistral`, or `qwen2.5-coder`.
- **Cloud Providers (Bring Your Own Key)**:
  - **Anthropic**: Enter your Claude API key to use Claude 3.5 Sonnet.
  - **OpenAI**: Paste your OpenAI API key to use GPT-4o.
  - **Google Gemini**: Paste your Gemini API key.

## 3. Architecture & SDK Blueprint (For Extension Builders)

The Copilot extension demonstrates how to build streaming AI chat interfaces, inject active document context, and manage multi-turn conversational memory via the Noether SDK.

### SDK Extension Points Used
- `this.registerSidebarTab()`: Mounts the conversational chat interface in the right sidebar.
- `this.registerSlashCommand()`: Adds the `/ai` prompt command to the editor popover.
- `this.registerTool()`: Registers MCP tools enabling AI agents to query vault documents.

### Real SDK Implementation Pattern

```typescript
import { Extension, NoetherApp } from 'noether';

export default class CustomAssistantExtension extends Extension {
  async onload(): Promise<void> {
    this.addCommand({
      id: 'ai:summarize-active',
      title: 'Copilot: Summarize Active Note',
      section: 'AI Assistant',
      action: async (app: NoetherApp) => {
        const activeDoc = app.vault.activeDocument;
        if (!activeDoc) return;

        app.workspace.showToast('Generating summary...', 'info');
        // Stream inference from local or cloud endpoint
      },
    });
  }
}
```

## 4. MCP Tools Reference

### 1. `copilot_chat`
- **Description**: Dispatches a grounded prompt to the configured LLM with optional document context.
- **Parameters**:
  - `prompt` (string, required): User question or instruction.
  - `documentId` (string, optional): Context document identifier.

## 5. Development & Local Building

To build and test this community extension locally:

```bash
git clone https://github.com/yvliet/noether-copilot.git
cd noether-copilot
npm install
npm run build
```

Copy the compiled bundle `dist/main.js` and `manifest.json` into your vault's `.noether/extensions/noether-copilot/` directory and reload Noether.

## 6. License

MIT © [Yuliet Li](https://github.com/yvliet)
