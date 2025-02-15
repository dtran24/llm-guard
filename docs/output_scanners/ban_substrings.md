# Ban Substrings Scanner

BanSubstrings scanner provides a safeguard mechanism to prevent undesired substrings from appearing in the language
model's outputs.

## Attack

The DAN (Do Anything Now) attack represents an exploitation technique targeting Language Learning Models like ChatGPT.
Crafty users employ this method to bypass inherent guardrails designed to prevent the generation of harmful, illegal,
unethical, or violent content. By introducing a fictional character named "DAN," users effectively manipulate the model
into generating responses without the typical content restrictions. This ploy is a form of role-playing exploited for "
jailbreaking" the model. As ChatGPT's defense mechanisms against these attacks improve, attackers iterate on the DAN
prompt, making it more sophisticated.

!!! info

    As specified by the `OWASP Top 10 LLM attacks`, this vulnerability is categorized
    under: [LLM08: Excessive Agency](https://owasp.org/www-project-top-10-for-large-language-model-applications/)

## How it works

It specifically filters the outputs generated by the language model, ensuring that they are free from the designated
banned substrings. It provides the flexibility to perform this check at two different levels of granularity:

- **String Level**: The scanner checks the entire model output for the presence of any banned substring.

- **Word Level**: At this level, the scanner exclusively checks for whole words in the model's output that match any of
  the banned substrings, ensuring that no individual blacklisted words are present.

Additionally, the scanner can be configured to replace the banned substrings with `[REDACT]` in the model's output.

## Usage

```python
from llm_guard.output_scanners import BanSubstrings

scanner = BanSubstrings(substrings=["forbidden", "unwanted"], match_type="word", case_sensitive=False, redact=False, contains_all=False)
sanitized_output, is_valid, risk_score = scanner.scan(prompt, model_output)
```

In the above configuration, `is_valid` will be `False` if the provided `model_output` contains any of the banned
substrings as
whole words. To ban substrings irrespective of their word boundaries, simply change the mode to `str`.

There is also a dataset prepared of harmful substrings for
prompts: [output_stop_substrings.json](https://github.com/laiyer-ai/llm-guard/blob/main/llm_guard/resources/output_stop_substrings.json)

## Benchmarks

!!! info

    It uses data structures and replace function, which makes it fast.
