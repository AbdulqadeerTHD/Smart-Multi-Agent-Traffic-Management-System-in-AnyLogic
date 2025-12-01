# Project Structure

This document describes the complete file and folder structure of the Smart Multi-Agent Traffic Management System project.

## Repository Structure

```
Smart_City_ANYLOGIC/
│
├── README.md                          # Main project documentation
├── ARCHITECTURE.md                    # System architecture diagrams
├── PROJECT_STRUCTURE.md               # This file
├── .gitignore                         # Git ignore rules
│
├── instructions                       # Original project requirements
├── Phase0_Setup_Instructions.md      # Phase 0 detailed guide
│
├── Documentation/                     # Additional documentation
│   ├── AnyLogic_8_in_Three_Days.md   # AnyLogic reference book
│   ├── MAS_Concepts/                 # MAS concept explanations
│   │   ├── Autonomy.md
│   │   ├── Reactivity.md
│   │   ├── Proactiveness.md
│   │   ├── Social_Ability.md
│   │   └── Decision_Making.md
│   ├── Exam_Preparation/              # Oral exam materials
│   │   ├── Key_Concepts.md
│   │   ├── Lecture_Mapping.md
│   │   └── Demo_Scenarios.md
│   └── Architecture_Diagrams/         # Visual diagrams
│       ├── System_Overview.png
│       ├── Agent_Communication.png
│       └── State_Machines.png
│
├── SmartTrafficMAS/                   # AnyLogic Project Folder
│   │
│   ├── SmartTrafficMAS.alp            # Main AnyLogic model file
│   │
│   ├── Main/                          # Main Agent Type
│   │   ├── Parameters/                # Global parameters
│   │   │   ├── controlStrategy
│   │   │   ├── carArrivalRate
│   │   │   ├── enableEmergencyVehicles
│   │   │   └── enablePedestrians
│   │   ├── Variables/                 # Statistics variables
│   │   │   ├── totalTravelTime
│   │   │   └── totalCarsProcessed
│   │   ├── Functions/                 # Utility functions
│   │   │   └── getAverageTravelTime()
│   │   ├── RoadNetwork/               # Road network configuration
│   │   │   ├── Intersections/
│   │   │   ├── Roads/
│   │   │   ├── EntryPoints/
│   │   │   └── ExitPoints/
│   │   └── Presentation/             # GUI elements
│   │       ├── Charts/
│   │       ├── Controls/
│   │       └── 3D_Animation/
│   │
│   ├── CarAgent/                      # Car Agent Type
│   │   ├── Parameters/
│   │   │   ├── origin
│   │   │   ├── destination
│   │   │   ├── route
│   │   │   ├── desiredSpeed
│   │   │   ├── actualSpeed
│   │   │   ├── totalTravelTime
│   │   │   └── isEmergency
│   │   ├── Statechart/                 # Behavior statechart
│   │   │   ├── Driving
│   │   │   ├── ApproachingIntersection
│   │   │   ├── WaitingAtRed
│   │   │   ├── CrossingIntersection
│   │   │   └── Arrived
│   │   ├── Functions/
│   │   │   ├── updatePosition()
│   │   │   ├── checkTrafficSignal()
│   │   │   ├── sendQueueUpdate()
│   │   │   ├── reconsiderRoute()
│   │   │   └── sendEmergencyMessage()
│   │   └── Presentation/              # Car animation
│   │       └── 3D_Model/
│   │
│   ├── TrafficLightAgent/             # Traffic Light Agent Type
│   │   ├── Parameters/
│   │   │   ├── currentPhase
│   │   │   ├── minGreenTime
│   │   │   ├── maxGreenTime
│   │   │   ├── queueLengths
│   │   │   ├── congestionLevel
│   │   │   ├── neighbors
│   │   │   └── intersectionID
│   │   ├── Statechart/                # Phase statechart
│   │   │   ├── NS_Green
│   │   │   ├── EW_Green
│   │   │   ├── AllRed
│   │   │   └── Amber
│   │   ├── QTable/                    # RL Q-table (if RL enabled)
│   │   │   └── qTable: Map<State, Map<Action, Double>>
│   │   ├── Functions/
│   │   │   ├── measureQueues()
│   │   │   ├── selectNextPhase()
│   │   │   ├── broadcastState()
│   │   │   ├── handleEmergency()
│   │   │   ├── updateQTable()         # RL function
│   │   │   └── getReward()           # RL function
│   │   └── Presentation/              # Traffic light visualization
│   │       └── Signal_Display/
│   │
│   ├── PedestrianAgent/               # Pedestrian Agent Type (Optional)
│   │   ├── Parameters/
│   │   │   ├── origin
│   │   │   ├── destination
│   │   │   ├── waitingTime
│   │   │   └── crossingSpeed
│   │   ├── Statechart/
│   │   │   ├── Waiting
│   │   │   ├── Crossing
│   │   │   └── Arrived
│   │   ├── Functions/
│   │   │   ├── requestCrossing()
│   │   │   └── checkSafety()
│   │   └── Presentation/
│   │       └── Pedestrian_Model/
│   │
│   ├── EmergencyVehicle/              # Emergency Vehicle (extends CarAgent)
│   │   ├── Parameters/
│   │   │   ├── emergencyType
│   │   │   └── priorityLevel
│   │   ├── Functions/
│   │   │   ├── sendEmergencyApproach()
│   │   │   └── requestPriority()
│   │   └── Presentation/
│   │       └── Emergency_Vehicle_Model/
│   │
│   └── Experiments/                   # Experiment Configurations
│       ├── BaselineExperiment/        # Fixed-time baseline
│       │   ├── Parameters/
│       │   └── Output_Charts/
│       ├── MASExperiment/             # MAS adaptive
│       │   ├── Parameters/
│       │   └── Output_Charts/
│       ├── RLExperiment/               # MAS + RL
│       │   ├── Parameters/
│       │   ├── Training_Mode/
│       │   └── Output_Charts/
│       ├── EmergencyScenario/         # Emergency vehicle test
│       │   ├── Parameters/
│       │   └── Output_Charts/
│       ├── PedestrianScenario/        # High pedestrian demand
│       │   ├── Parameters/
│       │   └── Output_Charts/
│       └── CompareRuns/                # Comparison experiment
│           ├── Parameters/
│           └── Comparison_Charts/
│
├── Code/                               # Source code (if extracted)
│   ├── Java/
│   │   ├── agents/
│   │   ├── messages/
│   │   ├── utils/
│   │   └── rl/
│   └── Scripts/
│       └── analysis.py
│
├── Data/                               # Input data files
│   ├── road_network/
│   ├── traffic_patterns/
│   └── scenarios/
│
├── Results/                            # Simulation results (gitignored)
│   ├── baseline/
│   ├── mas_adaptive/
│   ├── mas_rl/
│   └── comparisons/
│
└── Tests/                              # Test scenarios
    ├── unit_tests/
    ├── integration_tests/
    └── validation_tests/
```

## AnyLogic-Specific Structure

### Model File Structure (.alp)

The `.alp` file is an AnyLogic project archive containing:

```
SmartTrafficMAS.alp
│
├── model.xml                          # Model structure definition
├── agents/                             # Agent type definitions
│   ├── Main.xml
│   ├── CarAgent.xml
│   ├── TrafficLightAgent.xml
│   ├── PedestrianAgent.xml
│   └── EmergencyVehicle.xml
├── experiments/                        # Experiment definitions
│   ├── Simulation.xml
│   ├── BaselineExperiment.xml
│   ├── MASExperiment.xml
│   └── RLExperiment.xml
├── database/                          # Built-in database
│   └── tables/
├── resources/                         # Resources (images, data)
│   ├── images/
│   ├── layouts/
│   └── data/
└── presentation/                      # Presentation elements
    ├── shapes/
    ├── charts/
    └── controls/
```

## Key Files Description

### Documentation Files

- **README.md**: Main project documentation, overview, setup instructions
- **ARCHITECTURE.md**: Detailed system architecture and diagrams
- **PROJECT_STRUCTURE.md**: This file - complete structure description
- **instructions**: Original project requirements document

### Configuration Files

- **.gitignore**: Git ignore rules for AnyLogic and IDE files
- **SmartTrafficMAS.alp**: Main AnyLogic model file (binary, gitignored by default)

### Phase Documentation

- **Phase0_Setup_Instructions.md**: Detailed Phase 0 setup guide
- Additional phase guides will be created as development progresses

## File Naming Conventions

### Agent Types
- PascalCase: `CarAgent`, `TrafficLightAgent`, `PedestrianAgent`
- Descriptive names indicating agent purpose

### Functions
- camelCase: `getAverageTravelTime()`, `updatePosition()`, `selectNextPhase()`
- Verb-noun pattern for clarity

### Parameters
- camelCase: `controlStrategy`, `carArrivalRate`, `enableEmergencyVehicles`
- Descriptive names without abbreviations where possible

### Variables
- camelCase: `totalTravelTime`, `totalCarsProcessed`, `currentPhase`
- Clear, descriptive names

### Experiments
- PascalCase: `BaselineExperiment`, `MASExperiment`, `RLExperiment`
- Descriptive of experiment purpose

## Directory Organization Principles

1. **Separation of Concerns**: Each agent type in its own directory
2. **Logical Grouping**: Related components grouped together
3. **Documentation**: Comprehensive documentation at each level
4. **Modularity**: Components can be developed and tested independently
5. **Scalability**: Structure supports future extensions

## Version Control Strategy

### Tracked Files
- Documentation (Markdown files)
- Configuration files
- Code snippets and examples
- Project structure documentation

### Ignored Files
- Binary AnyLogic files (.alp)
- Temporary files
- Build artifacts
- Large data files
- Simulation output results

### Recommended Approach
1. Keep documentation in Git
2. Share `.alp` files via cloud storage or project submission
3. Version control code snippets and configurations
4. Document model structure in markdown

## Development Workflow

1. **Phase Documentation**: Create phase-specific guides
2. **Code Snippets**: Store reusable code in documentation
3. **Architecture Updates**: Update ARCHITECTURE.md as design evolves
4. **Structure Updates**: Update this file when structure changes
5. **Version Tagging**: Tag releases at major milestones

---

**Note**: The actual AnyLogic project structure may differ slightly based on AnyLogic version and specific implementation choices. This structure represents the conceptual organization.

