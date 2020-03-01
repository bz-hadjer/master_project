**Table of Contents**

- [📝 Notes](#-notes)
  - [IDS Classification](#ids-classification)
    - [By location ( source of data)](#by-location--source-of-data)
    - [By technique](#by-technique)
  - [Threats](#threats)
  - [Datasets](#datasets)
  - [Used ML Techniques](#used-ml-techniques)
- [📑 References](#-references)
- [🛠 Tools](#-tools)
- [💡 Ideas](#-ideas)

# 📝 Notes

## IDS Classification

Mainly every single paper includes this classification such as [P01](references/1806.03517.pdf), [P02](references/1701.02145.pdf), [P03](references/algorithms-10-00039-v2.pdf)

### By location ( source of data)

1. **Host based (HIDS)**

   - Monitor host system log file.
   - Advantage of being able to detect threats from within by scanning through the traffic activities before sending and receiving data
   - Its main disadvantage is that only monitors the host computer, meaning it has to be installed on each host

2. **Network based (NIDS)**

   - Monitor network traffic.
   - Advantage of having a single system monitoring an entire network, saving time and cost of installing software on each host
   - Disadvantage is its vulnerability to internal intrusions.

3. **Hybrid**

Table 1, page 04 from [P02](references/1701.02145.pdf) , provides a a performance comparison of HIDS and NIDS.

### By technique

1. **Signature / Misuse based detection**

   - Defines a set of rules used to match the patterns in the network traffic. If a mismatch is detected it raises an alarm
   - Advantage of being able to detect attacks giving a low false positive detection ratio.
   - Drawback of being able to detect only attacks known to the database

2. **Anomaly based detection**

   - Depends on identifying patterns and comparing them to normal traffic patterns.
   - Can detect attacks with unknown patterns
   - However, the false positive rate is often high.
   - ML techniques falls into this category.

3. **Specification / Hybrid Based**

   - It is used for detecting firstly known and then unknown attacks.

## Threats

- [P01](references/1806.03517.pdf) provides taxonomy of threats based on the affected layer on the OSI model, the type of attack active/passive ..., recent updates can be found in their [github repo](https://github.com/AbertayMachineLearningGroup/network-threats-taxonomy)

- [P03](references/algorithms-10-00039-v2.pdf) provides the classification of threats based on the CIA triangle.

- The type of threats to defense against is highly related to the chosen dataset.

## Datasets

- The data used to train an IDS algorithm are normally provided as a dataset of recorded network features with their associated intrusion labels.
- [P04](references/Survey_DataSets.pdf) is a full survey about NIDS datasets, it provides a comparison between 34 datasets and some good recommendations.
- Network-based data appear in packet-based or flow-based format:
  1. **packet based data**: is commonly captured in pcap format and contains payload.
  2. **flow-based data**: aggregate all packets which share some properties within a time window into one flow and usually do not include any payload.
- There are some datasets that are neither purely packet-based nor flow-based.
- The most used datasets are:

  1. **KDD CUP 99** which is criticized for the large amount of redundancy and considered as deprecated
  2. **NSL-KDD** enhanced the KDD CUP 99 dataset and it is considered as a good benchmark dataset to compare results between researchers, although it data go back to 1998.

- The authors of [P04](references/Survey_DataSets.pdf) give a general recommendation for the use of:

  - **CICIDS 2017**: contains a wide range of attack scenarios, [link](https://www.unb.ca/cic/datasets/ids-2017.html)
  - **CIDDS-001**: contains detailed metadata for deeper investigations, [link](https://www.hs-coburg.de/index.php?id=927)
  - **UGR’16**: stands out by the huge amount of flows, [link](https://nesg.ugr.es/nesg-ugr16/index.php)
  - **UNSW-NB15**: contain a wide range of attack scenarios, [link](https://www.unsw.adfa.edu.au/unsw-canberra-cyber/cybersecurity/ADFA-NB15-Datasets/)
  - **CTU-13**: increasing age , [link](https://mcfp.weebly.com/the-ctu-13-dataset-a-labeled-dataset-with-botnet-normal-and-background-traffic.html)
  - **ISCX 2012**: increasing age [link](https://www.unb.ca/cic/datasets/ids.html)
  - **AWID**: better suited for certain evaluation scenarios (wireless network), [link](http://icsdweb.aegean.gr/awid/index.html)
  - **Botnet**: better suited for certain evaluation scenarios, [link](https://www.unb.ca/cic/datasets/botnet.html)

- The University of New Brunswick (Canada) provides a good set of datasets in thier [website](https://www.unb.ca/cic/datasets/index.html)
- Some IoT specific databases:

  - **IoT Network Intrusion Dataset** : it comes in raw pcap format [link](http://ocslab.hksecurity.net/Datasets/iot-network-intrusion-dataset)

- I think in order to deploy the trained model in real world, the chosen dataset should come with a script ( or just an explained method ) how the features are extracted from the raw packets.

## Used ML Techniques

The applied machine learning techniques varies from supervised, unsupervised, semi-supervised, RL and DRL!!!

- [P05](references/document.pdf) do not analyze network traffic or series of system calls. Instead, they process alerts generated by sensors (IDSs such as Snort, applications, etc).
- [P06](references/lopez-martin2019.pdf) presents how to perform supervised learning based on a DRL framework.

  - DRL is a type of reinforcement learning (RL) which uses deep learning models (e.g. NN, convolutional NN…) as function approximators for the policy and/or value functions used in RL.
  - They apply 4 different DRL algorithms (DQN, DDQN, PG amd AC) using a dataset of labeled samples without interacting with a live environment where:
    - the states to network traffic samples,
    - the actions to label predictions,
    - the rewards to a value associated with the goodness of the prediction, i.e. classification accuracy (better prediction implies a greater reward value).
  - They choose NSL-KDD and AWID datasets
  - DRL, with their model and some parameter adjustments:
    - can improve the results of intrusion detection in comparison with current machine learning techniques
    - the classifier obtained with DRL is faster than alternative models.

- Similar to [P06](references/lopez-martin2019.pdf), [P07](references/caminero2019.pdf) propose a model base on DRL but with 2 agents:

  - a classifier agent
  - an environment agent: choose samples from the dataset that is hard for the classifier to correctly predict.

- The work in [P07](references/caminero2019.pdf) AE-RL (Adversarial Environment RL) is very interesting and all code is available in their [github reop](https://github.com/gcamfer/Anomaly-ReactionRL)

- [P08](references/1906.05799.pdf) presents a survey on Deep Reinforcement Learning for Cyber Security and highlight attacks in adversarial environment where **hackers can take advantages of AI to make attacks smarter and more sophisticated to bypass detection methods to penetrate computer systems or networks**.
- [P09](references/1911.02621.pdf) presents a survey about the Threat of Adversarial Attacks on Machine Learning in Network Security:
  - The attacker can apply some small changes to the input data that is possible to induce a wrong prediction from the machine learning model
  - This domain is well known and applied in Image classification, but its application in Cyber Security is not that well explored.
  - Machine learning applications in network security such as malware detection, intrusion detection, and spam filtering are by themselves adversarial in nature.
  - [P10](references/sommer2010.pdf) propose a model for generating adversarial attacks targeted towards intrusion detection systems, that model is based on GAN (Generative adversarial network)
  - One of the defenses against adversarial attacks is **Adversarial Training** which improves the classification performance of the machine learning model and makes it more resilient to adversarial crafting, it is done in 3 steps:
    1. Train the classifier on the original dataset
    2. Generate adversarial samples
    3. Iterate additional training epochs using the adversarial examples.

# 📑 References

| Code                                         | Title                                                                                                       |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| [P01](references/1806.03517.pdf)             | A Taxonomy and Survey of Intrusion Detection System Design Techniques, Network Threats and Datasets         |
| [P02](references/1701.02145.pdf)             | Shallow and Deep Networks Intrusion Detection System: A Taxonomy and Survey                                 |
| [P03](references/algorithms-10-00039-v2.pdf) | From Intrusion Detection to an Intrusion Response System: Fundamentals, Requirements, and Future Directions |
| [P04](references/Survey_DataSets.pdf)        | A Survey of Network-based Intrusion Detection Data Sets                                                     |
| [P05](references/document.pdf)               | An approach to the correlation of security events based on machine learning techniques                      |
| [P06](references/lopez-martin2019.pdf)       | Application of deep reinforcement learning to intrusion detection for supervised problems                   |
| [P07](references/caminero2019.pdf)           | Adversarial environment reinforcement learning algorithm for intrusion detection                            |
| [P08](references/1906.05799.pdf)             | Deep Reinforcement Learning for Cyber Security                                                              |
| [P09](references/1911.02621.pdf)             | The Threat of Adversarial Attacks on Machine Learning in Network Security - A Survey                        |
| [P10](references/1809.02077.pdf)             | IDSGAN: Generative Adversarial Networks for Attack Generation against Intrusion Detection                   |

# 🛠 Tools

- **Packet Tracer** for network simulation
- **Wireshark** for packet analyzing

# 💡 Ideas

1. Increase the robustness of IDS using adversarial attacks we can use GANs for that.
2. An adversarial multi agent RL (an ids agent and an attacker) inspired from the [Hide & Seek example](https://www.youtube.com/watch?v=kopoLzvh5jY) from OpenAI.
