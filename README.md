# EastAnt-Atlantis documentation
Welcome to the EastAnt-Atlantis model documentation. Here you can find references and points of contact for the research and resources behind the development of the model.

## EastAnt structure
The EastAnt model is the result of a large collaborative effort of data collation, analysis, and interpretation. The model draws from various sources:
- physical domain: ACCESS-OM2-CICE and the COSIMA community (see Kiss et al. 2020 for model description: [https://gmd.copernicus.org/articles/13/401/2020/gmd-13-401-2020.html])
- biological information: SOKI (Southern Ocean Wiki, [https://sokiaq.atlassian.net/wiki/spaces/ABOUT/overview?mode=global]) and the community behind it, as well as models and literature published after SOKI's inception

## How do you run EastAnt Atlantis?
You will need the following files in order to run the model:
- **Run.sh**: .sh file with all input .prm files and initial conditions (.nc), as well as names for output folder and files; used to run model.
- **AntarcticGroups.csv**: file containing functional group definitions and behaviour switches (e.g., if a group migrates outside of model, performs DMV, is a primary producer/predator...).
- **EA_migrations.csv**: file containing information on migrating functional groups.
- **initial_conditions.nc**: .nc file containing initial conditions of the model, such as the characterisation of different functional groups, initial biomass, and potentialy physical forcing conditions if not provided in other ways (see EA_run.prm and extra forcing files, below).
- **EA_run.prm**: parameter file which switches on/off certain submodels, tracer tracking, checks for debugging and calibrating the model; also used to set the model run time and output frequency.
- **EA_force.prm**: parameter file which specifies which files contain forcings (e.g., salinity, temperature, precipitation, solar radiation, fishing, sea ice...).
- **EA_physics.prm**: parameter file for physical drivers (scaling parameters, temp/sal ranges for biology-related calcs, etc.).
- **EA_biol.prm**: parameter file for biological interactions and life history conditions, reproduction, feeding, distribution in the water column, primary production, etc.
- Extra **EA_forcings.nc** files: depending on EA_force.prm parameters, you will need extra .nc files to provide forcings to the model.
