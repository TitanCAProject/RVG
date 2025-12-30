#### 1. Tool Information

* **Tool Name:** RVG (Realistic Vulnerability Generation) Framework 
* **Repository / URL:** https://github.com/yikun-li/TitanVul-BenchVul

#### 2. Authors and Contact

* **Main Author(s):** Yikun Li, Ngoc Tan Bui, Ting Zhang, Chengran Yang, Xin Zhou, Martin Weyssow, Jinfeng Jiang, Junkai Chen, Huihui Huang, Huu Hung Nguyen, Chiok Yew Ho, Jie Tan, Ruiyin Li, Yide Yin, Han Wei Ang, Frank Liauw, Eng Lieh Ouh, Lwin Khin Shar, David Lo 
* **Contact:** [yikunli@smu.edu.sg](mailto:yikunli@smu.edu.sg)

#### 3. Overview

RVG generates synthetic, context-aware vulnerability examples for specified CWE types using a multi-agent LLM workflow that simulates development and security review. It outputs paired vulnerable and remediated function-level code, designed to augment scarce CWE categories for training and evaluation. 

#### 4. Installation

Clone the repository and install the required Python dependencies. RVG relies on an LLM provider (e.g., OpenAI or Anthropic), so you typically also need to set the corresponding API key(s) in your environment. 

```bash
git clone https://github.com/yikun-li/TitanVul-BenchVul.git
cd TitanVul-BenchVul

pip install -r requirements.txt

# Example (adjust to your provider)
export OPENAI_API_KEY="..."
# or
export ANTHROPIC_API_KEY="..."
```

#### 5. Usage

RVG can be run from the vulnerability generation pipeline directory. You can generate samples generally, or target specific CWE IDs and a desired count. (The repository includes more details under `vulnerability_generation/README.md`.)

```bash
cd vulnerability_generation_pipeline

# Generate vulnerability samples
python main.py --provider openai --model gpt-4o

# Generate for specific CWEs
python main.py --specific-cwe CWE-89 CWE-22 --target-count 50
```

RVG’s multi-agent roles are: **Context & Threat Modeler**, **Vulnerable Implementer**, **Security Auditor**, and **Security Reviewer**; the framework may also perform cross-model validation (e.g., one model generates, another validates) depending on configuration. 

#### 6. Input and Output Format

* **Input format:**

  * Command-line arguments such as:

    * `--provider` and `--model`
    * `--specific-cwe` (one or more CWE IDs)
    * `--target-count` (desired number of samples)
  * Conceptually, RVG conditions generation on the CWE definition and produces an application context, attack vector, and function-level code pair. 

* **Output format:**

  * For each generated instance, RVG produces:

    * a structured **application context** (e.g., language/stack, scenario, attack vector)
    * a **vulnerable function** and a corresponding **remediated (fixed) function**
    * labels/metadata such as the target **CWE** (and often the rationale/validation outcome from the reviewer step) 
  * The exact on-disk serialization (e.g., JSON/JSONL/CSV and output paths) is defined by the repository implementation/configuration.
