Please write a script of a podcast episode using the following information using the information below. The title of podcast is "Life is Artificial". 
This is the technical deep dive into Cerabras super computer CS-2 and its chip, WSE-2. The target audience is people who are interested in investing to Cerabras. We want to highlight the competitive advantage of Cerebras solution over the industry standard NVIDIA GPU. 


# IPO

AI chipmaker Cerebras is trying to be the first major venture-backed tech company to go public in the U.S. since April and to capitalize on investors’ insatiable demand for Nvidia
, now valued at $3.3 trillion.

While its position in artificial intelligence infrastructure represents a major tail wind, Cerebras has challenges — most notably a hefty reliance on a single Middle Eastern customer — that may prove too weighty to overcome in the company’s attempt to ride the Nvidia wave. Valued at $4 billion in 2021, Cerebras is reportedly seeking to roughly double that in its IPO.

# Cerebras CS-2

The CS-2 is a system solution that consists of innovations across three dimensions: a) the second
generation Cerebras Wafer Scale Engine (WSE-2) — the industry’s largest and only multi-trillion-
transistor processor, b) the Cerebras System and c) the Cerebras software platform.

The CS-2 is 26-inches (15 rack units) tall and fits in one-third of a standard datacenter rack.

WSE-2 has 2.6 trillion transistors.

The WSE-2 covers 46,255 square millimeters — 56 times larger than the largest graphics processing unit.
With 850,000 cores, 40 Gigabytes of on-chip SRAM, 20 petabytes/sec of memory bandwidth, and 220
petabits/sec of interconnect bandwidth, the WSE-2 contains 123 times more compute cores, 1,000 times
more high-speed on-chip memory, 12,862 times more memory bandwidth and 45,833 times more fabric
bandwidth than its graphics processing competitor

Computation inside the WSE-2 happens in 850,000 AI-optimized Sparse Linear Algebra Compute
(SLAC) cores. The SLAC cores are designed for the sparse linear algebra primitives that underpin
all neural network computation. This programmability ensures cores can run all neural network
algorithms in the constantly changing machine learning field.

The WSE-2 has 40 Gigabytes of on-chip memory, all uniformly distributed alongside the cores, and
20 Petabytes/sec of memory bandwidth. As a result, the WSE-2 can keep the entire neural network
model in on-chip memory, all of the time, on the same piece of silicon as the compute cores. This
means that all model parameters can be accessed at extremely high bandwidth at single-cycle latency.

To supply power and signal to the world’s largest chip, the CS-2 uses tailored configurations of
standard parts and interfaces. In the top left of the system, as shown in Figure 4, twelve standard
power supplies (PSUs), in a 9+3 redundant configuration, deliver up to 23,000 watts to the WSE-2.
12 x 100 Gigabit Ethernet links sit above the PSUs to bring in up to 1.2 Terabits per second of
input data bandwidth from the surrounding datacenter infrastructure.

The rest of the system is dedicated to removing heat from the WSE-2. To provide the cooling
horsepower the WSE-2 needs while keeping datacenter integration simple, the CS-2 is internally
water-cooled. Water circulates through a closed loop, fully self-contained within the system. Like a
giant gaming PC, the CS-2 uses water to cool the WSE-2, and then air to cool the water.

In the engine block, thousands of watts delivered through the power pins are stepped down to the
sub-volt thresholds used by the WSE-2. Because the current density required is so high, the traditional
method of distributing power through the edges of the board results in too much dissipation at the
chip’s center. The custom packaging solution of the engine block instead brings power and data through
the main board, perpendicularly to the wafer face, rather than at the edges. A novel, flexible connector
between the silicon wafer and main board maintains electrical connectivity to the chip as the pieces
expand and contract at different rates when heated and cooled.

# Software

The Cerebras software platform allows machine learning (ML) researchers to leverage CS-2
performance without changing their existing workflows. Users can define their models using
popular ML frameworks such as TensorFlow and PyTorch. A flexible graph compiler automatically
converts these models into optimized executables for the CS-2, and a rich set of tools enables
intuitive debugging and profiling.

The Cerebras Graph Compiler
The Cerebras Graph Compiler (CGC) takes as input a user-specified neural network. For maximum
workflow familiarity and flexibility, researchers can use both existing ML frameworks — such as
TensorFlow and PyTorch and well-structured graph algorithms written in other general-purpose
languages, such as C and Python, to program the CS-2.
To translate a deep learning network into an optimized executable, GCC extracts a static graph
representation of the problem from the source language and converts it into the Cerebras Linear
Algebra Intermediate Representation (CLAIR). As ML frameworks evolve rapidly to keep up
with the needs of the field, this consistent input abstraction allows CGC to quickly support new
frameworks and features, without changes to the underlying compiler.
Once the CLAIR graph has been extracted, CGC performs a matching and covering operation that
matches subgraphs to kernels from the Cerebras kernel library. These kernels are optimized to
provide high-performance compute at extremely low latency on the fabric of the WSE-2. The result
of this matching operation is a kernel graph. CGC then allocates compute and memory to each
kernel in the graph and maps every kernel onto a physical region of the computational array of
cores. Finally, a communication path, unique to each network, is configured onto the fabric.
During this compilation process, kernel placement is formulated as a multi-constraint problem on
1) memory capacity and bandwidth, 2) computation requirements, and 3) communication costs.
The placement engine then takes into account both algorithmic efficiency and compute core
utilization to generate a result that maximizes locality, minimizes routing distances, and avoids
hotspots. The final result is a CS-2 executable, customized to the unique needs of each neural
network, so that all 850,000 SLAC cores and 40 Gigabytes of on-chip SRAM can be used at
maximum utilization towards accelerating the deep learning application.

Because of the massive size of the WSE-2, every layer in the neural network can be placed onto
the fabric at once and run simultaneously. The computation is parallel at three levels: within the
core, there is multiple operation per-cycle parallelism; across each fabric region, the cores can
work in parallel on one layer; and all layers can run in parallel on separate fabric regions. This
approach to whole-model acceleration is unique to the WSE-2 — no other device has sufficient
on-chip memory to hold all layers at once on a single chip, or the enormous high-bandwidth and
low-latency communication advantages that are only possible on silicon, to prevent bottlenecks
from arising when communicating between layers.

On other devices, hardware characteristics will typically constrain applicable distribution modes.
In contrast, CS-2’s memory and bandwidth advantages combined with the uniformity of the cores
mean that CGC can support any hybrid execution mode combining data-parallel, layer-parallel,
and layer-pipelining techniques. It can run in a traditional, layer-sequential mode to support
exceptionally large networks. It can combine model and layer-parallelism, with the entire model
spread across the cores of the WSE-2, and each layer running pipeline-parallel. It can map multiple
copies of a layer-parallelized model to the fabric at once, and train them all in a data-parallel
fashion. In summary, CGC can distribute a network across the WSE-2 with complete flexibility,
for maximum utilization, automatically — choosing the optimal distribution strategy to suit the
computational and memory requirements of a given model.
