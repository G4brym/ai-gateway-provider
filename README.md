# AI Gateway Provider for Vercel AI SDK

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This library provides an AI Gateway Provider for the [Vercel AI SDK](https://sdk.vercel.ai/docs), enabling you to seamlessly integrate multiple AI models from different providers behind a unified interface. It leverages Cloudflare's AI Gateway to manage and optimize your AI model usage.

## Features

* **Runtime Agnostic:** Works in all JavaScript runtimes supported by the Vercel AI SDK including Node.js, Edge Runtime, and more.
* **Automatic Provider Fallback:** ✨ Define an array of models and the provider will **automatically fallback** to the next available provider if one fails, ensuring high availability and resilience for your AI applications.
* **Multi-Provider Support:** Easily switch between and combine models from various providers like OpenAI, Anthropic, DeepSeek, Google AI Studio, Grok, Mistral, Perplexity AI, Replicate and Groq.
* **AI Gateway Integration:** Utilizes Cloudflare's AI Gateway for advanced features like request management, caching, and rate limiting.
* **Simplified Model Management:** Abstracts away the complexities of interacting with different AI provider APIs.
* **Easy Configuration:** Simple and intuitive setup process.

## Installation

```bash
npm install ai-gateway-provider
```

## Usage

### Basic Example

```typescript
import {createOpenAI} from '@ai-sdk/openai';
import {createAiGateway} from 'ai-gateway-provider';
import {generateText} from "ai";
import {createAnthropic} from "@ai-sdk/anthropic";

const aigateway = createAiGateway({
  accountId: "my-cloudflare-account-id",
  gateway: 'my-gateway-name',
  apiKey: 'optionally my cloudflare api key'
});
const openai = createOpenAI({apiKey: 'openai api key'});
const anthropic = createAnthropic({apiKey: 'anthropic api key'});

const model = aigateway([
  anthropic('claude-3-5-haiku-20241022'),
  openai("gpt-4o-mini"),
]);

const {text} = await generateText({
  model: model,
  prompt: 'Write a vegetarian lasagna recipe for 4 people.',
});
```

### Explanation

1. **Import necessary modules:**
  * `createOpenAI` and `createAnthropic` from their respective provider libraries.
  * `createAiGateway` from `ai-gateway-provider`.
  * `generateText` from `ai` (Vercel AI SDK).
2. **Create AI Provider instances:**
  * `createAiGateway` is initialized with your Cloudflare account ID, gateway name, and optionally, your Cloudflare API key.
  * `createOpenAI` and `createAnthropic` are initialized with their respective API keys.
3. **Create the AI Gateway Model:**
  * The `aigateway` function takes an array of models as input. In this example, it uses `claude-3-5-haiku-20241022` from Anthropic and `gpt-4o-mini` from OpenAI.
  * **Key Feature:** The gateway will try the models in the order they are defined in the array, automatically falling back to the next model if the primary choice is unavailable.
4. **Generate Text:**
  * The `generateText` function from the Vercel AI SDK is used to generate text based on the specified model and prompt.

## Automatic Fallback Example

```typescript
// Define multiple provider options with fallback priority
const model = aigateway([
  anthropic('claude-3-5-haiku-20241022'),  // Primary choice
  openai("gpt-4o-mini"),                   // First fallback
  mistral("mistral-large-latest"),         // Second fallback
]);

// The system will automatically try the next model if previous ones fail
const {text} = await generateText({
  model: model,
  prompt: 'Suggest three names for my tech startup.',
});
```

## Configuration

### `createAiGateway(options: AiGatewaySettings)`

* `options`: An object containing the following properties:
  * `accountId`: Your Cloudflare account ID. This is required.
  * `gateway`: The name of your AI Gateway. This is required.
  * `apiKey` (Optional): Your Cloudflare API key. Required if your AI Gateway has authentication enabled.

## Supported Providers

The following providers are currently supported:

* OpenAI
* Anthropic
* DeepSeek
* Google AI Studio
* Grok
* Mistral
* Perplexity AI
* Replicate
* Groq

## Supported Methods

Currently only chat completions (non-streaming) is supported.
More can be added, please open an issue in the GitHub repository!

## Error Handling

The library throws the following custom errors:

* `AiGatewayUnauthorizedError`: Your AI Gateway has authentication enabled, but a valid API key was not provided.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Relevant Links

* [Vercel AI SDK Documentation](https://sdk.vercel.ai/docs)
* [Cloudflare AI Gateway Documentation](https://developers.cloudflare.com/ai-gateway/)
* [GitHub Repository](https://github.com/G4brym/ai-gateway-provider)
