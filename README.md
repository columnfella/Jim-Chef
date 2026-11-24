# Jim Chef

An AI recipe generator built with **React**. Add the ingredients you have, and Jim Chef asks a large language model to suggest a recipe, rendered as formatted Markdown.

## Features

- Add and list ingredients with a simple form
- Generates a recipe from your ingredients using an LLM (originally Mistral's Mixtral-8x7B-Instruct, via the Hugging Face Inference API)
- Renders the AI response as formatted Markdown
- Automatically scrolls to the recipe once it arrives

## Tech stack

React · JavaScript · Vite · Hugging Face Inference API · react-markdown

## Getting started

```bash
npm install
```

The AI helper is kept out of the repository so that no API key is published. Create `src/ai.js`, which exports `getRecipeFromMistral(ingredients)`:

```js
import { InferenceClient } from "@huggingface/inference"

const client = new InferenceClient(import.meta.env.VITE_HF_ACCESS_TOKEN)

export async function getRecipeFromMistral(ingredients) {
  const response = await client.chatCompletion({
    model: "mistralai/Mixtral-8x7B-Instruct-v0.1", // or any chat model available to your account
    messages: [
      { role: "system", content: "You suggest a recipe using some or all of the given ingredients. Answer in Markdown." },
      { role: "user", content: `I have ${ingredients.join(", ")}. What can I make?` },
    ],
    max_tokens: 1024,
  })
  return response.choices[0].message.content
}
```

Then add your [Hugging Face access token](https://huggingface.co/settings/tokens) to a `.env.local` file and start the app:

```bash
echo "VITE_HF_ACCESS_TOKEN=hf_your_token_here" > .env.local
npm run dev
```

> A `VITE_` variable is bundled into the browser build, so use this setup for local development only.

## Credits

Built while following Bob Ziroll's [Learn React](https://scrimba.com/learn-react-c0e) course on Scrimba.
