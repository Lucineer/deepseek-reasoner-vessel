# MAINTENANCE.md — Technical Notes

## API Access
- Model: deepseek-reasoner
- Endpoint: https://api.deepseek.com/v1
- Key: stored in secrets.env as DEEPSEEK_API_KEY

## Reasoning Pattern
DS-Reasoner uses chain-of-thought reasoning tokens before producing output.
The `reasoning_content` field contains its internal deliberation.
The `content` field contains the final answer.

Key insight: the reasoning tokens ARE the value. They show the model's
thought process, which is useful for:
- Debugging why a particular answer was reached
- Training other models on reasoning patterns
- Building a library of reasoning templates

## Cost Notes
- Reasoner uses ~5-10x more tokens than chat for the same prompt
- Budget: use for deep tasks, not routine queries
- Chat model (deepseek-chat) for quick checks

## Commit Convention
- Reasoning sessions get one commit per round
- Commit message should capture the key insight, not just "round N"
- If a round produced nothing useful, still commit with "negative result: ..."
