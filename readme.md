# ISAC 4096 Integrated Sensing Revolution with a Single Antenna! 16 APs handle distributed architecture, 500-second latency reduced to 10ms [with MATLAB]

For instance, a 4096-antenna GIGAMIO system with centralized processing requires 500 seconds to process a single frame. Not only can it not meet real-time communication requirements, but even basic signal response cannot be satisfied. Today's deep dissection of this MATLAB code presents a revolutionary solution: using a distributed architecture with 16 access points (AP) and 256 antennas, combined with ADMM consensus beamforming, the complexity is reduced by 256 times while reducing latency to 10-20ms. It also ensures both communication rate and perception accuracy.
Next, let's dissect the code logic line by line to understand this technological leap from "centralized computing explosion" to "distributed real-time response."
The core focus of the code is to verify the feasibility of a distributed architecture for the GIGAMIO ISAC. The full name of this code is "GIGA-MIMO ISAC: M=4096 DISTRIBUTED ARCHITECTURE DEMONSTRATION," which literally translates to "4096-antenna integrated communication and perception: distributed architecture demonstration." Its main goal is not just to simulate signals; it aims to prove:
•  
A distributed architecture can support a large-scale system with 4096 antennas;
•  
Communication and perception can cooperate efficiently within the same architecture (60% communication resources + 40% perception resources);
•  
Compared to centralized systems, it achieves exponential optimization in performance without degrading performance.
The code starts by clearly defining four key contributions, which are the "value anchors" of the entire simulation:
1  
A 256-fold reduction in computational complexity compared to centralized systems;
2  
Real-time performance: Single frame processing takes only 10-20ms (centralized requires 500 seconds);
3  
Enhanced multi-base station perception gain: Distributed observation improves detection rates for weak targets;
4  
Preservation of full FOV resolution: Through coherent combining technology, angle measurement precision is not sacrificed.
II. The full process of code execution: 7 key steps from configuration to verification
1. System Configuration: Define the "Hardware and Resource Rules" code for the integrated tactile-sensory system by using struct() to define the complete system parameters (params), which serve as the foundation for all subsequent simulations, each parameter aligning with real engineering scenarios.
•  
Hardware parameters: Total number of antennas M=4096 (64×64 planar array), 16 users (also as perception targets K=16), each user equipped with 8 receiving antennas (N=8);
•  
Frequency parameters: Carrier frequency fc = 13GHz (millimeter wave band, considering both communication bandwidth and perception resolution), Bandwidth BW = 500MHz (supporting ultra-high speed data transmission), Antenna spacing d = λ/2 (λ is the wavelength, optimized for signal coherence);
•  
Power and Noise: Transmitted power P_tx = 40W (suitable for large-scale array signal coverage needs), noise figure 7dB, calculate the noise power according to Boltzmann constant (k_B=1.38e-23) and room temperature (T0=290K), simulate real signal environment;
•  
Transduction resource allocation: α=0.6, i.e., 60% of resources are allocated for communication, and 40% for perception, to balance the performance of two core functions.
•  
Perception Boundary: The maximum detection distance is 500 meters, the maximum target speed is 50m/s, covering typical scenarios such as urban traffic and industrial park security.
These parameters are not set arbitrarily, but refer to 3GPP standards and industrial-grade deployment requirements, ensuring the reliability of simulation results.
2. Subarray Architecture Design: The core of distributed computing is the "reasonable partitioning" step, which is the soul of the entire scheme - code through the function `design_subarray_architecture()`, splits 4096 antennas into 16 APs (sub-arrays), with each AP equipped with 256 antennas, forming a "16×256" distributed layout.
Key Design Logic:
•  
Partitioning Principle: Ensure the independence of each sub-array while designing for "phase coherence", laying the groundwork for subsequent signal combination;
•  
Performance Prediction: Code outputs directly the "complexity reduction ratio" and "single frame processing latency", preliminarily verifying the feasibility of the architecture;
•  
Hardware Compatibility: Set the number of radio link parameters (params.MRF) in sync to ensure the architecture design matches hardware capabilities.
Final Output Clarity: 16 APs × 256 antennas, complexity reduced by 256 times, estimated single frame processing latency between 10-20ms, addressing the centralized computing power bottleneck from an architectural perspective.
3. Channel Generation: Simulate the performance of "signal transmission channels" in real environments, relying entirely on precise channel modeling. Code generates complex Gaussian random signals using `randn()`, constructing a full array channel (H_KN_M) with dimensions "K*N * M", and superimposing three major real channel characteristics:
•  
3GPP Path Loss: According to the UMa (Urban Macrocell) model, calculates the path loss dB value = 32.4+20log10(fc/1e9)+30log10(distance/1000), fitting actual propagation attenuation;
•  
Amplitude Correlation: Sets the antenna correlation coefficient to 0.7 (common value in large-scale MIMO), constructs the correlation model using the Toeplitz matrix (toeplitz), simulating the correlation of antenna signals in real environments;
•
Dimensional adaptation: Convert the "K*N*M" full channel to a simplified channel (H_full) of "M*K", facilitating subsequent subarray decomposition.
The whole process takes about 0.2 seconds, and the generated channel not only ensures the authenticity but also considers the simulation efficiency.
4. Channel Decomposition: The key to enabling each AP to have its "dedicated communication / perception channel" distributed architecture is to allow each AP to independently handle its own channel data. The code uses the subarray_channel_decomposition() function to split the full array of channels into 16 sub-channels (H_subarrays) for each AP, with two core design points:
•  
Phase Coherence Preservation: decomp_options.preserve_phase_coherence = true, ensuring the phase alignment of 16 AP signals for subsequent "coherent combination".
•  
The final output is 16 independent sub-channels, where each AP only needs to process the channel data of its corresponding 256 antennas × 16 users, significantly reducing computational complexity.
5. Distributed ADMM Beamforming: The "Collaborative Signal Processing" beamforming for 16 APs is the core algorithm of integrated sense and communication - it focuses signals on target users (communication) or targets (perception) by adjusting antenna weights. The code implements distributed beamforming using the "Alternating Direction Method of Multipliers (ADMM)", with the process as follows:
•  
Algorithm configuration: Maximum iterations 20, convergence threshold 1e-4, using "average consensus" (consensus_type='average') mechanism;
•  
Distributed Computing: 16 APs compute beamforming weights (W_subarrays) based on local subchannels, without requiring global data interaction.
•  
Consensus Synchronization: Through the dual variable update of ADMM, a weight consensus is achieved for 16 APs, ultimately generating the global beamforming matrix (W_global).
•  
Real-time verification: Code recording processing time, the final output of single frame processing delay is about 10ms, far lower than centralized 500 seconds.
This step directly verifies the feasibility of distributed algorithms: it achieves the synergy of 16 APs while avoiding the global computing pressure of a centralized approach.
6. Dual-Dimension Performance Analysis: The core value of the "dual compliance" code for communication and perception lies in demonstrating that the performance of a distributed architecture does not degrade, which is analyzed from two dimensions: Communication and Perception.
(1) Communication Performance: Rate, fairness, and signal-to-noise ratio fully meet the standards.
Total Rate: 50bps/Hz (converted to 500MHz bandwidth, i.e., 25Gbps), meeting the requirements of ultra-high speed communication;
•  
Single user rate: average about 3.1bps/Hz, fairness index between users 0.98 (close to 1, indicating uniform rate allocation);
•  
Signal-to-Noise Ratio (SINR): The minimum SINR is about 5dB, and the maximum SINR is about 45dB, covering the communication quality needs of users at different distances.
(2) Perception Performance: Detection and localization accuracy of dual-optimized code definition for 4 perception targets (different distances, speeds, RCS), compute the perception boundaries through isac_utilities.computeSensingBounds()
•  
Detection rate: 100% (4 targets all detected);
•  
Distance accuracy: Average distance estimation error (CRLB) of 0.001 meters, with extremely high positioning accuracy.
•  
Multi-site gain: Enhance the detection capability of weak targets by approximately 12dB through distributed observation.
(3) Efficiency Comparison: The distributed "cost reduction and efficiency enhancement" code also compares the core efficiency indicators between centralized and distributed systems.
•  
Calculations: Centralized approximately 6.9e10 operations, distributed about 2.7e8, a reduction of 256 times;
•
Latency: Centralized 500 seconds/frame, distributed 10ms/frame, 50,000 times faster;
•
Energy Efficiency: Distributed at 0.52Gbps/W, more energy-efficient than centralized, meeting green communication requirements.
Visualization output: Visualization of performance with 7 types of IEEE standard charts to intuitively display the performance for academic reporting and engineering verification purposes. Code automatically generates 7 specialized charts covering core dimensions such as architecture, performance, and perception results:
•
Figure 1: System architecture visualization (16 AP distributions, ADMM convergence curve, complexity comparison);
•
Figure 2: Communications performance metrics (single user rate, SINR, power allocation);
•
Figure 3: Radar distance - Doppler plot (visualizes target distance and velocity);
•
Figure 4: Beam pattern comparison (full array vs single AP, validates resolution without degradation);
•
Figure 5: Subarray analysis (single AP rate, phase coherence matrix, processing time splitting);
•
Figure 6: Statistical performance (SINR / rate CDF, relationship between array size and capacity);
•
Figure 7: Target detection visualization (target position distribution, distance - detection probability curve).
All charts are colored and formatted according to IEEE standards, directly applicable for papers or project presentations.
The practical value of the code: A "blueprint" for large-scale integrated systems in 5G-A/6G communication and perception. The significance of this code extends beyond "running a simulation"—it provides a direct reference for the engineering implementation of large-scale integrated systems in 5G-A/6G, including core application scenarios:
1. Intelligent Transportation
2. Implementation of Industrial Internet of Things (IIoT) with 16 APs deployed within the factory park, covering production workshops and storage areas: The communication layer can transmit real-time data from devices and high-definition surveillance videos; the perception layer can locate AGV robots and track the position of goods, achieving intelligent scheduling of production processes.
3. The urban security and emergency distributed APs form a "virtual radar network," balancing urban communication coverage with security monitoring: capable of real-time detection of personnel and vehicle flow in key areas, ensuring unimpeded rescue communication in emergency scenarios (such as fires or earthquakes), while also pinpointing the location of trapped individuals.
4.6G Prospective Layout As the requirements for antenna numbers (expected to break through ten thousands) and deep integration of sensory and communication are raised, centralized architectures will be completely unable to support them. This code's "distributed + ADMM consensus + coherent merging" approach will become the core technology route for large-scale integrated systems in 6G.
IV. Summary of Core Features in the Code
Architecture Innovation: A 16x256 distributed layout, breaking the computational bottleneck of 4096 antennas, reducing complexity by 256 times.
2  
Real-time: 10-20ms/frame processing latency, meeting the real-time requirements of communication and perception.
3  
Performance not degraded: Communication rate reaches 25Gbps, perception detection rate 100%, retaining full aperture resolution;
4  
Engineering-ready: Parameters align with 3GPP standards, charts conform to IEEE formats, ready for project implementation and academic research.
5  
Synergy of Sensory Coordination: A 6:4 resource allocation strategy to achieve efficient synergy between communication and perception without sacrificing any function.
The emergence of this code marks a crucial step in the transition from "theoretical feasibility" to "engineering implementation" for the GIGA-MIMO integrated system. In the future, with further optimization of distributed algorithms and reduction in hardware costs, systems with 4096 antennas will be able to be deployed on a large scale during the 5G-A era, completely changing the mode of integration between communication and perception.

## Images

![(ISAC)4096 Root Antenna: A Revolution in Integrated Sensing! 16 APs for Distributed Architecture, Time Latency of 500ms Down to 10ms [with MATLAB]](01.jpg)
![(ISAC)4096 Root Antenna: A Revolution in Integrated Sensing! 16 APs for Distributed Architecture, Time Latency of 500ms Down to 10ms [with MATLAB]](02.jpg)
![(ISAC)4096 Root Antenna: A Revolution in Integrated Sensing! 16 APs for Distributed Architecture, Time Latency of 500ms Down to 10ms [with MATLAB]](03.jpg)
![(ISAC)4096 Root Antenna: A Revolution in Integrated Sensing! 16 APs for Distributed Architecture, Time Latency of 500ms Down to 10ms [with MATLAB]](04.jpg)
![(ISAC)4096 Root Antenna: A Revolution in Integrated Sensing! 16 APs for Distributed Architecture, Time Latency of 500ms Down to 10ms [with MATLAB]](05.jpg)
![(ISAC)4096 Root Antenna: A Revolution in Integrated Sensing! 16 APs for Distributed Architecture, Time Latency of 500ms Down to 10ms [with MATLAB]](06.jpg)
![(ISAC)4096 Root Antenna: A Revolution in Integrated Sensing! 16 APs for Distributed Architecture, Time Latency of 500ms Down to 10ms [with MATLAB]](07.jpg)
![(ISAC)4096 Root Antenna: A Revolution in Integrated Sensing! 16 APs for Distributed Architecture, Time Latency of 500ms Down to 10ms [with MATLAB]](08.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
