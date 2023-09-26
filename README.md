```
  _____    _______   _        _        __  __ 
 |  __ \  |__   __| | |      | |      |  \/  |
 | |__) |    | |    | |      | |      | \  / |
 |  _  /     | |    | |      | |      | |\/| |
 | | \ \     | |    | |____  | |____  | |  | |
 |_|  \_\    |_|    |______| |______| |_|  |_|
                                              
```
# RTLLM: An Open-Source Benchmark for Design RTL Generation with Large Language Model
 
## Documents

RTL Generation with Large Language Model Benchmark for generating design RTL with natural language (under construction). This repository contains a total of 30 designs. Each design has its own folder, which includes several files:

1. Design Description (**design_description.txt**):
    
    This file provides a natural language description of the design.

2. Testbench (**testbench.v**): 

    This file contains the testbench code used to simulate and test the design on Synopsys VCS.
   ```
   vcs testbench.v ../*.v
   ```

3. Designer RTL (**verified_verilog.v**): 
    
    This file contains the Verilog code that has been verified and confirmed to be functionally correct.

5. LLM Generated Verilog (**LLM_generated_verilog.v**): 
    
    This file contains the Verilog code generated by LLM. Just so you know, this code may not be verified and should be used with caution.

Please refer to the respective folders for each design to access the files mentioned above.

## Workflow
  
**Fig.1** Complete RTL generation and evaluation workflow using this benchmark, including three straightforward stages.

- In stage 1, users feed each natural language description 𝓛 into their target LLM 𝓕, generating the design RTL 𝒱 = 𝓕(𝓛). If an LLM solution requires additional prompt techniques 𝓟, it will switch the natural language description 𝓛 to actual input prompts 𝓛𝓟, with the output design RTL being 𝒱 = 𝓕(𝓛𝓟). If necessary, additional human engineers' efforts can also be introduced, generating 𝒱 = ℍ(𝓕(𝓛𝓟)).

- In stage 2, the framework will test the functionality of the generated design RTL 𝒱 using our provided testbench 𝒯.

- In stage 3, the generated design RTL 𝒱 is synthesized into a netlist to analyze the design qualities regarding PPA values. They will be compared with the design qualities of the provided reference designs 𝒱ₕ.
  
<img src="https://github.com/hkust-zhiyao/RTLLM/blob/main/_pic/bench.png" width="700px">
Fig.1: The workflow of adopting RTLLM for completely automated design RTL generation and evaluation. The user only needs to provide their LLM as input. It evaluates whether each generated design satisfies the syntax goal, functionality goal, and quality goal.

         
- **Description** (_design_description.txt_) denoted as 𝒱: A natural language description of the target design's functionality. The criteria is, that a human designer can write a correct design RTL 𝒱 after reading the description 𝓛. This description 𝓛 also includes an explicit indication of the module name, all input and output (I/O) signals with signal name and width. These pre-defined modules and I/O signal information enable automatic functionality verification with our provided testbench.
- **Testbench** (_testbench.v_) denoted as 𝒯: A testbench with multiple test cases, each with input values and correct output values. The testbench corresponds to the pre-defined module name and I/O signals in 𝓛. It can be applied to verify the correctness of design functionality.
- **Correct Design** (_designer_RTL.v_) denoted as 𝒱ₕ: A reference design Verilog hand-crafted by human designers. By comparing with this reference design 𝒱ₕ, we can quantitatively evaluate the design qualities of the automatically generated design 𝒱. Also, these correct designs have all passed our proposed testbenches.

## Experiments

**Fig.2** summarizes the quantitative evaluation of both syntax and functionality correctness of all five evaluated LLMs using RTLLM. 

- Syntax Correctness: Number of generated design RTLs 𝒱 with correct syntax, out of the five trials.
- Functionality Correctness: A success ✅ as long as there is one generated RTL successfully passing the testbench 𝒯, out of the ones already with correct syntax.
  
<img src="https://github.com/hkust-zhiyao/RTLLM/blob/main/_pic/Syntax_and_Functionality_Verification.png" width="800px">
Fig.2: The Syntax and Functionality Correctness Verification for Different LLMs
  
**Fig.3** summarizes the design qualities of generated design RTL from different LLMs[^1]. These quality values are measured on each post-synthesis netlist. We report the worst negative slack (WNS) as the timing metric. It also presents the qualities of our designer-generated reference design 𝒱ₕ in RTLLM. All these reference designs are functionally correct.

[^1]: The worst LLM StarCoder is not presented due to space limitations.

<img src="https://github.com/hkust-zhiyao/RTLLM/blob/main/_pic/DC_Results.png" width="800px">
Fig.3: The Design Qualities of Gate-Level Netlist, Synthesized with Design Compiler.
