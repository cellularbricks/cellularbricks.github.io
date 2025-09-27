<p align="center">
<video src="https://github.com/cellularbricks/cellularbricks.github.io/raw/refs/heads/main/brick_banner_text.mp4" type="video/mp4" autoplay="" muted="" playsinline="" loop="" style="margin: 0; width: 100%;"></video>
</p>

Rodrigo Moreno<sup>1</sup>, Andres Faina<sup>1</sup>, Shyam Sudhakaran<sup>1,3</sup>, Kathryn Walker<sup>1</sup>, and Sebastian Risi<sup>1,2</sup>

<sup>1</sup>IT University of Copenhagen
<sup>2</sup>Sakana AI
<sup>3</sup>Autodesk

# Summary

Biological systems possess remarkable capabilities for self-recognition and morphological regeneration, often relying solely on local interactions. Inspired by these decentralized processes, we present a novel system of physical 3D bricks—simple cubic units equipped with local communication, processing, and sensing—that are capable of inferring their global shape class and detecting structural damage. Leveraging Neural Cellular Automata (NCA), a learned, fully-distributed algorithm, our system enables each module to independently execute the same neural network without access to any global state or positioning information. We demonstrate the ability of collections of hundreds of these cellular bricks to accurately classify a variety of 3D shapes through purely local interactions. The approach shows strong robustness to out-of-distribution shape variations and high tolerance to communication faults and failed modules. In addition to shape inference, the same decentralized framework is extended to detect missing or damaged components, allowing the collective to localize structural disruptions and to guide a recovery process. This work provides a physical realization of large-scale, decentralized self-recognition and damage detection, advancing the potential of robust, adaptive, and bio-inspired modular systems.

For further details, please read our [technical report](https://arxiv.org/abs/2509.18659). Code will be made available now.

# Introduction

Many biological systems exhibit a remarkable capacity to accurately determine their anatomical structure. Through local communication and self-organization, groups of cells can assess whether they have correctly formed a target shape, such as an organ. Moreover, they can actively remodel body parts following injury. For instance, a salamander can regenerate a damaged tail that transforms into a functional leg, and simple organisms like Hydra and Planaria can fully restore their morphology, regardless of which part is lost. This ability to classify general anatomical features—rather than matching a fixed target shape—enables variability among individuals, enhancing the robustness of the process. While the overall function and design of an organ may be consistent across a species, its specific shape, size, or scale can differ from one individual to another.

Artificial systems composed of many physically distributed modules that can autonomously infer their structural class — without centralized control — would represent a significant step toward more adaptable, intelligent artificial  collectives. Such systems could enable powerful applications in smart materials and reconfigurable robotics, where global knowledge must emerge from local sensing and communication.  Motivated by the scalability and resilience of the collective intelligence of biological systems, we introduce a fully decentralized system in which hundreds of physically embodied "cellular" bricks  collectively classify their global shape and detect local damage. 

<p align="center">
<img width="90%"  alt="NCA for shape classification." src="https://github.com/user-attachments/assets/956c07df-0bbc-4588-a468-bfdc2f1603bc" />
 <figcaption><sub><Strong>Neural cellular automata for shape classification.</Strong> (A) Cellular brick module. (B) Examples of cellular bricks assembled into four different shapes. (C) Cells takes as input local information from their connecting neighboring cells and their hidden channels. Information is aggregated locally, enabling the object to recognize its particular shape over multiple iterations. (D) The local update rules are encoded with a neural cellular automata, a deep neural network.</sub> </figcaption>
</p>


The collective intelligence algorithm we developed for shape classification is built on the framework of [differentiable Neural Cellular Automata](https://distill.pub/2020/growing-ca/) (NCA) and [self-classifying collective systems](https://distill.pub/2020/selforg/mnist/), extended to operate in 3D and implemented on physical modular hardware. NCAs generalize traditional Cellular Automata, in which local update rules are typically hand-crafted, by instead learning these rules. Unlike traditional cellular automata (CA) that operate with discrete cell states and hand-crafted rules, NCAs use continuous-valued cell states, enabling end-to-end differentiability and compatibility with gradient descent–based learning algorithms. On a high level, each cell in our system is tasked to determine which type of shape it is a part of, solely based on communication with its local neighbors and its memory state. The update rules are parameterized by a deep neural network, consisting of 3D convolutional layers -- where the network outputs updates that are added to the state of each cell. In the work here, the cell collective is tasked to distinguish between objects resembling planes, chairs, cars, tables, houses, guitars, and boats. The cells are trained with cross entropy loss to predict the class label through gradient-based numerical optimization. 

Rather than matching against a single, predefined configuration, our system generalizes across entire classes of shapes — including different e.g. planes, tables, etc. This shift from precise self-recognition to high-level shape classification enables greater flexibility and tolerance to variation. Crucially, the system can detect structural inconsistencies caused by missing or faulty modules, using only local interactions and without requiring actuation or centralized sensing.

We validate our approach on real hardware,  demonstrating that shape classification is not only feasible in a fully distributed setting, but also robust under the kinds of imperfections and hardware constrains that arise in the physical world.  By enabling modular cellular bricks to autonomously infer their morphology and detect structural faults, this system brings us closer to creating artificial systems with some of the properties that helped biological organisms to strive — such as decentralized self-assessment, resilience to damage, and adaptive organization — without the need for centralized control or predefined templates. 

# Results

We evaluated our approach on simple cellular bricks, which are composed of printed circuit board (PCB) cubes with electrical connectors on their six faces, a microcontroller module, a LED to display color information (i.e. the class label outputted by the cell) and the electronic components necessary to power these components.  Multiple bricks can be stacked together to create different objects. These cellular bricks can communicate and aggregate information purely locally through a custom protocol (digital serial communication). Over multiple iterations they are tasked to globally convergence on the correct shape label. 

We conducted several large-scale experiments with more than 500 bricks in simulation and almost 200 physical bricks. In simulation, the approach is able to reach an overall accuracy score of 98.97%. 
To test how the approach works in the real world, we transferred the NCA trained in simulation onto the physical bricks and built four distinct shapes with numbers of bricks ranging from 26 for a guitar, to 197 for a round table. The cellular bricks reach consensus to which object class they think they belong to, successfully recognizing a plane, a guitar, a boat, and a table. The results show that the approach is remarkably robust, with a success rate of 100% for the four shapes we build. 

<img width="100%"  alt="Screenshot 2025-09-25 at 20 44 47" src="https://github.com/user-attachments/assets/f15cc2d1-246d-437c-b4cc-ee54b0eb8a6c" />

## Robustness to faulty cells

Biological systems are remarkably robust to damage, noise, and faulty components. From cellular networks in living organisms to entire anatomical structures, biological processes often continue functioning even when faced with partial failure or incomplete information. This resilience stems from the local, distributed nature of biological communication and decision-making, where cells collectively assess and respond to their environment through redundant, fault-tolerant interactions.

Inspired by this property, we conducted a series of experiments to evaluate the fault tolerance of our system under simulated communication failures. Specifically, we investigated how disabling a subset of cellular bricks—preventing them from sending or receiving messages—affects shape recognition accuracy and convergence speed. 

<p align="center">
<img width="80%" alt="Robustness to faulty cells" src="https://github.com/user-attachments/assets/5f609e47-9496-4e21-b9f6-94e9f4df45ba" />
 <figcaption><sub>We show examples of five guitars (left) and five planes (right) with different disabled cells (marked in red). Because of their design, the plane
is much more robust than the guitar, in which a single failure along the guitar neck can disrupt the classification process.
</sub> </figcaption>
</p>

The results show that most shapes maintain high recognition performance at 5% failure rates, suggesting that the system exhibits a level of redundancy that contributes to its robustness. Notably, some shapes, such as the plane and boat, showed only minimal degradation in classification accuracy even at 15% failure rates, demonstrating strong collective robustness. However, shapes with narrow structural bottlenecks, like the guitar, were more sensitive to localized faults. In these cases, failure of a single module in the neck region could sever connectivity between subparts of the shape, leading to misclassification or delayed convergence. These findings support the hypothesis that local, learned communication rules—like those in multicellular organisms—can lead to globally coherent and robust behavior in modular cellular systems, even under imperfect conditions.

## Robustness to out-of-distribution shape variations

In biological systems, collective structures and functions are typically robust to variations in morphology, size, and proportion. Organs may vary in shape across individuals, limbs may regenerate at different scales, and yet the overall anatomical identity and function are preserved. This capacity for generalization—recognizing a category of form rather than a precise template — is fundamental to biological development and repair. Inspired by this principle, we evaluated whether our system could similarly generalize beyond the specific examples it encountered during training.

To assess this, we designed a series of test shapes that introduce novel variations within known shape classes. First, we modified a table instance from the training set by removing parts from two sides and altering the design to include five legs placed at random positions and shortened in length. This tests the model’s ability to handle asymmetry and irregular support structures. Next, we modified a known boat configuration by shifting the central bridge structure from the middle to an off-center position, testing sensitivity to internal rearrangement. Finally, we evaluated the system on scaled-down versions of both the plane and table configurations, challenging the NCA’s capacity to infer shape class under global size reduction. 


<div style="display: flex; align-items: center; gap: 1rem;">
  <video src="https://github.com/cellularbricks/cellularbricks.github.io/raw/refs/heads/main/ood_table.mp4" 
         type="video/mp4" autoplay muted playsinline loop 
         style="width: 30%; border-radius: 15px;">
  </video>
  <p style="flex: 1;">
    The results demonstrate that the system can successfully classify several of these novel configurations, confirming a degree of invariance to geometric variation and scale. For instance, the altered table with five legs was still correctly classified as a table, and the shifted boat bridge did not significantly impair recognition. These behaviors suggest that the distributed representations learned by the NCA capture abstract structural features rather than overfitting to specific examples—mirroring biological systems' capacity to recognize morphologies despite developmental variability.
  </p>
</div>

However, the system is not immune to failure. The scaled-down table, for example, was misclassified as a chair. This may reflect a limitation in spatial resolution or context available at the smaller scale, where reduced module count compresses structural cues. Such misclassifications highlight opportunities for improving the system’s scale invariance and further aligning its behavior with biological robustness.

Overall, these experiments underscore the potential of local, learned communication rules to support generalizable morphological inference—an essential step toward building artificial systems that exhibit the flexible, fault-tolerant pattern recognition found in natural development.

## Emerged communication strategies 

Collective systems can come up with efficient strategies to reach their goal. In biological organisms, this is exemplified by how cells coordinate during development to form complex structures without central control. Cells make local decisions based on their environment, often guided by morphogens—diffusible molecules that form gradients across developing tissues. These gradients provide positional information, enabling cells to infer their location within the organism and adopt appropriate identities, such as becoming part of a limb or an organ. This decentralized yet coordinated decision-making process inspired our investigation into the communication strategies developed by NCAs to recognize and differentiate shapes. Specifically, we drew inspiration from the role of morphogens in biology and examined the activation patterns of the NCA’s hidden channels. We can see that the approach often establishes left-right morphogen-like and radial patternings early on, resembling the emergence of developmental axes in embryos.

<p align="center">
<video src="https://github.com/cellularbricks/cellularbricks.github.io/raw/refs/heads/main/hidden_channel_fast.mp4" type="video/mp4" autoplay="" muted="" playsinline="" loop="" style="margin: 0; width: 100%;"></video>
</p>

How is the approach able to tell apart tables from chairs? Looking at the morphogen development for a chair object can give some insights. Similar to the tables, many cells in the chair are initially classified as a plane (green). However, unlike in the table case, an anterior-posterior patterning is established (channel 21), akin to the biological head-to-tail axis. This pattern is mirrored in the classification of the cells, which initially identify the backrest and seat as separate table-like components. Over time, the signal propagates from posterior to anterior, guiding the cells to reach consensus that they form a chair. These observations suggest that the default classification for tables and chairs is initially “table,” but morphogen-like signals originating from the backrest region of chairs gradually induce a reclassification in neighboring cells, leading to a coherent identification of the object as a chair.

<img width="100%" alt="Hidden channel activations" src="https://github.com/user-attachments/assets/f33e1f8f-b26e-4029-86ed-56163387fa92" />

## Damage detection and recovery 

An exciting possibility is to not only classify  which shapes  the  cell is a part of but also for each cell to determine if any damage  occurred to its neighbors. To eliminate ambiguity (e.g. a particular damaged table might look like an undamaged chair), here we conditioned the NCA on a particular shape instead of a general class.  To do this, we simply replaced the class embeddings with a compact 3D convolutional encoder that takes in a voxelized shape and returns an embedding.  Given this conditioning, each cell was trained to predict a single damage direction through cross-entropy loss. Specifically, each cell was classified as either not having any damaged neighbors or as having damage in the -X, +X, -Y, +Y, -Z, or +Z direction. 

For each training iteration, we sampled random shapes, damaged them, and ran the NCA for a random number of steps (between 64 and 96). The last channels for each cell was treated as logits for classification, and the model was optimized  using the labels generated by the synthetically damaged shapes.  We achieved a damage detection accuracy of more than 90% for all the different types of shapes it was tested on. 

Can we exploit this ability to also recover from damage, instead of merely predicting it? In this regard, biological systems possess remarkable capabilities, often through distributed, decentralized processes of sensing and regeneration. For example, organisms like planarians and axolotls exhibit the ability to regrow complex body parts by coordinating cell behavior in response to injury, even in the absence of a central controller. These processes involve local detection of damage, activation of repair pathways, and iterative remodeling until the original structure is restored. 

To test the ability of our system to guide damage recovery, we started from a small cluster of cells, and added cells in the direction determined by the existing cells; this process was repeated  until no more damaged was detected.

<div align="center">
  <img src="https://github.com/user-attachments/assets/ca4eb13c-9b43-4c97-b63b-27fcdef2ea08" width="30%" />
  <img src="https://github.com/user-attachments/assets/7b98e8e7-70e8-4dcb-ae7e-f78f14afcd05" width="30%" />
</div> 

Surprisingly, without ever being trained with only a few cells, the model was able to detect damage and recover almost all shapes across all object classes with a high accuracy.  We compared the approach across different cell hidden dimension sizes and found that models with larger hidden states performed  significantly better. These results are likely explained by the fact that the larger hidden states allow the models to capture more information across development. 

# Conclusion

The experiments presented here demonstrate that learned, fully-decentralised control enables large assemblies of simple, identical modules to reach a coherent understanding of their global morphology and to pinpoint structural faults, using nothing more than short-range communication and on-board computation. In hardware trials with almost 200 cellular bricks, the collective converged on the correct class for 3D shapes such as a plane, table and car in fewer than 60 update cycles—about three minutes of real time. Extending the same NCA to localize missing modules shows that damage detection and even damage recovery can be achieved with exactly the same infrastructure and identical code on every cube. Taken together, these results constitute the first physical realisation of large-scale, bio-inspired self-recognition in three dimensions. By allowing decentralized self-recognition, the approach thus takes a step towards the major goal of developing modular self-reconfigurable systems. 

## Relation to biological and robotic systems

Unlike previous modular-robot platforms that depend on centralised computation or handcrafted rules, our approach mirrors the decentralised strategies seen in morphogenetic processes—from planarian regeneration to salamander limb regrowth—where global form emerges from purely local interactions. Kilobots and similar swarms have illustrated consensus in two dimensions, but they require line-of-sight signalling and prescriptive behaviour tables; their extension to 3D structures has proved challenging. By coupling 3D NCAs with minimal six-face connectivity, we close this gap and show that a biologically plausible mechanism scales to hardware assembled from off-the-shelf microcontrollers and passive connectors. Moreover, the collective generalises beyond the discrete instances encountered during learning: shortened table legs and rescaled vehicles were still classified correctly, highlighting an ability to infer shape categories rather than match one-to-one templates. Such categorical reasoning is fundamental to developmental robustness in living tissues, and its replication in machines opens a path toward morphologically resilient artefacts.

## Robustness and emergent communication

The network tolerates realistic imperfections. For some shapes, classification convergence is still high, even with up to 15% of modules rendered silent—either through message loss or complete failure. Inspired by morphogen-driven development in organisms, we observed that hidden channels within the NCA form spatial gradients reminiscent of biological axes, enabling cells to coordinate classification decisions without centralized control. The emergence of consistent left-right and radial patterns in tables, and an anterior-posterior pattern in chairs, suggests that the NCA leverages internal morphogen-like signals to disambiguate similar shapes. Notably, the gradual reclassification of chair components from "table" to "chair" highlights a dynamic, context-sensitive communication process, mirroring how biological cells interpret positional cues. These results support the idea that biologically inspired mechanisms can guide the design of robust and interpretable collective systems.

## Limitations

Incorporating actuation, e.g. magnetically docked milli-scale blocks or lattice-based walkers, will be required for active self-repair in the future. Additionally, miniaturisation of the electronics and refinement of the mechanical connectors would permit denser collectives and more organic geometries, bringing the platform closer to the cellular scale of its biological inspiration. Closed-loop growth, whereby bricks autonomously recruit additional bricks from a pool, could transform the current recognition capability into full morphological regulation. 

<p align="center">
<video src="https://github.com/cellularbricks/cellularbricks.github.io/raw/refs/heads/main/big_hero.mp4" type="video/mp4" autoplay="" muted="" playsinline="" loop="" style="margin: 0; width: 100%;"></video>

 <figcaption><sub>A self-assembling structure made out of many modular robots. Scene from the movie Big Hero 6.
</sub> </figcaption>
</p>

Want to learn more about self-organizing AI and collective systems? Check out this [blog post](https://sebastianrisi.com/self_assembling_ai/) and this [survery article](https://arxiv.org/abs/2111.14377) of recent developments in collective intelligence for deep learning. 



