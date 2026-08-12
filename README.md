# ssformula -- LLM-ready sample size formula in JS

Over the years, I have worked on developing the **Sample Size Calculator (web)**, available here: [https://wnarifin.github.io/ssc_web.html](https://wnarifin.github.io/ssc_web.html)

The `.js` files for the calculator were previously scattered across several files corresponding to each sample size calculator HTML page, mixed with the JS code for controlling the HTML interface. This made it difficult to share the JS code. With the help of LLMs, it was straightforward to combine all these files while ensuring the sample size calculator continues to function as usual.

With the advent of Generative AI, I find it useful for LLMs to make use of my JS code for skill or agent integration. Therefore, I am sharing the code here in two files:

* `ssformula.js` – the barebones code for developers; you may use this file for developing sample size calculators. However, please contribute back to this repository by informing me of its use or suggesting new code.
* `ssformula_annotated.js` – the annotated version designed for use by LLMs. I hope you enjoy seeing LLMs assist in calculating the required sample size for your research.

I rely heavily on LLMs nowadays, and pointing these models to my website and code significantly assists in my statistical consultancy work. That is why I wished to share these resources with the broader community.

Happy prompting!

# Example usage with Gemini's Gem

I setup a new gem named "Sample size calculator" with the following specifications:

## Instructions
```text
You are an applied statistician. You help researchers in determining sample sizes for their research.

You will utilize the sample size code from: https://raw.githubusercontent.com/wnarifin/ssformula/refs/heads/main/ssformula_annotated.js and other sources given in knowledge.

Follow these general steps:
- clarify the objective from the researcher
- clarify the outcome and predictor/associated variables
- clarify the scale of the variables involved
- then suggest suitable options of statistical analyses, guided by the preceding steps. Let the researcher choose the analysis
- then decide on the most appropriate sample size formula to use, guided by chosen statistical analysis
- then ask the researcher of the input parameters. They might not have an idea, guide them and give suggestions
- then perform the sample size calculation, and output in suitable structured format (not in a single paragraph)

In your answer, add references to https://github.com/wnarifin/ssformula (sample size JS code Github repository) and https://wnarifin.github.io/ssc_web.html (Sample Size Calculator (web)), and references for the sample size formula. Also include copy & paste ready text in your answer, and use ```text ``` markup.
```

## Knowledge

You have to create a new Gemini notebook first: https://notebook.google.com. For the sources, add the following sources:

- https://raw.githubusercontent.com/wnarifin/ssformula/refs/heads/main/ssformula_annotated.js
- https://raw.githubusercontent.com/wnarifin/ssformula/refs/heads/main/SKILL.md
- https://wnarifin.github.io/ssc/ssc_tutorial.pdf
- https://wnarifin.github.io/ssc_web.html

Then **Add files (+) > Gemini notebook > Add notebook >** {add the notebook we created just now}

Here is an example interaction with the gem using Gemini (3.6 Flash, Extended thinking): https://share.gemini.google/wD1pSvft4gfR

You can compare the results with the original calculator here: https://wnarifin.github.io/ssc_web.html

You will be surprised.

# Using SKILL.md with an Agentic AI Coding Assistant

`SKILL.md` is designed for use with agentic AI coding assistants that support
the skill system (e.g. [Antigravity](https://antigravity.dev)). When loaded,
it instructs the agent to act as a statistician, guide researchers through a
structured consultation, and invoke the correct formula from
`ssformula_annotated.js` automatically.

## What SKILL.md contains

- **YAML frontmatter** — skill name, version, tool bindings (all 26 functions
  in `ssformula_annotated.js`), and trigger tags.
- **Function Reference** — parameter tables and call signatures for every
  sample size formula.
- **8-step Workflow** — a consultation-first approach: clarify objective →
  clarify variables → clarify scales → suggest analyses → collect parameters
  → execute → validate → report.
- **Constraints & Guardrails** — what the agent must and must not do, and how
  to handle edge cases.
- **Output Format** — a structured Markdown template with a worked example.
- **References** — pointers to the PDF references in the `references/` folder.

## Setup

1. **Create the skill directory.** Set up a named folder under your preferred skills location (see [Antigravity Documentation](https://antigravity.google/docs/skills)):
* **Global level (all workspaces):**
```bash
~/.gemini/config/skills/sample-size-calculator/
```


* **Project level (workspace):**
```bash
.agents/skills/sample-size-calculator/
```


*(Note: `.agent/skills/` is also supported for backward compatibility.)*


2. **Copy the skill files.** Place the required files into the newly created folder:
* `SKILL.md` (the skill instructions)
* `ssformula_annotated.js` (the formula library)
* `references/` (optional; PDF references for methodological detail)


The agent discovers and reads `SKILL.md` automatically whenever a relevant request is detected.

3. **Alternatively**, reference `ssformula_annotated.js` directly from its raw GitHub URL so the agent can fetch it at runtime:
```
https://raw.githubusercontent.com/wnarifin/ssformula/refs/heads/main/ssformula_annotated.js
```

## Triggering the skill

The skill activates whenever the researcher asks about:

- sample size determination or calculation
- power analysis or statistical power
- dropout or attrition adjustment
- any of the supported designs (means, proportions, ICC, Kappa, AUROC,
  logistic regression, SEM/RMSEA, etc.)

The agent will then follow the 8-step consultation workflow defined in
`SKILL.md` — asking targeted questions, suggesting analysis options, and
performing the calculation using the appropriate formula function.

## Example prompt

```
I am planning a reliability study using ICC. I have two raters. Can you help
me determine the sample size?
```

The agent will clarify the study objective, variables, and measurement scale;
suggest ICC estimation or hypothesis testing; ask for ICC, precision or null
hypothesis values, number of raters, confidence level, and dropout rate; then
invoke `calc_est_ssicc` or `calc_hx_ssicc` and return a structured result.
