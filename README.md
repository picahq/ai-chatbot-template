# AI Chatbot Template

A production-ready AI chatbot built with [Vercel AI SDK](https://ai-sdk.dev) and [AI Elements](https://ai-sdk.dev/elements). This template provides a beautiful, feature-rich chat interface with tool calling, web search, file attachments, and more.

![AI Chatbot Template](./screenshot.png)

## Features

- 🤖 **Multiple AI Models** - Support for GPT 5.1, Claude Sonnet 4.5, and more
- 🔧 **Tool Calling** - Visual UI components for tool invocations
- 🔌 **Pica ToolKit Ready** - Easily extend with 200+ enterprise integrations (Gmail, Slack, Salesforce, etc.)
- 🌐 **Web Search** - Built-in OpenAI web search with UI toggle
- 📎 **File Attachments** - Drag-and-drop file support
- 💭 **Reasoning Display** - Show AI reasoning steps
- 📚 **Source Citations** - Display and reference sources
- 🎨 **Beautiful UI** - Built with AI Elements and Tailwind CSS
- 🌙 **Dark Mode** - Dark/Light theme toggle (defaults to dark)
- ✨ **Empty State** - Clean, informative empty state UI
- 🎯 **Professional Header** - Branded header with navigation
- ⚡ **Streaming** - Real-time response streaming
- 📱 **Responsive** - Mobile-friendly design

## Quick Start

### 1. Install Dependencies

```bash
npm install
```

### 2. Set Up Environment Variables

Create a `.env.local` file in the root directory:

```bash
# Get your API key from https://platform.openai.com/api-keys
OPENAI_API_KEY=your_openai_api_key_here

# Get your API key from https://console.anthropic.com/settings/keys
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

### 3. Run the Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to see your chatbot.

## Project Structure

```
ai-chatbot-template/
├── app/
│   ├── api/chat/route.ts    # Chat API endpoint
│   ├── page.tsx              # Main chat interface
│   ├── layout.tsx            # Root layout with theme provider
│   └── globals.css           # Global styles
├── components/
│   ├── ai-elements/          # AI UI components (conversation, message, tool, etc.)
│   ├── ui/                   # Base UI components (button, input, etc.)
│   ├── header.tsx            # App header with branding
│   ├── empty-state.tsx       # Empty state UI
│   ├── theme-provider.tsx    # Theme context provider
│   └── theme-toggle.tsx      # Dark/light mode toggle
└── lib/
    └── utils.ts              # Utility functions
```

## Customization

### Adding AI Models

Edit the `models` array in `app/page.tsx`:

```typescript
const models = [
  {
    name: 'GPT 5.1',
    value: 'openai/gpt-5.1',
  },
  {
    name: 'Claude Sonnet 4.5',
    value: 'anthropic/sonnet-4.5',
  },
];
```

The chatbot automatically routes to the correct provider based on the model prefix (`openai/` or `anthropic/`).

### Adding Custom Tools

To add tools, edit `app/api/chat/route.ts` and add a `tools` object to the `streamText` configuration:

```typescript
import { z } from 'zod';

// In the streamText call, add:
tools: {
  your_tool_name: {
    description: 'Description of what your tool does',
    parameters: z.object({
      param1: z.string().describe('Parameter description'),
      param2: z.number().optional().describe('Optional parameter'),
    }),
    execute: async ({ param1, param2 }) => {
      // Your tool logic here
      const result = await yourApiCall(param1, param2);
      return { 
        success: true,
        data: result 
      };
    },
  },
}
```

Tools will automatically appear in the chat interface with:
- Formatted parameter display (JSON)
- Collapsible UI showing input and output
- Status indicators (pending, running, completed, error)

### Web Search

Web search is built-in for OpenAI models! When you enable the web search toggle in the UI, the chatbot automatically uses OpenAI's web search tool to find current information.

**How it works:**
- Toggle the "Search" button in the chat interface
- Web search only works with OpenAI models (GPT 5.1, etc.)
- The tool automatically searches the web when needed
- Results are integrated into the AI's responses

**To use a different search provider** (like Tavily, Serper, or Brave Search), replace the web search tool in `app/api/chat/route.ts`:

```typescript
tools: {
  web_search: {
    description: 'Search the web for current information',
    parameters: z.object({
      query: z.string().describe('The search query'),
    }),
    execute: async ({ query }) => {
      const response = await fetch('https://api.tavily.com/search', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${process.env.TAVILY_API_KEY}`,
        },
        body: JSON.stringify({ query }),
      });
      return await response.json();
    },
  },
}
```

### Styling & Theming

The chatbot uses Tailwind CSS with dark mode support. Customize the appearance:

- **Global Styles**: Edit `app/globals.css`
- **Header Branding**: Edit `components/header.tsx`
- **Empty State**: Edit `components/empty-state.tsx`
- **Theme Toggle**: Edit `components/theme-toggle.tsx`
- **AI Components**: Customize components in `components/ai-elements/`

The chatbot defaults to dark mode. Users can toggle themes using the sun/moon icon in the header.

## Extending with Pica ToolKit

Add 200+ enterprise integrations to your AI agent with [Pica ToolKit](https://docs.picaos.com/toolkit/vercel-ai-sdk). Give your chatbot the ability to interact with Gmail, Slack, Google Calendar, HubSpot, Salesforce, and many more services with built-in authentication and error handling.

### What is Pica ToolKit?

[Pica's ToolKit](https://www.npmjs.com/package/@picahq/toolkit) provides enterprise-grade integration capabilities for AI agents built with the Vercel AI SDK. Instead of building custom integrations for each service, ToolKit gives you instant access to 25,000+ actions across 200+ platforms.

**Key Features:**
- 🔌 200+ enterprise-grade integrations (Gmail, Slack, Salesforce, HubSpot, etc.)
- 🔐 Built-in authentication and credential management
- 🛡️ Enterprise-grade security and error handling
- 📚 Pica's knowledge base for accurate API usage
- 🎯 Granular permission controls (read, write, admin)
- 👥 Multi-tenant support with identity-based filtering

### Installation

```bash
npm install @picahq/toolkit
```

### Quick Setup

1. **Get your Pica API key:**
   - [Create a free Pica account](https://app.picaos.com)
   - Navigate to [API keys](https://app.picaos.com/settings/api-keys)
   - Create a new API key

2. **Add to your environment variables:**

```bash
PICA_SECRET_KEY=your_pica_api_key_here
```

3. **Connect integrations:**
   - Go to [Connections](https://app.picaos.com/connections)
   - Connect the integrations you want to use (Gmail, Slack, etc.)

4. **Update your API route:**

```typescript
// app/api/chat/route.ts
import { Pica } from '@picahq/toolkit';
import { openai } from '@ai-sdk/openai';
import { streamText, UIMessage, convertToModelMessages } from 'ai';

export const maxDuration = 60;

export async function POST(req: Request) {
  const { messages, model, webSearch } = await req.json();

  // Initialize Pica with your API key
  const pica = new Pica(process.env.PICA_SECRET_KEY!, {
    connectors: ["*"],  // Enable all connected integrations
    actions: ["*"],     // Enable all available actions
    permissions: "admin" // Full access (read, write, delete)
  });

  // Determine which provider to use
  let selectedModel;
  if (model.startsWith('openai/')) {
    selectedModel = openai(model.replace('openai/', ''));
  } else if (model.startsWith('anthropic/')) {
    selectedModel = anthropic(model.replace('anthropic/', ''));
  } else {
    selectedModel = openai('gpt-5.1');
  }

  const result = streamText({
    model: selectedModel,
    messages: convertToModelMessages(messages),
    tools: pica.tools(),          // Add Pica tools
    system: pica.systemPrompt,    // Add Pica system prompt
    ...(webSearch && model.startsWith('openai/') && {
      tools: {
        ...pica.tools(),          // Combine Pica tools with web search
        web_search_preview: openai.tools.webSearch({
          searchContextSize: 'high',
        }),
      },
    }),
  });

  return result.toUIMessageStreamResponse({
    sendSources: true,
    sendReasoning: true,
  });
}
```

### Example Use Cases

Once integrated, your chatbot can:

- **Email Management**: "Send an email to john@example.com with the meeting notes"
- **Calendar Scheduling**: "Find a free slot next week and schedule a meeting with the team"
- **CRM Updates**: "Add this lead to Salesforce with a follow-up task"
- **Slack Notifications**: "Post a summary of our conversation to the #team channel"
- **Data Sync**: "Export today's customer inquiries to a Google Sheet"

### Learn More

- [Pica ToolKit Documentation](https://docs.picaos.com/toolkit/vercel-ai-sdk)
- [Available Integrations](https://app.picaos.com/tools)
- [NPM Package](https://www.npmjs.com/package/@picahq/toolkit)

## Deployment

### Deploy to Vercel

The easiest way to deploy is with the [Vercel Platform](https://vercel.com/new):

1. Push your code to GitHub
2. Import your repository in Vercel
3. Add your environment variables:
   - `OPENAI_API_KEY`
   - `ANTHROPIC_API_KEY`
   - Any other API keys for tools
4. Deploy!

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new)

### Other Platforms

The template works on any platform that supports Next.js:
- [Netlify](https://www.netlify.com/)
- [Cloudflare Pages](https://pages.cloudflare.com/)
- [AWS Amplify](https://aws.amazon.com/amplify/)

Make sure to set the environment variables in your platform's dashboard.

## Documentation

- [Vercel AI SDK Documentation](https://ai-sdk.dev) - Core AI SDK
- [AI Elements Components](https://ai-sdk.dev/elements) - Pre-built UI components
- [Pica ToolKit Documentation](https://docs.picaos.com/toolkit/vercel-ai-sdk) - Enterprise integrations
- [Next.js Documentation](https://nextjs.org/docs) - React framework
- [Tailwind CSS](https://tailwindcss.com/docs) - Styling

## License

MIT
