# Phase 0: Project Setup and Skeleton

## Step-by-Step Instructions

### Step 1: Create New Model
1. Open AnyLogic 8.x PLE
2. **File → New → Model**
3. **Model name:** `SmartTrafficMAS`
4. **Location:** Choose your folder
5. Click **Finish**

### Step 2: Configure Model Settings
1. In **Projects** view, click the top-level `SmartTrafficMAS` item
2. In **Properties** view, set:
   - **Model time units:** `minutes`
   - **Start date:** `01/01/2024 00:00:00`
   - **Stop time:** `60` (1 hour)

### Step 3: Add Libraries
1. Right-click `SmartTrafficMAS` in Projects → **Properties**
2. Go to **Dependencies** tab
3. Check:
   - ✅ **Road Traffic Library**
   - ✅ **Pedestrian Library** (optional)
4. Click **Apply**

### Step 4: Add Parameters to Main Agent
Open `Main` diagram and add these parameters from **Agent** palette:

#### Parameter 1: Control Strategy Mode
- **Name:** `controlStrategy`
- **Type:** `int`
- **Default value:** `0`
- **Comment:** `0=Fixed-time, 1=MAS Adaptive, 2=MAS+RL`

#### Parameter 2: Car Arrival Rate
- **Name:** `carArrivalRate`
- **Type:** `double`
- **Default value:** `10.0`
- **Comment:** `Cars per minute`

#### Parameter 3: Enable Emergency Vehicles
- **Name:** `enableEmergencyVehicles`
- **Type:** `boolean`
- **Default value:** `false`

#### Parameter 4: Enable Pedestrians
- **Name:** `enablePedestrians`
- **Type:** `boolean`
- **Default value:** `false`

### Step 5: Create Agent Type Placeholders
We'll create these in later phases, but for now, note the structure:

1. **CarAgent** - will be created in Phase 2
2. **TrafficLightAgent** - will be created in Phase 3
3. **PedestrianAgent** - will be created in Phase 7
4. **EmergencyVehicle** - will be created in Phase 8

### Step 6: Add Statistics Variables (in Main)
Add these variables from **Agent** palette → **Variable**:

#### Variable 1: Total Travel Time
- **Name:** `totalTravelTime`
- **Type:** `double`
- **Initial value:** `0.0`

#### Variable 2: Total Cars Processed
- **Name:** `totalCarsProcessed`
- **Type:** `int`
- **Initial value:** `0`

### Step 7: Add a Function for Statistics
Add a **Function** from **Agent** palette:

- **Name:** `getAverageTravelTime`
- **Return type:** `double`
- **Function body:**
```java
if (totalCarsProcessed > 0) {
    return totalTravelTime / totalCarsProcessed;
}
return 0.0;
```

### Step 8: Save the Model
1. **File → Save** (or Ctrl+S)
2. Save as: `SmartTrafficMAS.alp`

## Verification Checklist

After completing Phase 0, you should have:

✅ A new AnyLogic model named `SmartTrafficMAS`
✅ Model time units set to minutes
✅ Road Traffic Library added to dependencies
✅ Main agent with:
  - 4 parameters (controlStrategy, carArrivalRate, enableEmergencyVehicles, enablePedestrians)
  - 2 variables (totalTravelTime, totalCarsProcessed)
  - 1 function (getAverageTravelTime)
✅ Model saves successfully

## How to Verify It Works

1. Click the **Build** button (hammer icon) in toolbar
2. Check **Problems** view - should show no errors
3. Click **Run** button - model window should open (empty for now, that's OK)

## What's Next?

Once you confirm Phase 0 is complete, we'll move to **Phase 1: Build Static Road Network and Environment**

---

**Lecture Connection:** This setup phase establishes the simulation framework (Lecture 3-4: Event-discrete modeling foundations).


