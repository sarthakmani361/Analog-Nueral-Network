# Analog Neural Network

### Training Neural Networks Using Analog Circuits

An experimental **analog neural network (ANN)** implemented and simulated using **LTspice**, where neural-network operations such as weighted multiplication, summation, activation, and weight adaptation are realized using analog electronic circuits.

Instead of performing neural-network computation entirely in software, this project explores how fundamental neural-network operations can be implemented using **op-amps, analog multipliers, integrators, capacitors, and feedback circuits**.

The project demonstrates a hardware-oriented approach to neural computation and explores concepts relevant to **neuromorphic computing and analog AI accelerators**.

---

## 🧠 Project Overview

A conventional neural network performs operations such as:

```text
Weighted Sum → Activation → Output
```

This project maps these operations onto analog circuitry.

The implemented architecture consists of:

* Analog weight storage
* Analog multiplication
* Analog summation
* Nonlinear activation
* Error feedback
* Weight adjustment
* Multi-neuron network interconnection

The weights are represented as analog voltage levels stored using **capacitor-based integrator circuits**.

---

## 🏗️ Neural Network Architecture

The basic neuron follows:

```text
       Input x₁ ─────┐
                     │
       Weight w₁ ───►×─────┐
                           │
       Input x₂ ─────┐     │
                     │     │
       Weight w₂ ───►×─────┼──► Σ ──► Activation ──► Output
                           │
                          ...
                           │
       Input xₙ ─────┐     │
                     │     │
       Weight wₙ ───►×─────┘
```

The analog implementation replaces the mathematical operations with physical circuit blocks.

---

# 🔧 Circuit Implementation

## 1. Analog Synapse / Weight Storage

The synapse stores the weight associated with an input.

A capacitor-based integrator is used to maintain an analog voltage representing the weight.

The weight can be adjusted through an external control voltage.

Conceptually:

```text
Adjustment Signal
       │
       ▼
   Integrator
       │
       ▼
 Capacitor Voltage
       │
       ▼
   Stored Weight
```

This allows the circuit to behave as an analog memory element.

A key advantage of this approach is that the rate of weight adjustment can be controlled through the adjustment signal.

---

## 2. Analog Multiplication

The stored weight is multiplied by the corresponding neuron input using an **AD633 analog multiplier**.

The multiplication operation can be represented as:

```text
y = x × w
```

where:

* `x` = neuron input
* `w` = stored analog weight
* `y` = weighted input

The AD633 is used to implement this multiplication directly in the analog domain.

---

## 3. Analog Adder

The weighted inputs are combined using an op-amp-based summing circuit.

For a neuron with multiple inputs:

```text
S = x₁w₁ + x₂w₂ + ... + xₙwₙ
```

The resulting signal is then passed to the activation stage.

---

## 4. Activation Function

A nonlinear activation stage is implemented using analog circuitry.

The project investigates a **ReLU-like activation characteristic**.

Ideally:

```text
f(x) = max(0, x)
```

However, the practical analog implementation introduces limitations due to:

* Op-amp characteristics
* Supply voltage
* Circuit offsets
* Saturation
* Component limitations

Therefore, the implemented characteristic includes a small positive offset and an upper limit imposed by the available supply voltage.

---

# 🔄 Feedback-Based Training

One of the main aspects of the project is attempting to perform **neural-network weight training using analog feedback**.

The circuit compares:

```text
Expected Output
      │
      ▼
   ┌───────┐
   │ Error │◄──── Actual Output
   │ Amp   │
   └───┬───┘
       │
       ▼
 Adjustment Signal
       │
       ▼
 Weight Storage
       │
       ▼
 Updated Weight
```

The error between the expected and actual outputs is used to generate a weight-adjustment signal.

This creates a closed-loop learning mechanism.

---

## 🎯 Error Feedback

The training circuit compares the expected output `Vex` with the actual network output `Vac`.

The resulting error signal controls the adjustment of the analog weight.

A feedback network is used to limit the effective gain of the error amplifier and reduce excessive oscillations caused by the high open-loop gain of the op-amp.

This allows the weight to gradually converge toward a value that produces an output closer to the expected response.

---

# 🧩 Single Neuron Training

The project first investigates training of an individual neuron.

```text
Input
  │
  ▼
Weight × Input
  │
  ▼
Summation
  │
  ▼
Activation
  │
  ▼
Actual Output
  │
  ▼
Error Feedback
  │
  ▼
Weight Adjustment
  │
  └──────────► Weight
```

During simulation, the weight changes dynamically in response to the difference between the expected and actual outputs.

---

# 🌐 Multi-Neuron Network

The individual neuron circuit is modularized into a reusable **neuron unit**.

Multiple neuron units can then be interconnected to form a larger analog neural network.

The project includes a network-level LTspice schematic containing multiple neuron blocks.

```text
             ┌────────────┐
Input ──────►│  Neuron 1  │─────┐
             └────────────┘     │
                                │
             ┌────────────┐     │
Input ──────►│  Neuron 2  │─────┼──► Network Output
             └────────────┘     │
                                │
             ┌────────────┐     │
Input ──────►│  Neuron 3  │─────┘
             └────────────┘
```

The modular architecture makes it possible to experiment with larger analog neural-network configurations.

---

# 🧪 Simulation

All circuit simulations are performed using **LTspice**.

The project includes simulations for individual circuit components as well as complete neural-network configurations.

The training simulation applies an input signal and an expected output signal and observes the evolution of the analog weight and network output.

The simulation demonstrates the basic principle of **closed-loop analog learning**, where the circuit dynamically adjusts its stored weight based on the output error.

---

# 📁 Repository Structure

```text
Analog-Neural-Network/
│
├── README.md
│
├── ad633.cir
├── ad633.lib
│
├── Weight.asc
├── Adder.asc
├── Neuron.asc
├── neuron_unit.asc
├── Train.asc
├── Network.asc
├── Works.asc
│
├── Modular Neuron Tile.stl
│
└── Draft*.asc
```

### Circuit Files

| File                      | Description                          |
| ------------------------- | ------------------------------------ |
| `Weight.asc`              | Analog weight-storage circuit        |
| `Adder.asc`               | Analog summing circuit               |
| `Neuron.asc`              | Individual analog neuron             |
| `neuron_unit.asc`         | Modular neuron implementation        |
| `Train.asc`               | Single-neuron training circuit       |
| `Network.asc`             | Multi-neuron network                 |
| `Works.asc`               | Integrated working circuit           |
| `ad633.cir`               | AD633 multiplier model               |
| `ad633.lib`               | AD633 simulation library             |
| `Modular Neuron Tile.stl` | 3D model for modular neuron hardware |

---

# 🛠️ Technologies & Components

### Simulation

* **LTspice**
* SPICE circuit simulation

### Analog Components

* Operational amplifiers
* AD633 analog multipliers
* Resistors
* Capacitors
* Voltage sources
* Analog feedback networks

### Concepts

* Analog neural networks
* Neuromorphic computing
* Analog computation
* Neural-network training
* Weighted summation
* Analog multiplication
* Feedback control
* Capacitive analog memory
* Nonlinear activation functions

---

# ▶️ Running the Simulations

### Requirements

* LTspice
* AD633 SPICE model included in the repository

### Steps

1. Clone the repository.
2. Open LTspice.
3. Open the required `.asc` schematic.
4. Ensure the included AD633 model/library is available to LTspice.
5. Run the transient simulation.

For example, start with:

```text
Weight.asc
```

Then explore:

```text
Adder.asc
Neuron.asc
Train.asc
Network.asc
Works.asc
```

The circuits can be examined individually before running the complete network.

---

# 📐 Mathematical Model

For a neuron with inputs `x₁, x₂, ..., xₙ` and weights `w₁, w₂, ..., wₙ`, the weighted sum is:

```text
z = Σ(xᵢwᵢ)
```

The activation stage produces:

```text
y = f(z)
```

where `f(.)` represents the analog implementation of the activation function.

The feedback mechanism attempts to minimize the difference:

```text
e = y_expected − y_actual
```

The error signal is then used to adjust the analog weight storage elements.

---

# 🔬 Key Engineering Challenges

The project explores several challenges involved in implementing neural networks using analog hardware:

### Precision

Analog weights are represented by voltages and are therefore affected by:

* Component tolerances
* Noise
* Offset voltages
* Leakage
* Supply variations

### Stability

High-gain feedback can cause oscillations or overshoot. Feedback gain therefore needs to be controlled carefully.

### Weight Storage

Unlike digital memory, capacitor-based analog storage is affected by:

* Leakage
* Charging/discharging behavior
* Drift
* Time constants

### Activation Function

Implementing nonlinear functions in analog circuitry is significantly more complicated than evaluating them digitally.

---

# 🚀 Future Improvements

Potential extensions include:

* Fabrication of the analog neuron circuit
* PCB implementation
* Hardware-based weight programming
* Improved analog weight retention
* Multi-layer analog neural networks
* Improved activation circuits
* Hardware noise characterization
* Process and temperature variation analysis
* OTA-based multiplication and computation
* Low-power implementation
* CMOS implementation
* Memristor-based weight storage
* Comparison with digital neural-network implementations
* Hardware-in-the-loop training

---

# 🎓 Learning Objectives

This project provides practical experience with:

* Analog circuit design
* Neural-network fundamentals
* Op-amp circuits
* Analog multiplication
* Analog signal processing
* Feedback systems
* SPICE simulation
* Capacitor-based analog memory
* Hardware-oriented machine learning
* Neuromorphic computing

---

# 📚 References

The project draws upon concepts from research literature and standard analog-electronics references.

* IEEE research on analog neural-network implementations
* Springer literature on neural-network hardware
* **Fundamentals of Microelectronics — Behzad Razavi**
* Analog Devices documentation and LTspice resources

---

# 👨‍💻 Author

**Sarthak Mani**

B.Tech — Electronics & Communication Engineering

Interests:

**Analog & Mixed-Signal Electronics • VLSI • RF & Microwave • DSP • Neuromorphic Computing • AI/ML • Hardware Acceleration**

---

# 📜 License

This project is intended for educational and research purposes.

The included circuit designs and simulation files may be studied, modified, and extended with appropriate attribution.
