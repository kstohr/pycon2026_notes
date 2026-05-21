# Pycon 2026 Notes

## Overview 
- Language updates: 
    - The GIL is gone; long live free-threading
    - Lazy imports
    - T-strings

- Security 
    - Hockey stick increase in malware attacks in PyPI libraries. 
        - often transitive dependencies (i.e. torch)
        - only two developers focused on securing PyPI
    - Maintainers overwhelmed by agents and burning out
    - Tools: See [fetter](https://github.com/fetter-io/fetter-py)
    - Pin libraries. You will want to minimize risk
    - 2FA auth - good, but 2FA tokens phishable, 30 sec window
    - Adopt WebAuthn - passkey (stored on device) 
        - Passkeys - better than 2FA.
        - Stop rotating keys, get rid of them. If grant permanently, not least privilege 
    - transitive dependency vulnerabilities!!!!  Pin your packages
    - One service account access entire data lake -- if compromised own
      everything 
        - ensure separate service accounts per db, role, service
        - A researcher gave a great talk on securing data -- every file gets a
          single short-lived credential

- Agentic tooling/use cases 
    - Using tooling in browsers/cli 
    - Building a "trust layer" for agent use. (See Text2SQL)
    - Projects running LLMs locally. 

## Python Language Updates 

### PEP standards
- [free-threading (PEP 703)](https://peps.python.org/pep-0703/)
- [Lazy imports (PEP 690)](https://peps.python.org/pep-0690/)
- [T-strings (PEP 750)](https://peps.python.org/pep-0750/)

### Talks
  - [Free-threaded Python: past, present and future](https://us.pycon.org/2026/schedule/presentation/63/)

  - [Lock-Free Multi-Core Performance with Behavior-Oriented Concurrency](https://us.pycon.org/2026/schedule/presentation/54/)

  - [T-strings](https://us.pycon.org/2026/schedule/presentation/70/)


## Command Line/Browser Applications: 

Generally, with the use of agents a refocus on better CLI/Browser 
tooling - interesting trend. This is akin to the Jupyter Notebook as application
trend a few years ago.

- [The Terminal is the New Browser: Building Rich TUIs with Textual](https://us.pycon.org/2026/schedule/presentation/103/)

- [How to Write a Great Command Line Application](https://us.pycon.org/2026/schedule/presentation/165/)
[From Graveyard to Glory: Production Python in the Browser](https://us.pycon.org/2026/schedule/talks/)

- [How to port a Python kernel to Pyodide for a blazingly fast in-browser coding experience](https://us.pycon.org/2026/schedule/presentation/78/)

- [Distributing AI with Python in the Browser: Edge Inference and Flexibility Without Infrastructure](https://us.pycon.org/2026/schedule/presentation/126/)

## Analytics: 
[Text2SQL](https://us.pycon.org/2026/schedule/presentation/33/)
  - [slides](github.com/mrMakaronka/text2sql-pyconus26)

- Ok, this was a phenomenal talk about the pitfalls of trying to let an agent
  to run raw SQL commands. They found that it led to credibility issues and poor
  data not because the agent couldn't write and run a SQL command but because
  the nuances of the business were not correctly interpreted by the agent. So,
  they added a trust layer using certified metrics. Certified metrics are
  nothing new, but it's a great way of handling agent calls for analytics
  requests. 
- Building trusted metrics -> semantic layer 
- [Importance of a Semantic Layer](https://rsmus.com/insights/services/digital-transformation/importance-of-semantic-layer-for-trusted-ai-solutions.html)
- DBT/OSI = semantic layer; schema abstraction, typically used standards
    - [OSI](https://github.com/open-semantic-interchange/OSI/blob/main/core-spec/spec.md)
    - [DBT](https://www.getdbt.com/)
- Product:  https://github.com/JetBrains/databao-agent
    - As always with data products issue of
      auth/acl complexity, distance from codebase, etc.

[Beyond the P-Value: A Data Scientist’s Guide to Tier-1 Feature Launches](https://us.pycon.org/2026/schedule/presentation/69/)
- Fantastic talk on causal inference for product analytics. 
- Spoke to presenter about our small data problems and what to do when you do
  not have enough data for experiment design, A/B testing or even a control
  group. 
- [Causal Inference Mixtape](https://mixtape.scunning.com/)


## Data Engineering
[Why Software Engineering Best Practices Fail in Data
Engineering](https://us.pycon.org/2026/schedule/presentation/94/)
[slides](https://drive.google.com/file/d/1-svauV8RWVkXcxSgMlzdI_Obwc--5xzy/view)
- 5 error patterns that are different than other software development
- This was a great talk. The five things to test for in data engineering: 
    - Code correctness (unit tests, linting, etc.)
    - Data correctness (Data checks, null checks etc)
    - Time correctness (freshness, lateness, completeness by cutoff)
    - State transition safety (partial writes, duplicate insertion)
    - Recovery and degradation (dead letter paths, stale-but-labeled data,
      idempotent backfills, trustable data even when degraded) 
- Time and data state = 1st class
- Betamax playbacks - record payloads 
- Cron monitoring. 

[The Surprising Effectiveness of Immutable Data
Structures](https://us.pycon.org/2026/schedule/presentation/115/)
[slides](https://docs.google.com/presentation/d/1SXd7u-mGtc3iq0QlI5s63Vof-43UUYX1-TJ5j3caQU4/edit?slide=id.p#slide=id.p)
- A great talk on immutable data structures and
  why they should be preferred except in cases when speed requires caching,
  shared state.


## AI Tooling

- [Running Large Language Models on Laptops: Practical Quantization Techniques in Python](https://us.pycon.org/2026/schedule/presentation/138/)



- [How to Build Your First Real-Time Voice Agent in Python (Without Losing Your Mind)](https://us.pycon.org/2026/schedule/presentation/101/) This one was Just sweet... 


### Package for model quantization 
- Quantize unnecessary layers, functional use.
- [LangGraph](https://docs.langchain.com/oss/python/langgraph/overview)
  graph-based tuning of LLMs
- [GGML](https://huggingface.co/blog/introduction-to-ggml) - may become standard
- [QLoRA (Quantized Low-Rank Adaptation)](https://arxiv.org/abs/2402.10462)

- [Docling](https://www.docling.ai/)

### Hardware: 
- [NVIDIA Spark](https://marketplace.nvidia.com/en-us/enterprise/personal-ai-supercomputers/dgx-spark/) 

## Recommended Packages: 
Packages that were mentioned as useful

- [flyte](https://flyte.org/platform/flyte-2) - may be useful for jobs like K8s topic training, where we spin off a container
- [Pandera](https://pandera.readthedocs.io/en/stable/) - Pydantic for dataframes 
- [Immich](https://awesome.immich.app/) - google photos, but on your compute
- [mayfly](https://github.com/matth/mayfly) - single use short-lived credentials
for document storage. Every file gets one credential that expires 
- [fastglob](https://crates.io/crates/fast-glob) - Rust

 ## Lightning talks 
 - [gh-profiler](https://github.com/ehmatthes/gh-profiler/discussions/18) - are
   you a bot? 
  - Predict date of opossum arrival ( I don't have slides, but there was a
    github repo and great pics of opossums)


