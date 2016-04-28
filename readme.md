#PLEXOS-Vis Readme

This repository is intended as a submodule to a PLEXOS project specific repository. 

This package creates figures and an HTML file with all of those figures for plexos solutions.
* You must use process_folder() from the rplexos package to create a database file from the PLEXOS solution .zip, before running this code.

##To Run:
1. Copy run_html_output.R and input_data.csv into another folder associated with your project specific repository.
2. Edit input_data.csv to point to the required databases, input files, and generator categories for your project.
3. If using a CSV to map generator name to type, create this.
4. If creating individual region and zone dispatch stacks, create CSV to map generator name to region and zone. Without this file, individual region and zone dispatch stacks will not work.
5. Edit run_html_output.R to point to your input_data.csv
6. Run: ```source('run_html_output.R')```

### Notes:
1. If you have trouble with the render function, make sure the "rmarkdown" package is installed. If there are issues locating or sourcing files, make sure the appropriate working directories and paths are setup correctly.
2. You can choose to run individual chunks or the entire script. This is setup in the input_data.csv file. There should no longer be inter-dependencies between chunks. In an earlier version certain chunks depended on others being run.
3. The name of your database file must start with "Model".
4. Report any problems or issues to Matt O'Connell. 

##### input_data.csv columns:
1. Database.Location
	+ Location of the .db file created from PLEXOS .zip solution file.
2. Using.Gen.Type.Mapping.CSV
	+ If usings a CSV file to assign generation type by generator name, this should be TRUE, else it should be FALSE. Column names should be "name" and "Type"
	+ If you don't do it this way, it will assign generation type by PLEXOS category.
3. Gen.Region.Zone.Mapping.Filename
	+ CSV file which assigns a Region and Zone to each generator. 
	+ Column names for generator name, region, and zone must be "name", "Region", "Zone". This can also be the same file you use to assign generation type.
	+ Only needed for region and zone dispatch stacks.
	+ If this field is blank there will be an error, so it must read in any CSV file, even if not being used.
4. CSV.Gen.Type.File.Location
	+ Location of CSV file to assign generation type by generator name, if using this (Using.Gen.Type.Mapping.CSV == TRUE).
	+ Column names for generator name and type must be "name", and "Type". This can also be the same file you use to assign region and zone.
	+ If "Using.Gen.Type.Mapping.CSV" is FALSE, this field can be left blank.
5. PLEXOS.Gen.Category
	+ Categories in PLEXOS database. This must contain all the categories in your PLEXOS database.
	+ Only required if "Using.Gen.Type.Mapping.CSV" is FALSE.
6. PLEXOS.Desired.Type	
	+ Assigns a generation type to each of the PLEXOS categories.
	+ Only requires if "Using.Gen.Type.Mapping.CSV" is FALSE.
7. Gen.Type
	+ All types of generation that you either assign through the CSV or PLEXOS category. It can contain more types than you currently have, but it must have all of them otherwise they will be missing from the results.
8. Plot.Color
	+ Assigns a plot color to each generation type
9. Gen.Order
	+ Assigns an order from top to bottom of generation types for dispatch stacks. This list must also contain every generation type in that you assign either in 5-6 or 7-8.
10. Renewable.Types.for.Curtailment
	+ Generation types to be considered in curtailment calculations.
11. Key.Periods
	+ You can choose individual time spans to plot results for. This column assigns names to those time periods.
12. Start.Time
	+ Corresponding start time for each of the periods in 13.
13. End.Time
	+ Corresponding end time for each of the periods in 13. 
14. Start.Day
	+ Starting day of the PLEXOS run. 
15. End.Day
	+ Ending day of the PLEXOS run.
16. Intervals.Per.Day
	+ Number of intervals per day.
17. Sections.to.Run
	+ Sections (chunks) of the HTML_output.Rmd file to run.
18. Fig.Path
	+ Location to save each figure.
19. Cache.Path
	+ Location to save caches if any chunks have cahce=TRUE. This is not required but if you are running the script several times for the same solution file, certain chunks take a long time to run and this can be beneficial. Described below.
20. Ignore.Zones
 	+ Zones to exclude from plots
21. Ignore.Regions
	+ Regions to exclude from plots
22. Interfaces.for.Flows
	+ Specific interfaces to show flow data for

##### About cache=TRUE

Setting cache=TRUE makes the markdown script save the data before it outputs it into an HTML for an individual chunk. The "key-period-dispatch" and "yearly-curtailment" chunks take a long time to process due to the interval queries (instead of yearly queries) required. With cache=TRUE, the first time still takes the full length to run, but subsequent runs of the script just load the saved cache data. However, if anything in the file in which cache=TRUE changes (maybe just chunk, but not sure), it will overwrite the cache and take the full length of time to process. So beware of that as well.
	
For "key-period-dispatch", to set cache=TRUE, change ```{r key-period-dispatch}``` to ```{r key-period-dispatch, cache=TRUE}```

For "yearly-curtailment", you must open up the source_scripts/p_yearly_curtailment.Rmd file, and change 
```{r yearly_curtailment, include=TRUE}``` to ```{r yearly_curtailment, include=TRUE, cache=TRUE}```

## Required PLEXOS outputs

PLEXOS must report the following data in order for the scripts to work.

### Annual
##### Generator:
 + Generation
 + Available Energy
 + Emission Cost
 + Fuel Cost
 + Start and Shutdown Cost
 + VO&M Cost
 + Installed Capacity

##### Region:
 + Load
 + Imports
 + Exports
 + Unserved Energy

##### Zone:
 + Load
 + Imports
 + Exports
 + Unserved Energy

##### Reserves:
 + Provision
 + Shortage

##### Interface
 + Flow

### Interval
##### Generator:
 + Generation
 + Available Capacity

##### Region:
 + Load

##### Zone:
 + Load
 
##### Reserve:
 + Provision

##### Interface
 + Flow
