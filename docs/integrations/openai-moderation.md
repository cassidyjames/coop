# OpenAI Moderation API

Coop integrates with [OpenAI's moderation endpoint](https://platform.openai.com/docs/guides/moderation) to classify text content for harmful categories. The moderation endpoint is free to use.

## Requirements

- An [OpenAI](https://platform.openai.com) account with API access

## Configuration

In Coop, go to **Settings → Integrations** and add your OpenAI API key.

## Signals

Each harmful category is available as a separate signal in Coop's signal library. All signals return a score between 0 and 1, which you can use as a condition threshold in routing rules and proactive rules.

| Signal             | What it detects                                                                 |
| ------------------ | ------------------------------------------------------------------------------- |
| Hate               | Content that expresses hatred toward a group based on protected characteristics |
| Hate (threatening) | Hate content that also includes threats or violent language                     |
| Self-harm          | Content that depicts, encourages, or provides instructions for self-harm        |
| Sexual             | Sexually explicit content                                                       |
| Sexual (minors)    | Sexual content involving minors                                                 |
| Violence           | Content that depicts or glorifies violence or physical harm                     |
| Violence (graphic) | Graphic or gory violent content                                                 |

Coop also includes an OpenAI Whisper transcription signal, which can convert audio content to text for downstream analysis with text-based signals.

## Models

OpenAI offers two models for the moderation endpoint:

- **omni-moderation-latest** (recommended): supports text and image inputs, more category coverage, and receives ongoing updates
- **text-moderation-latest** (legacy): text inputs only, fewer categories

## Limitations

- Scores are probabilistic and not definitive; use thresholds appropriate to your platform and review confirmed positives
- `omni-moderation-latest` supports image inputs but Coop's current integration passes text fields; images require separate configuration
- Performance may vary across languages and locales; optimized for English content
