# System Architecture Documentation

This document provides detailed architecture diagrams and explanations for the Smart Multi-Agent Traffic Management System.

## Table of Contents

- [System Overview](#system-overview)
- [Agent Architecture](#agent-architecture)
- [Communication Patterns](#communication-patterns)
- [State Machine Diagrams](#state-machine-diagrams)
- [Data Flow](#data-flow)
- [Control Flow](#control-flow)

## System Overview

### Complete System Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           SIMULATION ENVIRONMENT                         │
│                                                                           │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │                         MAIN AGENT                                │  │
│  │                                                                   │  │
│  │  ┌────────────────────────────────────────────────────────────┐ │  │
│  │  │              ROAD NETWORK (AnyLogic Library)               │ │  │
│  │  │                                                             │ │  │
│  │  │    ┌────────────┐    ┌────────────┐    ┌────────────┐   │ │  │
│  │  │    │Intersection│    │Intersection│    │Intersection│   │ │  │
│  │  │    │     1      │    │     2      │    │     3      │   │ │  │
│  │  │    │            │    │            │    │            │   │ │  │
│  │  │    │  [Light1] │    │  [Light2] │    │  [Light3] │   │ │  │
│  │  │    └────────────┘    └────────────┘    └────────────┘   │ │  │
│  │  │         │                  │                  │           │ │  │
│  │  │         └──────────────────┼──────────────────┘           │ │  │
│  │  │                            │                              │ │  │
│  │  │                    Road Connections                        │ │  │
│  │  └────────────────────────────────────────────────────────────┘ │  │
│  │                                                                   │  │
│  │  ┌────────────────────────────────────────────────────────────┐ │  │
│  │  │                    AGENT POPULATIONS                       │ │  │
│  │  │                                                             │ │  │
│  │  │  CarAgent[]: [Car1, Car2, Car3, ..., CarN]                │ │  │
│  │  │  TrafficLightAgent[]: [Light1, Light2, Light3, ...]      │ │  │
│  │  │  PedestrianAgent[]: [Ped1, Ped2, ...] (optional)            │ │  │
│  │  │  EmergencyVehicle[]: [Emerg1, ...] (subclass of CarAgent)  │ │  │
│  │  └────────────────────────────────────────────────────────────┘ │  │
│  │                                                                   │  │
│  │  ┌────────────────────────────────────────────────────────────┐ │  │
│  │  │              GLOBAL CONTROL & STATISTICS                  │ │  │
│  │  │                                                             │ │  │
│  │  │  Parameters:                                               │ │  │
│  │  │    - controlStrategy (0/1/2)                              │ │  │
│  │  │    - carArrivalRate                                        │ │  │
│  │  │    - enableEmergencyVehicles                               │ │  │
│  │  │    - enablePedestrians                                     │ │  │
│  │  │                                                             │ │  │
│  │  │  Statistics:                                               │ │  │
│  │  │    - totalTravelTime                                       │ │  │
│  │  │    - totalCarsProcessed                                    │ │  │
│  │  │    - averageTravelTime()                                   │ │  │
│  │  └────────────────────────────────────────────────────────────┘ │  │
│  └──────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

## Agent Architecture

### CarAgent Internal Structure

```
┌─────────────────────────────────────────────────────────┐
│                    CarAgent                              │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │              ATTRIBUTES                          │  │
│  │  • origin: Node                                  │  │
│  │  • destination: Node                              │  │
│  │  • route: List<Node>                             │  │
│  │  • desiredSpeed: double                          │  │
│  │  • actualSpeed: double                           │  │
│  │  • totalTravelTime: double                       │  │
│  │  • isEmergency: boolean                          │  │
│  │  • currentPosition: Point                        │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │              STATECHART                          │  │
│  │                                                   │  │
│  │    [Entry]                                       │  │
│  │       │                                           │  │
│  │       ▼                                           │  │
│  │  ┌─────────┐                                      │  │
│  │  │ Driving│                                      │  │
│  │  └────┬────┘                                      │  │
│  │       │                                           │  │
│  │       ├─── approaching intersection ──┐          │  │
│  │       │                                │          │  │
│  │       ▼                                ▼          │  │
│  │  ┌──────────────────┐          ┌──────────────┐ │  │
│  │  │Approaching       │          │WaitingAtRed  │ │  │
│  │  │Intersection      │          └──────┬───────┘ │  │
│  │  └────────┬─────────┘                 │          │  │
│  │           │                           │          │  │
│  │           ├─── signal green ─────────┘          │  │
│  │           │                                       │  │
│  │           ▼                                       │  │
│  │  ┌──────────────────┐                             │  │
│  │  │Crossing          │                             │  │
│  │  │Intersection      │                             │  │
│  │  └────────┬─────────┘                             │  │
│  │           │                                       │  │
│  │           ├─── crossed ──┐                        │  │
│  │           │              │                        │  │
│  │           ▼              ▼                        │  │
│  │  ┌─────────┐      ┌──────────┐                   │  │
│  │  │ Driving │      │ Arrived  │                   │  │
│  │  └─────────┘      └──────────┘                   │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │              FUNCTIONS                           │  │
│  │  • updatePosition()                             │  │
│  │  • checkTrafficSignal()                         │  │
│  │  • sendQueueUpdate()                            │  │
│  │  • reconsiderRoute()                            │  │
│  │  • sendEmergencyMessage()                        │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### TrafficLightAgent Internal Structure

```
┌─────────────────────────────────────────────────────────┐
│                TrafficLightAgent                         │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │              ATTRIBUTES                          │  │
│  │  • currentPhase: Phase (NS_Green/EW_Green/etc)   │  │
│  │  • minGreenTime: double                          │  │
│  │  • maxGreenTime: double                          │  │
│  │  • queueLengths: Map<Direction, Integer>         │  │
│  │  • congestionLevel: double                      │  │
│  │  • neighbors: List<TrafficLightAgent>           │  │
│  │  • qTable: Map<State, Map<Action, Double>> (RL)  │  │
│  │  • intersectionID: int                           │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │              STATECHART                          │  │
│  │                                                   │  │
│  │    [Entry]                                       │  │
│  │       │                                           │  │
│  │       ▼                                           │  │
│  │  ┌──────────┐                                    │  │
│  │  │ NS_Green │                                    │  │
│  │  └────┬─────┘                                    │  │
│  │       │                                           │  │
│  │       ├─── timeout ──┐                           │  │
│  │       │              │                           │  │
│  │       ▼              ▼                           │  │
│  │  ┌──────────┐  ┌──────────┐                    │  │
│  │  │  Amber   │  │ EW_Green │                    │  │
│  │  └────┬─────┘  └────┬─────┘                    │  │
│  │       │              │                           │  │
│  │       └─── timeout ──┘                           │  │
│  │              │                                   │  │
│  │              ▼                                   │  │
│  │  ┌──────────┐                                    │  │
│  │  │ AllRed   │                                    │  │
│  │  └──────────┘                                    │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │              FUNCTIONS                           │  │
│  │  • measureQueues()                              │  │
│  │  • selectNextPhase()                            │  │
│  │  • broadcastState()                              │  │
│  │  • handleEmergency()                            │  │
│  │  • updateQTable() (RL mode)                      │  │
│  │  • getReward() (RL mode)                         │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

## Communication Patterns

### Message Flow Diagram

```
┌─────────────┐
│  CarAgent   │
│             │
│  Position: │
│  Near Light1│
└──────┬──────┘
       │
       │ 1. queueUpdate(queueLength, direction)
       │
       ▼
┌─────────────────────┐
│ TrafficLightAgent 1 │
│                     │
│  Receives:          │
│  - queueUpdate      │
│  - emergencyApproach│
│                     │
│  Sends:             │
│  - stateBroadcast   │
└──────┬──────────────┘
       │
       │ 2. stateBroadcast(phase, congestion)
       │
       ▼
┌─────────────────────┐
│ TrafficLightAgent 2 │
│  (Neighbor)         │
│                     │
│  Receives:          │
│  - stateBroadcast   │
│                     │
│  Coordinates:       │
│  - Green wave       │
│  - Phase timing     │
└─────────────────────┘
```

### Emergency Vehicle Communication Flow

```
┌──────────────────┐
│ EmergencyVehicle │
│                  │
│  isEmergency=true│
└────────┬─────────┘
         │
         │ emergencyApproach(lane, ETA, priority)
         │
         ▼
┌─────────────────────┐         ┌─────────────────────┐
│ TrafficLightAgent 1 │────────▶│ TrafficLightAgent 2│
│  (Upcoming)         │         │  (Next)             │
│                     │         │                     │
│  Actions:           │         │  Actions:           │
│  1. Check safety    │         │  1. Prepare green    │
│  2. Extend green    │         │  2. Coordinate wave │
│  3. Notify neighbor │         │  3. Notify next     │
└─────────────────────┘         └─────────────────────┘
         │
         │
         ▼
┌─────────────────────┐
│ TrafficLightAgent 3 │
│  (Further ahead)   │
│                     │
│  Prepares:          │
│  - Green wave       │
│  - Priority phase   │
└─────────────────────┘
```

## State Machine Diagrams

### CarAgent State Machine (Detailed)

```
                    [Entry Point]
                         │
                         ▼
                    ┌─────────┐
                    │ Driving │
                    └────┬────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
    approaching    destination    congestion
    intersection      reached      detected
         │               │               │
         ▼               ▼               ▼
┌──────────────────┐ ┌──────────┐ ┌──────────────┐
│Approaching       │ │ Arrived  │ │Reconsidering │
│Intersection      │ └──────────┘ │Route         │
└────┬─────────────┘              └──────┬───────┘
     │                                   │
     │                                   │
     ├─── signal red ──┐                │
     │                  │                │
     ▼                  ▼                │
┌──────────────┐  ┌──────────────┐      │
│WaitingAtRed  │  │Crossing      │      │
│              │  │Intersection  │      │
└──────┬───────┘  └──────┬───────┘      │
       │                 │               │
       │                 │               │
       └─── signal green ┘               │
              │                         │
              │                         │
              └───────────┬─────────────┘
                          │
                          ▼
                    ┌─────────┐
                    │ Driving │
                    └─────────┘
```

### TrafficLightAgent State Machine (Control Modes)

```
                    [Entry Point]
                         │
                         ▼
                    ┌──────────┐
                    │ NS_Green │
                    └────┬─────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
    Fixed-Time      MAS Adaptive    MAS+RL
    (timeout)       (queue-based)   (RL action)
         │               │               │
         ▼               ▼               ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   Amber      │  │   Amber      │  │   Amber      │
└──────┬───────┘  └──────┬───────┘  └──────┬───────┘
       │                 │                 │
       │                 │                 │
       ▼                 ▼                 ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│  EW_Green    │  │  EW_Green    │  │  EW_Green    │
└──────┬───────┘  └──────┬───────┘  └──────┬───────┘
       │                 │                 │
       │                 │                 │
       └─────────┬───────┴─────────────────┘
                 │
                 ▼
          ┌──────────┐
          │  AllRed  │
          └────┬─────┘
               │
               └─── (cycle repeats)
```

## Data Flow

### Statistics Collection Flow

```
┌─────────────┐
│  CarAgent   │
│             │
│  Records:  │
│  - startTime│
│  - endTime  │
│  - stops    │
└──────┬──────┘
       │
       │ onArrival()
       │
       ▼
┌─────────────────┐
│  Main Agent     │
│                 │
│  Updates:       │
│  - totalTravelTime│
│  - totalCarsProcessed│
│                 │
│  Calculates:    │
│  - averageTravelTime()│
│  - queueLengths │
│  - throughput   │
└──────┬──────────┘
       │
       │
       ▼
┌─────────────────┐
│  Charts/Plots   │
│                 │
│  Displays:      │
│  - Time series  │
│  - Histograms   │
│  - Comparisons  │
└─────────────────┘
```

## Control Flow

### MAS Adaptive Control Decision Flow

```
TrafficLightAgent Decision Process:
────────────────────────────────────

1. Measure Queues
   │
   ├─► Count cars in each direction
   │
2. Receive Neighbor Info
   │
   ├─► Get congestion from neighbors
   │
3. Calculate Congestion
   │
   ├─► Aggregate queue data
   │
4. Decision Point
   │
   ├─► IF emergencyApproach received
   │   └─► Handle Emergency (priority)
   │
   ├─► ELSE IF currentPhase timeout
   │   └─► Select Next Phase:
   │       ├─► Compare queue lengths
   │       ├─► Check neighbor states
   │       ├─► Consider green wave
   │       └─► Choose optimal phase
   │
5. Execute Action
   │
   ├─► Change phase
   │
6. Broadcast State
   │
   └─► Notify neighbors
```

### RL Control Flow (Q-Learning)

```
TrafficLightAgent RL Process:
──────────────────────────────

1. Observe State (S_t)
   │
   ├─► Extract: queueDiff, lastPhase
   │
2. Select Action (A_t)
   │
   ├─► Epsilon-greedy:
   │   ├─► Random (exploration)
   │   └─► argmax Q(S_t, A) (exploitation)
   │
3. Execute Action
   │
   ├─► Change phase or extend
   │
4. Observe Reward (R_t)
   │
   ├─► Calculate: -totalWaitingTime
   │
5. Observe Next State (S_{t+1})
   │
   ├─► Extract new state
   │
6. Update Q-Table
   │
   ├─► Q(S_t, A_t) ← Q(S_t, A_t) + α[R_t + γ max Q(S_{t+1}, A) - Q(S_t, A_t)]
   │
7. Repeat
```

## Component Interactions

### Complete Interaction Diagram

```
┌──────────────┐
│  Main Agent  │
│              │
│  Creates &   │
│  Manages:    │
└──────┬───────┘
       │
       ├─────────────────┬─────────────────┬─────────────────┐
       │                 │                 │                 │
       ▼                 ▼                 ▼                 ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│  CarAgent    │  │TrafficLight  │  │ Pedestrian   │  │  Emergency   │
│              │  │   Agent      │  │   Agent      │  │   Vehicle    │
└──────┬───────┘  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘
       │                 │                 │                 │
       │                 │                 │                 │
       │ queueUpdate      │                 │                 │
       ├─────────────────►│                 │                 │
       │                 │                 │                 │
       │                 │ stateBroadcast   │                 │
       │◄────────────────┤                 │                 │
       │                 │                 │                 │
       │                 │                 │ crossingRequest  │
       │                 │◄────────────────┤                 │
       │                 │                 │                 │
       │                 │                 │                 │
       │                 │                 │                 │ emergencyApproach
       │                 │◄─────────────────────────────────┤
       │                 │                 │                 │
       │                 │                 │                 │
       │                 │◄────────────────┼─────────────────┤
       │                 │  coordination   │                 │
       │                 │  (green wave)  │                 │
       │                 │                 │                 │
```

---

**Note**: These diagrams represent the conceptual architecture. Actual implementation may vary based on AnyLogic-specific features and optimizations.

