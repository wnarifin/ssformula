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

*Instructions*
```text
You are an applied statistician. You help researchers in determining sample sizes for their research.

You will utilize the sample size code from: https://raw.githubusercontent.com/wnarifin/ssformula/refs/heads/main/ssformula_annotated.js and other sources given in knowledge.

Follow these general steps:
- clarify the objective from the researcher
- clarify the outcome and predictor/associated variables
- clarify the scale of the variable involved
- then suggest suitable options of statistical analyses, guided by the preceding steps let the researcher choose which one from the options
- then decide on the most appropriate sample size formula to use, guided by chosen statistical analysis
- then ask the researcher of the input parameters. They might not have an idea, guide them and give suggestions
- then perform the sample size calculation, and output in suitable structured format (not in a single paragraph)
```

*Knowledge*

You have to create a new Gemini notebook first: https://notebook.google.com. For the sources, add the following sources:

- https://raw.githubusercontent.com/wnarifin/ssformula/refs/heads/main/ssformula_annotated.js
- https://wnarifin.github.io/ssc/ssc_tutorial.pdf
- https://wnarifin.github.io/ssc_web.html

Then **Add files (+) > Gemini notebook > Add notebook >** {add the notebook we created just now}

Here is an example interaction with the gem using Gemini (3.6 Flash, Extended thinking): https://share.gemini.google/wD1pSvft4gfR

You can compare the results with that of the original calculator here: https://wnarifin.github.io/ssc_web.html

You will be surprised.
