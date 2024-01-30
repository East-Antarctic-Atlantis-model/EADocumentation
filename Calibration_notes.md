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
What happened after these changes? KR still declining, ZG (salps) taking over PPL at the end of simulation.
