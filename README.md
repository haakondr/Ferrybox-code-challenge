# Ferrybox code challenge

In this task, we'd like you to work with data collected via the [NorSOOP infrastructure project](https://www.norsoop.com/). 

## Table of contents
- [NorSOOP](#norsoop)
- [Disclaimer](#disclaimer)
- [Terminology](#terminology)
- [Units](#units)
- [Example data](#example-data)
- [Code challenge](#code-challenge)
- [Delivery](#delivery)
- [Contact](#contact)


## NorSOOP
![NorSOOP](./figures/norsoop.webp)

NorSOOP (Norwegian Ships of Opportunity program) is a national research infrastructure that uses ships of opportunity, such as container ships, ferries, and cruise ships, to support oceanic and atmospheric research and observations. NorSOOP will help to find ways to detect and manage human impacts on the ocean.

Various ships are equipped with a "Ferrybox" which is a collection of sensors that read out measurements every minute. The map below shows the path of Color Fantasy which is one of the Ships of Opportunity.

![Color Fantasy track](./figures/FA-track-sm.png)

The ships typically follow a planned transect between two or more ports, as can be seen in the picture. 


## Disclaimer

The data provided in this repository is not quality controlled and should not be used in any real application. The intention of this repository is to serve as a code challenge during interviews at NIVA.

## Terminology

#### Measurement

A single reading from a sensor. A signal can typically have many
measurements all sharing the same timestamp.

#### Signal

A universally unique ID for each signal. Usages:

- Duplicate detection
- State tracking during data ingestion
- Logging

#### Pump

The Ferrybox system pumps in water to perform measurements. If the pump is off, the system keeps measuring the same water. Pump status is indicated by <platform_code>/ferrybox/SYSTEM/PUMP (1 is on, 0 is off).

#### Platform / Data source

In the context of Ferrybox, the platform is the actual vessel. Identified by platform_code. 

## Units

- All temperature measurements are done in Celsius
- Location is using WGS 84 coordinates 
- time format is using iso 8601 timestamps in UTC timezone

## Example data

The [data](./data) directory contains an example of measurements from Color Fantasy. Each file may contain several readings. Below is an example signal:

```json
 {
      "properties": {
        "datetime": "2020-03-05T13:45:24",
        "platform_code": "FA",
        "signal_id": "287e6d03-d86e-474a-ae80-14fbd49c3e69"
      },
      "measurements": {
        "FA/ferrybox/GPS/TIME": 0.5731712962962963,
        "FA/ferrybox/SYSTEM/PUMP": 1.0,
        "FA/ferrybox/SYSTEM/OBSTRUCTION": 0.0,
        "FA/ferrybox/SAMPLER/MANUAL_COUNTER": 0.0,
        "FA/ferrybox/SAMPLER/AUTOMATIC_COUNTER": 0.0,
        "FA/ferrybox/SYSTEM/TRIP_NUMBER": 12560.0,
        "FA/ferrybox/TURBIDITY": 3.13,
        "FA/ferrybox/CHLA_FLUORESCENCE/RAW": 0.05,
        "FA/ferrybox/CHLA_FLUORESCENCE/ADJUSTED": 0.015,
        "FA/ferrybox/INLET/TEMPERATURE": 3.433,
        "FA/ferrybox/CTD/TEMPERATURE": 3.969,
        "FA/ferrybox/CTD/SALINITY": 24.992,
        "FA/ferrybox/OXYGEN/CONCENTRATION": 393.425,
        "FA/ferrybox/OXYGEN/SATURATION": 96.466,
        "FA/ferrybox/OXYGEN/TEMPERATURE": 4.146,
        "FA/ferrybox/CDOM_FLUORESCENCE/RAW": 5.45,
        "FA/ferrybox/CDOM_FLUORESCENCE/ADJUSTED": 5.45,
        "FA/ferrybox/CYANO_FLUORESCENCE/RAW": 3.68,
        "FA/ferrybox/CYANO_FLUORESCENCE/ADJUSTED": 3.68,
        "FA/ferrybox/INLET/OXYGEN/SATURATION": 90.66,
        "FA/ferrybox/INLET/OXYGEN/CONCENTRATION": 355.338,
        "FA/ferrybox/INLET/OXYGEN/TEMPERATURE": 5.684,
        "FA/ferrybox/PAH_FLUORESCENCE/RAW": 5.45,
        "FA/ferrybox/PAH_FLUORESCENCE/ADJUSTED": 5.45
      },
      "measured_flags": {
        "FA/ferrybox/QC/G0/SYSTEM/DATA_FLAG_UNDERWAY": 1.0
      },
      "location": {
        "FA/gpstrack": {
          "longitude": 10.5642,
          "latitude": 59.7269
        }
      }
    }
```

Each measurement is identified by a unique string (following the pattern boat_name/sensor_group/sensor/measurement) with a corresponding value. 
Sensor_group ferrybox relates to a physical box with sensors in a dedicated room inside the boat. Seawater is pumped from a subsurface inlet into the 
ferrybox system. Sensor_group ferrybox/inlet refers to a set of sensors installed at inlet. CTD is used to measure the conductivity (closely related to salinity), temperature, and pressure (i.e. depth which is closely related to pressure) of seawater. 

## Code challenge
Sensors can sometimes read out wrong values, especially sensors sitting in water. The example data provided is not quality controlled.  Before the interview, we'd like you to prepare a solution to the tasks below.

Perform quality control on the following measurements:

- FA/ferrybox/INLET/TEMPERATURE
- FA/ferrybox/CTD/TEMPERATURE

Create a code solution that can detect and flag any anomalies that may have caused wrong measurements. You can use the example data in [data](./data) directory.

## Delivery

Code solution as a **private** github fork. The code can be written in a language of your choice. During the interview we would like you to present the code. The code challenge is mainly used as a basis for conversation. **We don't expect a perfect solution**. 

## Contact

Please don't hesitate to reach out to [hakon.rokenes@niva.no](mailto:hakon.rokenes@niva.no) if you have any questions.
