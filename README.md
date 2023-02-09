# PyPSA-Eur-Sec to IPCC/IAMC

This repository contains scripts to export IAMC format datafiles from PyPSA-Eur-Sec networks. 

Formatting follows the requirements for the IPCC AR6 database that can be found here: [IPCC AR6 documentation](https://data.ene.iiasa.ac.at/ar6-scenario-submission/#/about)

[Information on the IAMC databeses](https://software.ene.iiasa.ac.at/ixmp-server/tutorials.html)

## Exporting a dataset

Using the [pypsa_to_IPCC.py](pypsa_to_IPCC.py) script a scenario run with the PyPSA-Eur-Sec model can be exportet to a dataset in IAMC format. The script will use information from the Config.yaml to get information regarting the scenario. Other information such as 'literature_reference' needs to be updated at top of the script
~~~
model = "PyPSA-Eur-Sec 0.0.2" 
model_version = "0.0.6"
literature_reference = "Pedersen, T. T., Gøtske, E. K., Dvorak, A., Andresen, G. B., & Victoria, M. (2022). Long-term implications of reduced gas imports on the decarbonization of the European energy system. Joule, 6(7), 1566-1580."
climate_target = 21 # CO2 budget

scenarios={'Base_1.5':'postnetworks/elec_s370_37m_lv1.0__3H-T-H-B-I-solar+p3-dist1-cb25.7ex0_',
            'Gaslimit_1.5':'postnetworks/elec_s370_37m_lv1.0__3H-T-H-B-I-solar+p3-dist1-cb25.7ex0-gasconstrained_',
            'Base_2':'postnetworks/elec_s370_37m_lv1.0__3H-T-H-B-I-solar+p3-dist1-cb73.9ex0_',
            'Gaslimit_2': 'postnetworks/elec_s370_37m_lv1.0__3H-T-H-B-I-solar+p3-dist1-cb73.9ex0-gasconstrained_',
                     }
~~~

Running the script will result in a single .xlsx file per scenario (each containing all the modeled years) located in the results folder. 

## The IAMC format

The IAMC format was developed by the [Integrated Assessment Modeling Consortium (IAMC)](https://www.iamconsortium.org/)
and is used in many model comparison projects at the global and national level.
It can be used for integrated-assessment models, energy-systems scenarios
and analysis of specific sectors like transport, industry or buildings.

The format is a tabular structure with the columns *model*, *scenario*, *region*,
*variable*, *unit*, and a time domain. Each project defines "codelists"
to be used across modelling teams for comparison and analysis of results.

The most recent high-profile application of the IAMC format is the [AR6 Scenario Explorer](https://data.ece.iiasa.ac.at/ar6)
hosting the scenario ensemble supporting the quantitative assessment
in the contribution by Working Group III to the IPCC's Sixth Assessment Report (AR6).


## Writing a configuration file



## List of relevant IAMC variables for PyPSA-eur-sec

### Capacity

Installed capacity : the installed capacity of different technologies that are operational at the specified year.
Added capacity : the added capacity of different technologies at the specified year.
Cumulative capacity : total installed capacity of different technologies from the start year: operational + decomissioned.

Example on how to read variables : 
How much biomass related technologies are installed to produce electricity, gas, and hydrogen? how much with or without carbon capture? 

Level | Commodity | Fuel | Carbon capture
---|---|---|---
Capacity|Electricity|Biomass
Capacity|Electricity|Biomass|w/ CCS
Capacity|Gases|Biomass
Capacity|Gases|Biomass|w/ CCS
Capacity|Gases|Biomass|w/o CCS
Capacity|Hydrogen|Biomass
Capacity|Hydrogen|Biomass|w/ CCS


### Secondary Energy

Includes energy production from different technologies.

Example on how to read variables : 
How much electricity, liquids, gases, solids, and hydrogen is produced from coal? 

Level | Commodity | Fuel | Carbon capture
---|---|---|---
Secondary Energy|Electricity|Coal| w/ and w/o CCS
Secondary Energy|Liquids|Coal
Secondary Energy|Gases|Coal
Secondary Energy|Solids|Coal
Secondary Energy|hydrogen|Coal| w/ and w/o CCS


### Final Energy

Includes demand of different sectors (residential, industry, transport, etc.) for electricity, heat, hydrogen, etc.

Example on how to read variables : 
What is the demand for coal in different sectors? 

Level | Sector | Fuel | Fuel
---|---|---|---
Final Energy|Industry|Solids|Coal
Final Energy|Residential and Commercial|Solids|Coal
Final Energy|Transportation|Liquids|Coal
Final Energy|Other Sector|Solids|Coal


### Emissions and Carbon Intensity

Emisions is total equivalent CO2 emissions for different technologies. Carbon intensity is included for industry sectors.


Level | Commodity | Sector | Sector | Sector
---|---|---|---|---
Emissions|CO2|Energy|Supply|Electricity
Carbon Intensity|Production|Cement

### Financial variables

Constant: Capital cost (annual), O&M costs, and lifetime assumed for different technologies
Non_constant : Investment (total capacity multiplied by capital cost for the modeled year)

Example on how to read variables : 
What is the lifetime/capital cost/O&M cost/investment for technologies that use biomass to produce electricuty?

Level | Commodity | Secondary commodity | Fuel | Carbon capture
---|---|---|---|---
Lifetime|Electricity||Biomass|w/o CCS
Capital Cost|Electricity||Biomass|w/o CCS
OM Cost|Fixed|Electricity|Biomass|w/o CCS
Investment|Energy Supply|Electricity|Biomass|w/ CCS
