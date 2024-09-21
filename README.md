# LionML - zkML libary for Aleo

# Overview
<img width="635" alt="image" src="https://github.com/user-attachments/assets/8be47ec7-1e79-471e-9cd9-d4b16ee6eb1d">

LionML is a library and command-line tool for doing machine learning model inference and other computational graphs in a zk-snark specifically for **Aleo’s Varuna Circuit**. It branches off of the **[ezkl library by zkonduit](https://github.com/zkonduit/ezkl)** for Aleo developers so that generalized deep learning models can work on Aleo Ecosystem. 

# Problem

Aleo lacks zkml resources for developers. The current machine learning related libraries and toolings on [Awesome Aleo](https://github.com/howardwu/awesome-aleo?tab=readme-ov-file#machine-learning) are either outdated or limited in transpiling a few models that are not readily usable. The current tools works as follows

1. Machine Learning Model is written in Python
2. LEO Program Runner transiles Python Model into LEO Language
3. LEO Program is deployed to Aleo Chain
4. User interacts with the LEO Program
   
Although the library is sufficient to test a few models, it posists the following limitations

1. Variety in Model: LEO Program Runner offers limited vairiety of machine learning modules and cannot cover deep learning models. Considering the trend of AI where most of sophisticated models are based on deep learning, it significanly reduces the possibilty of zkML on Aleo Ecosystem   
2. Program Orchestration: Transpiled LEO programs are written as a full end program. In this case, it is more difficult for developers to utilize the pre-existing modules by calling it to their own programs

For the consumer grade machine learning applications, we need a better sets of tools for developers to run more sophisticated models on Aleo Ecosystem.

# Solution
![Model Overview](https://github.com/user-attachments/assets/bc62c30e-c644-4064-ba88-9deba267d453)


LionML library provides a new framework for zkML inference that operates

- Offchain Proving: Any computational model, including Pytorch(superstar at ml) and Tensorflow, can be pointed to and the proof can be generated through **Varuna native low-level circuit**. The lirary modifies ezkl library by substituting halo2 proof system with Varuna circuit so that .onnx file can be converted and deployed to SnarkVM. 

- Onchain Verification: Deployed model will be designated with annotation. We will deploy a LEO Complier such that annotated models on SnarkVM can be called from LEO program

EZKL library provides computational graphs for the developers to deploy more variety of models direclty on SnarkVM. Also, individual developers can orchestrate more freely with deployed models by calling it during the LEO program deployment. 

# System Architecture
### SanrkVM Deployment
The structure of [ezkl library](https://github.com/zkonduit/ezkl) is as follows:
1. Define a computational graph, for instance a neural network (but really any arbitrary set of operations), as you would normally in pytorch or tensorflow.
2. Export the final graph of operations as an .onnx file and some sample inputs to a .json file
3. Point ezkl to the .onnx and .json files to generate a ZK-SNARK circuit

Theortically, computational graphs like pytorch or tensorflow should be able to point to Varuna Circuit. To check if there can be a side effect, we took a peak into Varuna circuit and AVM Opcode

![Varuna R1CS linear combination circuit](https://github.com/Ludium-Official/LionML/blob/main/images/Varuna%20R1CS%20linear%20combination%20circuit.png?raw=true)

![AVM Opcode](https://github.com/Ludium-Official/LionML/blob/main/images/AVM%20Opcode.png?raw=true)

- SnarkVM include both AVMOpcode, Varuna R1CS Circuit. **In short, both can be proved and executed!**
- In addition, SnarkOS generates proof on client/router in the following order
  check_transaction_basic → check_internal_transactions → check_internal_transaction → check_excution_internal→verify_excution
  → Trace::verify_execution_proof(verify_batch call and check global state root during execution) → Self::verify_batch
  → Verifying_Key::verify_batch(SNARK proof logic, include Varuna::<N>::verify_batch)

```
        if self.ledger.check_transaction_basic(&transaction, None, &mut rand::thread_rng()).is_ok() {
```

- Trace::verify_execution_proof checks the state  root. But the Subroutine that computes ZKML does not occur transistion
- Thus, Varuna::<N>::verify_batch can sufficiently infer the zkML execution 

### Integration with LEO Program

![LEO Program Integration](https://github.com/Ludium-Official/LionML/blob/main/images/LEO%20Program%20Integration.png?raw=true)

- ML model is transpiled to .rs file via varuna-r1cs circuit
- The file is deployed to SnarkVM submodule via with Annotation (There should be no side effect)
- LEO program calls the desired ML model by calling the Annotation Number
  ![LEO Program Example](https://github.com/Ludium-Official/LionML/blob/main/images/LEO%20Program%20Example.png?raw=true)
- ML model output is used on the LEO program

# Impact

LionML library should be useful for the two types of developers 

1. ML Transpiler: Developer who wish to upload their desired ML model for inference through Aleo Prover Network can utilize the library and upload it to SnarkVM
2. ML Application Developer: Aleo application developers can call the deployed ML model through annotation and implement their business logic


# Roadmap

### Scope

- Phase 1 - Quick Start
    - Objective: Build a testable framework based on minimal data
    - Publish open sourced LionML library
    - Publish Bench mark Varuna-ZKML Performance
    - Provide example models that run on LionML
- Phase 2 - Aleo zkML Infrastructure
    - Objective: Build Intergration pipline and sub-proof module at prover client 
    - Make Intergration Logic at Aleo ZKVM
    - Make Prover network can proof ZKML model easily with LionML library
- Phase 3 - Aleo zkML Application
	- Objective: More dataset / product level UI to start the service adoption
	- Build an application utilizing models on LionML
*  Phase 4 - Machine Learning on Aleo
    - Objective: Implement zkML model learning to be possible on Aleo
    - Publish a novel zkML Training circuit model
### Budget and Milestones - Quick Start

|**Category**|**Justification**|**Time**|**Amount**|
|---|---|---|
|Planning|Analyzing Varuna / Marlin Protocol, SnarkVM for more comprehensive zkML possibility |2 Week|
|LionML Library |Publish LionML implementation with the instructions|3 Weeks|
|Agent Example|Local test at least two models with results on CLI command view. It requires SnarkVM module and LEO Compiler modification |5 Weeks|
|**Total**||10 Weeks|

# Team

- [Luke](https://github.com/nam2ee): Luke is the tech lead at BAY, Blockchain student club at Yonsei University. He won multiple hackathons on crypto + AI applications. He is also a contributor for Mina Foundation, contributing to zk education and application
- [Ludium](https://docs.google.com/presentation/d/15mmCJ2OYudZY1ncR8kX_eJsq8x8QaTjuOs80ep_TmwE/edit?usp=sharing): Ludium is Web3 builder community with 1,800 + active contributors. It provides opportunities for builders ranging from education, hackathon, to open source contribution based works.

# Questions / Requests
