# EAAM Calibration notes
The original model has been modified in several ways:
- a box has been added (box id: 22) in the southern Kerguelen plateau in recognition of circulation patterns intensifying in that area, affecting nurseries of fish groups.
- ACCESS-OM2 model now provides flow and variables for physical configuration of the model, rather than ROMS.

# Calibration steps
- Model version: 29 boxes, ACCESS-OM2 physical configuration

### 29-01-2024: Model has been tested up to 10 yrs. 
Krill diet has been dialled down from a high percentage of ZM and ZG (copepods and salps, respectively) so that they are more reliant on primary producers. However, this leads to krill being unable to build enough reserves to ensure steady recruitment of larvae.
Change details:
- pPREY1KR1 and pPREY2KR1:
![image](https://github.com/East-Antarctic-Atlantis-model/EADocumentation/assets/85492378/01cf46a8-a728-421f-b7dd-1500e4203d67)
![image](https://github.com/East-Antarctic-Atlantis-model/EADocumentation/assets/85492378/206b6f05-1d99-40e9-a383-c3cb70c89e0c)

- pPREY1KR2, pPREY2KR2:
![image](https://github.com/East-Antarctic-Atlantis-model/EADocumentation/assets/85492378/c4da7c4b-f7c0-4ab8-adc3-180edcc9698b)
![image](https://github.com/East-Antarctic-Atlantis-model/EADocumentation/assets/85492378/a4e664fc-d01d-4796-87e5-51cefe381fc1)

### 30-01-2024: Primary production, krill numbers and reserve weight
pPREY for krill predators (e.g., krill-eating seals and penguins, whales) has been increased to keep in check adult and juvenile numbers, which likely overgraze phytoplankton groups. KR age class 01 do not "explode" but remain between 1-1.5x of initial values. Adult numbers still shooting up to 10-12x, with reserve dropping as low reserve younger individuals are recruited into older classes.

### 31-01-2024: Primary production and simplified food web
Since krill is having trouble building reserves after removing ZM from their diet, I will try to avoid competition for diatom food supply (ZM main predator of PPL). I will redirect ZM towards PPS only, and get krill to be exclusive predator of PPL. Also, according to Pauli et al. 2021 "Selective feeding in Southern Ocean key grazers - diet composition of krill and salps", between 4-15% of krill diet is composed by copepods. I will add ZM back in KR diet gradually so that it doesn't become bulk of diet.
- What happened after these changes? KR still declining, ZG (salps) taking over PPL at the end of simulation.

Next steps:
- Check that C_KR and mum_KR are the same
- Reduce ZG competition
- Increase predation on KR to reduce adult numbers
- Decrease recruitment params in Beverton-Holt KR equation (EA_biol_newdiet_29_d_TEST_19_bev_kr.prm)


### 16-04-2024: Fixing primary production decline
Primary production is behaving oddly: in ReactiveAtlantis shiny app, the absolute and relative biomass of PPL and PPS (pelagic diatoms and pelagic picophytoplankton, respectively) experience a spike at the beginning, then decline. PPL keeps declining after 100 years of simulation, whilst PPS tends to stabilise around 0.5 of the original value. Multiple potential hypotheses:
1. nutrients are limited by lack of renewal/stagnant fluxes (physical environ);
2. competition between groups not balanced (ecological issues);
3. nutrient-related parameters are too “demanding” of available nutrients (ecological interaction with physical environ).

In Olive: MicroNut values seem to be relatively stable at first, cycling seasonally, but then start declining at around 1/4 of the run duration (25 years into the run). In contrast, NO3 and NH2 generally increase over time. Here, primary producer groups both appear to be struggling and do not show any initial spikes.

What I tried: I wrote a function that allows me to try a suite of different combinations of values, modified by percentage ranges. I changed values for N and MicroNut requirements for PP growth over a range from -50% to +50% over 25% intervals (separately).
- Changes to KN did not have impact on the output. 

### 17-04-2024: Primary production limitation issues
The "basic" equation for primary producer growth shows that light and nutrient limitations control PP fluxes:
$$\
G_{pp} = mum * B_{PP} * \delta_{light} * \delta_{nutrient} ...
\$$ 
ReactiveAtlantis's pp.growth() function suggests that primary producers are not in fact limited by nutrients, but by light.
$\delta$<sub>nutrient</sub> (scalar for nutrient limitation) is close to 1 for almost all boxes at surface layers, meaning that limiting nutrient is not the issue here. 
$\delta$<sub>light</sub> (scalar for light limitation) is often very low for surface layers - light is being blocked/is not enough for PP growth. 

A parameter that is often influential for competition for light and/or nutrient is mortality; in Atlantis, PP mortality is direclty controlled through either linear mortality (mL) or lysis (KLYS). The "essential" equation is as follows:
$$\[
M_{pp} = \left( \frac{KLYS \times B_{pp}}{\delta_{nutrient} + 0.1} \right) + (mL \times B_{pp})
\]$$
KLYS is inversely related to $\delta$<sub>nutrient</sub>. Therefore if $\delta$<sub>nutrient</sub> is high or close to 1 (maximum value, PP is not limited by nutrient availability), KLYS will be particularly powerful. In this instance, increasing KLYS might be too destabilising as $\delta$<sub>nutrient</sub> is generally high. Increasing mL instead might represent a more cautious approach. 

What happens if mL is changed from its original value (0.14)?
- PPL_mL 0.02: PPL biomass drops off immediately
- PPL_mL 0.15: PPL biomass spikes then declines, PPS settles at 0.5 of original values 
- PPL_mL 0.16: PPL biomass spikes then declines to 0 after 25 years. PPS mirrors its trend, spiking once PPL declines
- Higher PPL_mL values only cause boom-and-bust of PPL biomass to occur earlier.
