# CONFORMANCE
practical verification framework for concurrent and distributed systems

- [Overview](#overview)
- [Automatic Formal Modeling based on LLMs and Agents](#llm)
- [Applications of the Conformance Framework](#app)

## <a id="overview"></a> Overview

(This is a *wrapper* repo for a series of our work on practical verification of concurrent and distributed systems.)

The CONFORMANCE framework is proposed for practical verification of concurrent and distributed systems.

The framework advocates the interaction between the *model* and the *code*.
As indicated by the name of the framework, the model and the code are expected and required to *conform* to each other.

### The *model* and the *code*

In the model layer, users (of the framework) formally specify the design of the target system, as well as the requirements (correctness conditions) the target system is expected to meet.

In the code layer, non-determinism in the system execution is controlled, whici enables effective and efficient interaction between the model and the code.

### The *interaction*

The model and the code interact basically in two ways:
- *model -> code*. &nbsp;&nbsp; The model layer execution (i.e. the model checking) is replayed at the code layer.
- *code -> model*. &nbsp;&nbsp; The code layer conducts standard test execution, employing various existing testing techniques. The model checks whether the code layer execution is permitted.

### Usage of the framework

The first thing using the framework is to ensure the conformance between the model and the code.
Various techniques, in both diretions, can be developed.

Given that the model accurately describe the code, typical useage ofthe framework include:

- Use model checking to find a bug, and replay the bug in the code layer to conform it.

- Test the system using existing techniques, and use the model as the test oracle.   

## <a id="llm"></a> Automatic Formal Modeling based on LLMs and Agents

### <a id="bench"></a> A Benchmark

Evaluating Agents on Formally Modeling Real-world Concurrent and Distributed Systems

| | |
|-|-|
| Arxiv | [https://arxiv.org/abs/2509.23130](https://arxiv.org/abs/2509.23130) |

### <a id="specula"></a> Specula

An Agentic Approach to Synthesizing High-Quality TLA+ Specifications from Source Code

| | |
|-|-|
| Repo | [https://github.com/specula-org/Specula](https://github.com/specula-org/Specula) |


## <a id="app"></a> Applications of the Conformance Framework

- [Practical Model Checking for Verifying Rust OS Kernel Concurrency](#atc25)
- [Practical model checking of the Zookeeper coordination service](#eurosys25)
- [Scalable Distributed System Model Checking with Specification-Level State Exploration](#eurosys24)
- [Model checking-driven explorative testing](#jsoftw24)

### <a id="atc25"></a> Practical Model Checking for Verifying Rust OS Kernel Concurrency

ASTERINAS is an open-source, general-purpose operating
system written in Rust, compatible with the Linux ABI, and
designed with a focus on reliability and security.

We developed a practical model-checking methodology,
CONVEROS, to verify the correctness of ASTERINAS concurrency modules such as synchronization primitives and critical thread-safety components. CONVEROS leverages the rigor of formal specifications and introduces a multi-layered, multi-grained specification approach to make writing scalable specifications practical, demonstrated in our case by writing PlusCal specifications for Rust code. It also makes conformance checking incremental and more automated to
detect specification-code discrepancies. 
While many formal methods are challenging to apply due to complexity and the expertise required, CONVEROS makes model checking cost-effective, accessible, and adaptable to evolving specifications and code. 

We applied CONVEROS to 12 critical concurrency modules, uncovering 20 bugs that led to issues such as data races, deadlocks, livelocks, and kernel panics. With a specification-to-code ratio ranging from 0.3 to 2.3 and a verification effort of only four person-months, our results demonstrate the practicality and effectiveness of CONVEROS.

| | |
|-|-|
| Repo | The CONVEROS tool is not open-source. The issues reported are available in the [Asterinas](https://github.com/asterinas/asterinas) repo (see detailed info in our ATC'25 paper). |
| Paper | Ruize Tang, Minghua Wang, Xudong Sun, Lin Huang, Yu Huang, Xiaoxing Ma, Converos: Practical Model Checking for Verifying Rust OS Kernel Concurrency, in proc. of the 2025 USENIX Annual Technical Conference, Jul. 2025. |

### <a id="eurosys25"></a> Practical model checking of the Zookeeper coordination service

This project presents our experience specifying and verify-
ing the correctness of ZooKeeper, a complex and evolving
distributed coordination system. 

We use TLA + to model fine-
grained behaviors of ZooKeeper and use the TLC model
checker to verify its correctness properties; we also check
conformance between the model and code. 

The fundamental
challenge is to balance the granularity of specifications and
thescalabilityofmodelchecking—fine-grainedspecifications
lead to state-space explosion, while coarse-grained specifica-
tions introduce model-code gaps. To address this challenge,
we write specifications with different granularities for com-
posable modules, and compose them into mixed-grained
specifications based on specific scenarios. For example, to
verify code changes, we compose fine-grained specifications
of changed modules and coarse-grained specifications that
abstract away details of unchanged code with preserved
interactions. 

We show that writing multi-grained specifica-
tions is a viable practice and can cope with model-code gaps
without untenable state space, especially for evolving soft-
ware where changes are typically local and incremental. We
detected six severe bugs that violate five types of invariants
and verified their code fixes; the fixes have been merged to
ZooKeeper. We also improve the protocol design to make it
easy to implement correctly.

| | |
|-|-|
| Repo | [https://github.com/Disalg-ICS-NJU/zookeeper-tla-spec](https://github.com/Disalg-ICS-NJU/zookeeper-tla-spec) |
| Paper | Lingzhi Ouyang, Xudong Sun, Ruize Tang, Yu Huang, Madhav Jivrajani, Xiaoxing Ma, Tianyin Xu, Multi-Grained Specifications for Distributed System Model Checking and Verification, in proc. of the 20th European Conference on Computer Systems (EuroSys), Mar. 2025. |

### <a id="eurosys24"></a> Scalable Distributed System Model Checking with Specification-Level State Exploration

Implementation-level distributed system model checkers
(DMCKs) have proven valuable in verifying the correctness
of real distributed systems. However, they primarily focus
on state space reduction, and often have a bottleneck on an-
other crucial dimension: exploration speed. 

To scale DMCK,
we introduce SandTable, a technique for lifting state-space
exploration from the implementation level to the specifica-
tion level, and confirming bugs at the implementation level.
We made SandTable practical through a methodology consisting of four essential parts:
1. writing specifications that
adhere to the implementation,
2. checking conformance to enhance specification quality and reduce false positives and false negatives,
3. exploring the state space with heuristics for effectiveness and efficiency, and 
4. confirming bugs and verifying their fixes in the implementation.

We implemented SandTable with the design of transparently verifying unmodified distributed systems on POSIX systems. SandTable was integrated into eight well-established open-source distributed systems that implement consensus protocols such as Raft and Zab. SandTable identified 23 bugs in total, with 18 new bugs, 17 confirmed, and 13 fixed. SandTable demonstrates exceptional scalability, with one machine-day of specification-level exploration checking up to $10^9$ distinct states. Furthermore, specification-level exploration offers a significant speedup, 114× — 2989× faster than implementation-level exploration.

| | |
|-|-|
| Repo | [https://github.com/Disalg-ICS-NJU/SandTable](https://github.com/Disalg-ICS-NJU/SandTable) |
| Paper | Ruize Tang, Xudong Sun, Yu Huang, Yuyang Wei, Lingzhi Ouyang, Xiaoxing Ma, SandTable: Scalable Distributed System Model Checking with Specification-level State Exploration, in proc. of the 19th European Conference on Computer Systems (EuroSys), Apr. 2024. |

### <a id="jsoftw24"> Model checking-driven explorative testing
The CONFORMANCE framework originates from our work on tesing of CRDT implementations. TLA+ spec of the CRDT design is model-checked to generate test cases.

| | |
|-|-|
| Repo | [https://github.com/Disalg-ICS-NJU/CRDT-Redis](https://github.com/Disalg-ICS-NJU/CRDT-Redis) |
| Paper | Yuqi Zhang, Yu Huang, Hengfeng Wei, Xiaoxing Ma, Model-checking-driven explorative testing of CRDT designs and implementations, J. Softw. Evol. Process. 36(4) (2024). |
