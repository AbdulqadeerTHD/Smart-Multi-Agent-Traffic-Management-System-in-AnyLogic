# Smart Multi-Agent Traffic Management System

A professional AnyLogic simulation model demonstrating intelligent multi-agent systems (MAS) for urban traffic management. This project implements autonomous traffic agents, adaptive traffic light control, and reinforcement learning-based optimization.

## Table of Contents

- [Project Overview](#project-overview)
- [Course Information](#course-information)
- [System Architecture](#system-architecture)
- [Components](#components)
- [Development Phases](#development-phases)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [MAS Concepts Demonstrated](#mas-concepts-demonstrated)
- [Project Structure](#project-structure)
- [Experiments & Scenarios](#experiments--scenarios)
- [Documentation](#documentation)
- [Team](#team)
- [License](#license)

## Project Overview

This project implements a comprehensive multi-agent traffic simulation system using AnyLogic 8.x. The system models an urban traffic network with multiple intersections, where intelligent agents (cars, traffic lights, pedestrians) interact, communicate, and make autonomous decisions to optimize traffic flow.

### Key Features

- **Multi-Agent Architecture**: Autonomous agents with reactive and deliberative behaviors
- **Adaptive Traffic Control**: MAS-based traffic light coordination with green wave optimization
- **Reinforcement Learning**: Q-learning implementation for signal timing optimization
- **Emergency Vehicle Priority**: Dynamic priority handling for emergency vehicles
- **Pedestrian Integration**: Agent-based pedestrian modeling with crosswalk interactions
- **Multiple Control Strategies**: Fixed-time baseline, MAS adaptive, and RL-enhanced modes
- **Comprehensive Statistics**: Travel time, queue length, and performance metrics

## Course Information

- **Course**: Intelligent Multi-Agent Systems (MRO-12)
- **University**: TH Deggendorf
- **Credits**: 5 ECTS
- **Exam Format**:
  - 50%: Group project in AnyLogic (this project)
  - 50%: Individual interview (project & lecture content)

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Main Agent                            │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Road Network (AnyLogic)                 │  │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐          │  │
│  │  │Intersect1│  │Intersect2│  │Intersect3│  ...      │  │
│  │  └──────────┘  └──────────┘  └──────────┘          │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Agent Populations                        │  │
│  │  • CarAgent[]                                        │  │
│  │  • TrafficLightAgent[]                              │  │
│  │  • PedestrianAgent[] (optional)                      │  │
│  │  • EmergencyVehicle[] (subclass of CarAgent)         │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │         Global Parameters & Statistics              │  │
│  │  • controlStrategy (0=Fixed, 1=MAS, 2=MAS+RL)      │  │
│  │  • carArrivalRate                                    │  │
│  │  • Performance metrics                               │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Agent Communication Architecture

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│  CarAgent    │────────▶│TrafficLight  │◀────────│TrafficLight  │
│              │         │    Agent      │         │    Agent      │
│  • Position  │         │  (Intersect1)│         │  (Intersect2)│
│  • Speed     │         │              │         │              │
│  • Route     │         │  • Phase     │         │  • Phase     │
│  • Emergency │         │  • Queues    │         │  • Queues    │
└──────────────┘         │  • RL State  │         │  • RL State  │
                         └──────────────┘         └──────────────┘
                                │                         │
                                └─────────┬───────────────┘
                                          │
                                   Green Wave
                                   Coordination
```

## Components

### 1. Main Agent

The root agent containing the entire simulation environment.

**Responsibilities:**
- Road network configuration
- Agent population management
- Global parameter control
- Statistics collection and reporting
- Experiment configuration

**Key Elements:**
- RoadNetwork (AnyLogic Road Traffic Library)
- Multiple intersections (3-6 intersections)
- Entry points (car sources)
- Exit points (car sinks)
- Global parameters and statistics

### 2. CarAgent

Autonomous vehicle agent with intelligent decision-making.

**Attributes:**
- `origin`, `destination`: Route endpoints
- `route`: Sequence of nodes/road connections
- `desiredSpeed`, `actualSpeed`: Speed parameters
- `totalTravelTime`: Performance metric
- `isEmergency`: Emergency vehicle flag
- `aggressiveness`: Behavioral parameter (optional)

**Statechart States:**
- `Driving`: Normal movement on road
- `ApproachingIntersection`: Near intersection, checking signals
- `WaitingAtRed`: Stopped at red light
- `CrossingIntersection`: Passing through intersection
- `Arrived`: Reached destination

**Behaviors:**
- Autonomous navigation
- Signal response (stop at red, go at green)
- Queue length reporting to traffic lights
- Emergency priority communication
- Route reconsideration based on congestion

**MAS Properties:**
- **Autonomy**: Independent movement decisions
- **Reactivity**: Responds to traffic signals and congestion
- **Proactiveness**: Can re-route based on conditions
- **Social Ability**: Communicates with traffic lights

### 3. TrafficLightAgent

Intelligent traffic signal controller with adaptive behavior.

**Attributes:**
- `currentPhase`: NS_Green, EW_Green, AllRed, Amber
- `minGreenTime`, `maxGreenTime`: Timing constraints
- `queueLengths`: Per-direction queue measurements
- `congestionLevel`: Aggregated congestion metric
- `neighbors`: References to adjacent TrafficLightAgents
- `qTable`: Q-learning state-action table (RL mode)

**Statechart States:**
- `NS_Green`: North-South direction green
- `EW_Green`: East-West direction green
- `AllRed`: Safety buffer state
- `Amber`: Transition warning state

**Control Modes:**

1. **Fixed-Time (Baseline)**
   - Predefined cycle times
   - No adaptation
   - Baseline for comparison

2. **MAS Adaptive**
   - Queue-based phase selection
   - Neighbor coordination for green waves
   - Emergency vehicle priority handling
   - Reactive decision-making

3. **MAS + RL**
   - Q-learning for timing optimization
   - State: queue difference, last phase
   - Actions: extend current phase, switch phase
   - Reward: negative total waiting time

**Communication:**
- Receives: `queueUpdate` from cars, `congestionBroadcast` from neighbors, `emergencyApproach` from emergency vehicles
- Sends: `stateBroadcast` to neighbors, `phaseChange` notifications

**MAS Properties:**
- **Autonomy**: Independent phase decisions
- **Reactivity**: Responds to queue changes and emergencies
- **Proactiveness**: Anticipates congestion patterns
- **Social Ability**: Coordinates with neighboring lights
- **Learning**: RL-based optimization (mode 2)

### 4. PedestrianAgent (Optional)

Human agent modeling pedestrian behavior at crosswalks.

**Attributes:**
- `origin`, `destination`: Pedestrian route
- `waitingTime`: Time spent waiting to cross
- `crossingSpeed`: Walking speed

**Behaviors:**
- Appears at crosswalks
- Waits for safe crossing opportunity
- Interacts with traffic lights (button press simulation)
- Conflict resolution with vehicle flows

**Integration:**
- Communicates with TrafficLightAgent for crossing permission
- Affects traffic light phase timing

### 5. EmergencyVehicle

Specialized CarAgent with priority handling.

**Attributes:**
- `isEmergency`: true (inherited from CarAgent)
- `emergencyType`: Ambulance, Fire, Police
- `priorityLevel`: Urgency rating

**Behaviors:**
- Sends `emergencyApproach` messages to upcoming intersections
- Traffic lights create temporary green wave
- Other vehicles yield (simulated)
- Priority routing through network

**Message Protocol:**
```
Message {
    type: "emergencyApproach"
    payload: {
        lane: direction,
        ETA: estimated_arrival_time,
        priority: priority_level
    }
}
```

## Development Phases

### Phase 0: Project Setup and Skeleton
- AnyLogic model creation
- Library dependencies configuration
- Global parameters and statistics setup
- Project structure initialization

### Phase 1: Static Road Network
- Road network design using Road Traffic Library
- Intersection creation (3-6 intersections)
- Entry/exit point configuration
- Basic network validation

### Phase 2: CarAgent Implementation
- CarAgent agent type creation
- Basic movement logic
- Statechart design (Driving, ApproachingIntersection, etc.)
- Integration with road network

### Phase 3: TrafficLightAgent - Fixed-Time
- TrafficLightAgent creation
- Fixed-time signal control logic
- Statechart for phase management
- Visual traffic light representation

### Phase 4: Car-Light Interaction
- Car response to traffic signals
- Stop/go logic at intersections
- Queue formation and measurement
- Basic traffic flow validation

### Phase 5: MAS Communication
- Message passing infrastructure
- Car → Light: queue updates
- Light → Light: congestion broadcasts
- Communication protocol implementation

### Phase 6: Adaptive MAS Control
- Queue-based phase selection
- Neighbor coordination logic
- Green wave implementation
- Performance comparison with fixed-time

### Phase 7: Pedestrian Integration
- PedestrianAgent creation
- Crosswalk modeling
- Pedestrian-light interaction
- Safety and fairness considerations

### Phase 8: Emergency Vehicle Priority
- EmergencyVehicle specialization
- Priority message protocol
- Traffic light emergency handling
- Green wave for emergency vehicles

### Phase 9: Reinforcement Learning
- Q-learning implementation
- State space definition
- Action space definition
- Reward function design
- Training and policy execution

### Phase 10: Experiments and Validation
- Scenario definitions (baseline, MAS, MAS+RL)
- Statistical analysis setup
- Performance metrics collection
- Comparison experiments

### Phase 11: Polish and Documentation
- Parameter tuning interface
- GUI improvements
- Code documentation
- Model validation

### Phase 12: Exam Preparation
- Oral exam explanation pack
- Key concepts mapping to lectures
- Demonstration scenarios
- Performance analysis summary

## Installation & Setup

### Prerequisites

- AnyLogic 8.x Personal Learning Edition (free)
  - Download from: https://www.anylogic.com/downloads/
- Java Development Kit (JDK) - included with AnyLogic
- Minimum 4GB RAM recommended
- Windows, macOS, or Linux

### Installation Steps

1. **Download AnyLogic PLE**
   ```
   Visit: https://www.anylogic.com/downloads/
   Select: Personal Learning Edition
   Download and install for your operating system
   ```

2. **Clone or Download Project**
   ```bash
   git clone <repository-url>
   cd Smart_City_ANYLOGIC
   ```

3. **Open Project in AnyLogic**
   - Launch AnyLogic
   - File → Open → Navigate to project folder
   - Select `SmartTrafficMAS.alp`

4. **Verify Dependencies**
   - In Projects view, check model properties
   - Ensure Road Traffic Library is enabled
   - Enable Pedestrian Library if needed

5. **Build and Run**
   - Click Build button (hammer icon)
   - Check Problems view for errors
   - Click Run to start simulation

## Usage

### Running the Simulation

1. **Select Control Strategy**
   - In Main agent properties, set `controlStrategy`:
     - `0`: Fixed-time baseline
     - `1`: MAS adaptive control
     - `2`: MAS + RL control

2. **Configure Parameters**
   - `carArrivalRate`: Vehicles per minute
   - `enableEmergencyVehicles`: true/false
   - `enablePedestrians`: true/false

3. **Run Simulation**
   - Click Run button
   - Observe agent behaviors in 2D/3D view
   - Monitor statistics in real-time

4. **View Results**
   - Check statistics variables
   - Review performance charts
   - Export data for analysis

### Running Experiments

1. **Create Experiment**
   - Right-click model → New → Experiment
   - Select experiment type (Simulation, Parameter Variation, etc.)

2. **Configure Scenarios**
   - Scenario 1: Baseline fixed-time
   - Scenario 2: MAS adaptive
   - Scenario 3: MAS + RL
   - Scenario 4: Emergency vehicles
   - Scenario 5: High pedestrian demand

3. **Compare Results**
   - Use Compare Runs experiment
   - Analyze performance metrics
   - Generate reports

## MAS Concepts Demonstrated

### 1. Autonomy
- Agents operate independently without central control
- Each agent makes decisions based on local information
- No global coordinator required

### 2. Reactivity
- Agents respond to environmental changes
- Cars react to traffic signals
- Lights respond to queue changes
- Emergency handling triggers immediate responses

### 3. Proactiveness
- Agents anticipate future states
- Route reconsideration based on congestion
- Green wave coordination
- RL-based predictive optimization

### 4. Social Ability
- Message passing between agents
- Car-Light communication
- Light-Light coordination
- Emergency priority broadcasting

### 5. Decision Making (BDI-style)
- Belief: Current traffic state (queues, signals)
- Desire: Optimize traffic flow
- Intention: Phase selection, route choice
- Commitment: Execute chosen action

### 6. Learning (RL Mode)
- Q-learning for signal timing
- State-action-reward framework
- Policy improvement over time
- Distributed learning across agents

### Agent Architectures

- **Reactive**: Traffic lights responding to immediate queue changes
- **Deliberative**: Route planning in cars, phase selection in lights
- **Hybrid**: Combination of reactive responses and planned actions

## Project Structure

For a detailed project structure, see [PROJECT_STRUCTURE.md](PROJECT_STRUCTURE.md).

### Quick Overview

```
Smart_City_ANYLOGIC/
│
├── README.md                          # Main project documentation
├── ARCHITECTURE.md                    # System architecture diagrams
├── PROJECT_STRUCTURE.md               # Detailed structure documentation
├── .gitignore                         # Git ignore rules
├── instructions                       # Project requirements
├── Phase0_Setup_Instructions.md      # Phase 0 guide
│
├── SmartTrafficMAS/                   # AnyLogic project folder
│   └── SmartTrafficMAS.alp            # Main model file
│
└── Documentation/                     # Additional documentation
    ├── AnyLogic_8_in_Three_Days.md    # AnyLogic reference
    ├── MAS_Concepts/                  # MAS explanations
    └── Exam_Preparation/              # Exam materials
```

See [PROJECT_STRUCTURE.md](PROJECT_STRUCTURE.md) for complete structure details.

## Experiments & Scenarios

### Scenario 1: Baseline Fixed-Time Control
- **Purpose**: Establish baseline performance
- **Configuration**: controlStrategy = 0
- **Metrics**: Average travel time, queue lengths, stops per vehicle

### Scenario 2: MAS Adaptive Control
- **Purpose**: Demonstrate MAS coordination benefits
- **Configuration**: controlStrategy = 1
- **Features**: Queue-based adaptation, green wave coordination
- **Metrics**: Comparison with baseline

### Scenario 3: MAS + RL Control
- **Purpose**: Show learning-based optimization
- **Configuration**: controlStrategy = 2
- **Features**: Q-learning, policy improvement
- **Metrics**: Learning curve, final performance

### Scenario 4: Emergency Vehicle Priority
- **Purpose**: Test priority handling
- **Configuration**: enableEmergencyVehicles = true
- **Features**: Dynamic green waves, priority routing
- **Metrics**: Emergency vehicle travel time, impact on regular traffic

### Scenario 5: High Pedestrian Demand
- **Purpose**: Test pedestrian integration
- **Configuration**: enablePedestrians = true, high arrival rate
- **Features**: Crosswalk interactions, pedestrian phases
- **Metrics**: Pedestrian waiting time, vehicle delay

## Performance Metrics

### Primary Metrics
- **Average Travel Time**: Mean time from origin to destination
- **Queue Length**: Maximum and average queue lengths per intersection
- **Stops per Vehicle**: Number of stops per vehicle journey
- **Throughput**: Vehicles processed per unit time

### Secondary Metrics
- **Signal Efficiency**: Green time utilization
- **Emergency Response Time**: Emergency vehicle travel time
- **Pedestrian Waiting Time**: Average wait at crosswalks
- **Fuel Consumption** (estimated): Based on stops and idling

## Team

- **Course**: Intelligent Multi-Agent Systems (MRO-12)
- **University**: TH Deggendorf
- **Group Size**: 4 students
- **Project Duration**: Course semester
- **Credits**: 5 ECTS

## Future Enhancements

Potential extensions for advanced development:

- Multi-objective optimization (travel time vs. emissions)
- V2X communication simulation
- Weather condition impacts
- Accident simulation and response
- Public transportation integration
- Dynamic route guidance systems
- Machine learning for demand prediction
- Real-time data integration

## License

This project is developed for academic purposes as part of the Intelligent Multi-Agent Systems course at TH Deggendorf.

## Documentation

This project includes comprehensive documentation:

- **[README.md](README.md)**: Main project overview and guide (this file)
- **[ARCHITECTURE.md](ARCHITECTURE.md)**: Detailed system architecture, diagrams, and design patterns
- **[PROJECT_STRUCTURE.md](PROJECT_STRUCTURE.md)**: Complete file and folder structure
- **[Phase0_Setup_Instructions.md](Phase0_Setup_Instructions.md)**: Phase 0 setup guide
- **[instructions](instructions)**: Original project requirements

### Additional Resources

- AnyLogic 8 Documentation: https://anylogic.help/
- AnyLogic Road Traffic Library Documentation
- Multi-Agent Systems: Modern Approach (Wooldridge)
- Reinforcement Learning: An Introduction (Sutton & Barto)
- Course Lecture Materials (MRO-12, TH Deggendorf)

## Contact

For questions about this project, please contact the development team or course instructor.

---

**Last Updated**: 2024  
**Version**: 1.0  
**Status**: In Development

