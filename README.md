# LionML - zkML libary for Aleo

# Overview
<img width="635" alt="image" src="https://github.com/user-attachments/assets/8be47ec7-1e79-471e-9cd9-d4b16ee6eb1d">
LionML is a library and command-line tool for doing machine learning model inference and other computational graphs in a zk-snark specifically for **Aleo’s Varuna Circuit**. It branches off of the **[ezkl library by zkonduit](https://github.com/zkonduit/ezkl)** for Aleo developers so that generalized machine learning models can work on Aleo Ecosystem. 

# Problem

Aleo lacks zkml resources for developers. The current machine learning related libraries and toolings on [Awesome Aleo](https://github.com/howardwu/awesome-aleo?tab=readme-ov-file#machine-learning) are either outdated or limited in transpiling a few models that are not readily usable.
It requires Leo contract for every instance of ML for verification which makes it so inefficient for running a machine learning algorithm service using Aleo Chain. Simply transpiling to Leo program which is not specifically designed for only ML is inferior to **ML-purpose(like matrix operation) native low-level varuna circuit**.  

For the consumer ready level of machine learning application, it is matter of speed. The computing power of the prover alone is not sufficient to achieve the speed necessary for a comfortable user experience (UX). This is because ZKML SDK is currently transpiling to Leo. Therefore, we need to **create ZKML models using low-level circuits that are native to Varuna proof systems, which are at a lower level, and integrate them with Leo Application program module.**

# Solution

LionML library provides a new framework for zkML inference that operates

- Offchain Proving: Any computational model, including Pytorch(superstar at ml) and Tensorflow, can be pointed to and the proof can be generated through **Varuna native low-level circuit**. By leveraging **prover network** , Aleo can take ZKML-Application narratives for privacy-preserving manner. 

- Onchain Verification: The proof is verified through Validators and even Leo contract so that the results can be further used onchain.

# System Architecture

The Diagram below explains two cases

<img width="485" alt="image" src="https://github.com/user-attachments/assets/cfd71fd5-f3a7-4ef9-bcdf-35adad23f331">


<img width="492" alt="image" src="https://github.com/user-attachments/assets/54b07284-9e6d-45c3-83a2-35c04c3141ad">

### System Flow

LionML consists of the following elements:

- Pre-trained model: Prepare ML model implemented and trained by pytorch, tensorflow, etc ... 
- Pre-trained model to ONNX: Convert the model to ONNX, which is a universal ML model technology graph
- Varuna-native Circuit: Conver the ONXX to Varuna-native circuit which is more faster than LEO 
- Intergration with Leo application program(Optional): Intergration ML circuit with Leo Program module which is using ZKML in its logic.
- Deploy: After intergration, the package which is consist of ML circuit and Leo program will be deployed at Validator and Prover.
- Off-chain Computation: Prover network compute with its heavy computational power and makes proof in fast way(Thanks to the speed of the 'native' Varuna Circuit).

### Details

- Computational Algorithm: Machine learning algorithms like Pytorch and Tensorflow
- Proof Generation: Generate zk Proof using Aleo’s Varuna circuit
- Onchain Contract: Even LEO contract with `makeproof()` or `verify()` to verify the proof onchain %% 이 부분은 다시 생각해보기... %%

# Impact

- General Purpose Inference Library: Provide a library and a tool to generate proof on Aleo using any Machine Learning algorithm compatible with
- Private Machine Learning Apps on Aleo: Onchain verified machine learning allows running onchain programs without revealing the input data

# Roadmap

### Scope

- Phase 1 - Quick Start
    - Objective: Build a testable framework based on minimal data
    - Publish open sourced Aleo EZKL library
    - Publish Bench mark Varuna-ZKML Performance
    - Provide example models that run on Aleo EZKL
- Phase 2 - Aleo zkML Infrastructure
    - Objective: Build Intergration pipline and sub-proof module at prover client 
    - Make Intergration Logic at Aleo ZKVM
    - Make Prover network can proof ZKML model easily with EZKL library
- Phase 3 - Aleo zkML Application
	- Objective: More dataset / product level UI to start the service adoption
	- Build an application utilizing models on Aleo EZKL
*  Phase 4 - Machine Learning on Aleo
    - Objective: Implement zkML model learning to be possible on Aleo
    - Publish a novel zkML Training circuit model
### Budget and Milestones - Quick Start

|**Category**|**Justification**|**Time**|**Amount**|
|---|---|---|---|
|Planning|Analyzing / Writing the full implementation details for Aleo EZML|1 Week|**$1,000 $USD**|
|Library|Publish Aleo EZKL implementation with the instructions|2 Weeks|**$2,000 $USD**|
|Agent Example|Test at least two models with results on CLI command view|2 Weeks|**10,000 $ALEO**|
|**Total**||5 Weeks|**3,000 $USD + 10,0000 $ALEOt**|

# Team

- [Luke](https://github.com/nam2ee): Luke is the tech lead at BAY, Blockchain student club at Yonsei University. He won multiple hackathons on crypto + AI applications. He is also a contributor for Mina Foundation, contributing to zk education and application
- [Ludium](https://docs.google.com/presentation/d/15mmCJ2OYudZY1ncR8kX_eJsq8x8QaTjuOs80ep_TmwE/edit?usp=sharing): Ludium is Web3 builder community with 1,800 + active contributors. It provides opportunities for builders ranging from education, hackathon, to open source contribution based works.

# Questions / Requests

- Varuna Circuit code needs to be open for integration
