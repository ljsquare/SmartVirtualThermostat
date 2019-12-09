# SmartVirtualThermostat
An smart virtual thermostat for OpenHAB

I’ve tried to “translate” the Smart Virtual Thermostat plugin from Domoticz to OpenHAB. 

#What does it do?
The rule decide based on the temperature of the room and outdoor temperature (to be made) heating duration necessary to achieve or maintain wishes temperature. To do that, once per amount of time (default 30 minutes) or when setpoint changes, etc, the rule calculates a percentage of heating. As an example (values ​​are fictive) the temperature is 21° and the room temperature to 20.3°, the rule estimate a heat time of 30%. It turns On heaters during the first 10 minutes, then Off for next 20 minutes. After a 30 minutes, it restarts the calculation to estimate the need for next hour and adjusting the constant.
The goal of this is to overcome over- or undershoots. Keeping a room a temperature is (unfortunately) not as simpel as:

if (temp<setpoint) {
  boiler.sendCommand(ON)
} else {
  boiler.sendCommand(OFF)
}
Because each room or area has their own characteristics: thermal inertia – heating power – outside Insulation, heating time required to reach the setpoint is specific to each of them.
The rule is learning to calculate right heating time and thus do not consume more than necessary. At each end of the heating phase, it examines and adjusts coefficients.
It’ll take a few days before it works as it suppose to be. It has to “learn” the room or area

#But for now
It needs testing en polishing the code, so please try it and give me feedback.
There are a few features not included (yet):

Constant calculated based on outside temperature (ConstT)
Minimal heating per cycle
Pause mode
If the items are new/first time use (=NULL) then fill them with defaults
All the items and groups that suppose to be together (temperature, setpoint, constC, etc) need to begin with the same name, explaind in DP: Associated Items 1. In version 0.2 i’ve made it possible to split a room in different devices, ex I’ve in my bathroom both a radiator and under the floor heating. They work differently, so the rule can make them indepentently
The heating mode is based on the modes of the Eurotronic Spirit Zwave thermostat 5, but any number as mode can be used.
The parameters used in de original SVT are converted to items and groups.
Last, the rule needs a database for the item history. I use RRD4J, but it can be other databases (except MAPDB)
