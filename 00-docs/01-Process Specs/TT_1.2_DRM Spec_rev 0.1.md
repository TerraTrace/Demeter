# TT1.2 Design Reference Mission Definition

**Version:** 0.1  
**Date:** June 11, 2025  
**Status:** Initial Draft - Outline  
**Parent Document:** TT1.0 Process Spec v0.2

## 1. Purpose and Scope
Define the parameters and data structures needed for AI simulation of spacecraft mission scenarios.

## 2. Mission Class Definition

### 2.1 Mission Class Attribute
**Attribute Name:** `mission_class`  
**Type:** Enumerated value with predefined environmental profiles

**Allowed Values:**
- `LEO` - Low Earth Orbit (150-2000 km)
- `MEO` - Medium Earth Orbit (2000-35,786 km) 
- `GEO` - Geostationary Earth Orbit (~35,786 km)
- `HEO` - Highly Elliptical Orbit (high eccentricity)
- `CISLUNAR` - Earth-Moon system operations
- `PLANETARY` - Interplanetary/deep space missions

### 2.2 Mission Class Sub-Parameters

**LEO:**
- `altitude` (km) - or altitude range for elliptical
- `inclination` (degrees) - drives coverage and launch requirements
- `orbit_type` (circular, elliptical, sun-synchronous)

**MEO:**
- `altitude` (km) - affects radiation exposure and eclipse patterns
- `inclination` (degrees) - for coverage requirements
- `constellation_role` (standalone, constellation_member) - affects operational requirements

**GEO:**
- `longitude_slot` (degrees East) - for station-keeping and coverage
- `inclination_drift` (degrees) - affects station-keeping propellant
- `colocation` (boolean) - multiple satellites at same slot

**HEO:**
- `perigee_altitude` (km)
- `apogee_altitude` (km) 
- `inclination` (degrees)
- `argument_of_perigee` (degrees) - affects ground coverage patterns

**CISLUNAR:**
- `orbit_family` (halo, distant_retrograde, near_rectilinear, etc.)
- `lagrange_point` (L1, L2, L4, L5) - if applicable
- `lunar_proximity` (boolean) - affects navigation and communication

**PLANETARY:**
- `target_body` (Mars, Venus, Jupiter, etc.)
- `mission_phase` (cruise, orbit, surface, flyby)
- `heliocentric_distance_range` (AU) - affects solar power and thermal

### 2.3 Size Class

**Size Class Attribute:**
- `CUBESAT` (1-10 kg) - Miniaturized systems, COTS components
- `SMALLSAT` (10-500 kg) - Compact systems, moderate capability
- `MEDIUM` (500-2000 kg) - Standard satellite bus class
- `LARGE` (2000-10000 kg) - Major mission class, high capability
- `XLARGE` (>10000 kg) - Heavy lift missions, space stations

### 2.4 Human Rating Categories

**Human Rating Attribute:**
- `CREW_CARRYING` - Humans are transported inside the spacecraft
- `CREW_INTERFACING` - Spacecraft docks/berths with human-rated systems
- `NON_HUMAN_RATED` - No direct human interaction requirements

**Requirements Impact by Category:**

**CREW_CARRYING:**
- Full life support systems (ECLSS)
- Abort and emergency escape capabilities
- Human-rated reliability requirements (typically >99.9%)
- Crew interfaces, displays, and manual controls
- Pressurized crew compartment
- Radiation shielding for crew protection

**CREW_INTERFACING:**
- Compatible docking/berthing systems
- Contingency crew access capabilities
- Elevated reliability for critical systems only
- Emergency communication systems
- Safe approach and departure procedures
- No internal life support required

**NON_HUMAN_RATED:**
- Standard reliability requirements
- No crew safety systems
- Autonomous or ground-controlled operations
- Cost-optimized design approaches

### 2.5 Environmental Profiles by Class
Each mission class automatically populates default environmental parameters for AI simulation:

**LEO Default Profile:**
- Eclipse frequency: ~15 eclipses per day
- Eclipse duration: ~35 minutes maximum
- Solar flux: 1361 W/m² (when illuminated)
- Drag environment: Significant atmospheric interaction
- Radiation: Moderate, SAA passages

**GEO Default Profile:**
- Eclipse frequency: Seasonal (equinox periods only)
- Eclipse duration: Up to 72 minutes during eclipse seasons
- Solar flux: 1361 W/m² (nearly continuous)
- Drag environment: Negligible
- Radiation: High GEO radiation environment

## 3. DRM Types and Selection
- Primary DRM (baseline mission)
- Extended DRM (growth scenarios)  
- Stress DRM (worst-case conditions)
- Heritage DRM (comparison baseline)

## 4. Mission Timeline Parameters
- Mission phases and durations
- Critical events and timing
- Operational cycles and frequencies
- Contingency time allocations

## 4. Orbital and Environmental Parameters
- Orbital elements and evolution
- Eclipse periods and thermal cycling
- Radiation environment profiles
- Atmospheric density (for drag)

## 5. Spacecraft State Definitions
- Operational modes and configurations
- Power consumption profiles
- Thermal dissipation patterns
- Attitude and pointing requirements

## 6. Resource Utilization Profiles

### 6.1 Power Profile Parameters
**Power Generation:**
- `solar_array_beginning_of_life` (W) - Initial power generation capability
- `solar_array_end_of_life` (W) - Degraded power after mission duration
- `degradation_rate` (%/year) - Annual power degradation
- `eclipse_capability` (boolean) - Battery-only operation during eclipse

**Power Consumption:**
- `housekeeping_power` (W) - Baseline spacecraft systems power
- `payload_power_nominal` (W) - Normal payload operations
- `payload_power_peak` (W) - Maximum payload power draw
- `communication_power` (W) - Transmitter and receiver power
- `propulsion_power` (W) - Thruster control and valve power

**Power Storage:**
- `battery_capacity` (Wh) - Total energy storage
- `depth_of_discharge_limit` (%) - Maximum allowable discharge
- `charge_efficiency` (%) - Battery charging efficiency

### 6.2 Propellant Budget Parameters
**Mission Delta-V Requirements:**
- `orbit_insertion_dv` (m/s) - Launch vehicle separation to operational orbit
- `station_keeping_dv_annual` (m/s/year) - Orbit maintenance
- `attitude_control_dv_annual` (m/s/year) - Momentum dumping
- `disposal_dv` (m/s) - End-of-mission disposal maneuvers
- `contingency_dv` (m/s) - Reserve for off-nominal operations

**Propulsion System Parameters:**
- `propellant_type` (hydrazine, cold_gas, electric, etc.)
- `specific_impulse` (seconds) - Propulsion efficiency
- `thruster_minimum_impulse_bit` (N⋅s) - Smallest controllable impulse
- `tank_capacity` (kg) - Total propellant storage

### 6.3 Thermal Load Parameters
**Heat Generation Sources:**
- `electronics_dissipation` (W) - Heat from electronics
- `solar_absorption` (W) - Solar heating (varies with attitude)
- `earth_infrared` (W) - Earth thermal radiation (LEO missions)
- `equipment_duty_cycles` (%) - Fraction of time heat sources are active

**Heat Rejection Requirements:**
- `radiator_area` (m²) - Passive thermal radiator area
- `operating_temperature_range` (°C) - Equipment thermal limits
- `survival_temperature_range` (°C) - Non-operating thermal limits

### 6.4 Data Handling Parameters
**Data Generation:**
- `science_data_rate` (Mbps) - Payload data generation
- `housekeeping_data_rate` (kbps) - Spacecraft telemetry
- `data_storage_capacity` (GB) - Onboard storage capability
- `data_compression_ratio` - Data reduction before storage/transmission

**Communication Parameters:**
- `downlink_data_rate` (Mbps) - Ground transmission capability
- `contact_duration_per_orbit` (minutes) - Ground station access time
- `contacts_per_day` (number) - Ground communication opportunities

## 7. Interface and Communication Parameters
- Ground contact windows and schedules
- Data downlink requirements and volumes
- Command uplink frequencies and priorities
- Emergency communication procedures

## 8. Performance and Success Metrics
- Mission objective quantification
- Performance thresholds and margins
- Success criteria and measurement methods
- Failure mode impacts and recovery

## 9. Simulation Integration Requirements
- Physics model input parameters
- Subsystem interaction definitions
- Monte Carlo uncertainty parameters
- Validation and verification criteria