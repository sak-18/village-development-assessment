# village-development-assessment

The aim of the project is to analyze various village level indicators from census, LULC data sources and get some insights on the distribution and diversity of villages. Clustering and visualization algorithms can be run once the data at a village level is aggregated.

## Collecting Data
At a village level the following data is to be collected
 - [x] Data from Census
 - [x] ADI data: Water, Bathroom facilities
 - [ ] % of fallow, barren, builtup land

### Data Collection Census/ADI
* Download PostGres Client/PGAdmin to connect to the server and run queries
* We do have data related to Census and ADI on PostGres Databases. Connect to the server (10.237.27.8) on which the database is hosted and obtain username, password
* Some tables are:
    1. `adi` - District level indices like ADI, water, assets etc.
    2. `census_data` - Contains some important handpicked columns from the actual census data like literacy, working population etc. at a village level
    3. `census_location` - mapping b/w village names and census codes
    
Sample query used to collect data a village level:
```
SELECT district_census_code.district, district_census_code.state, census_locations.location_name, dist_code_2011, vill_code_2011, adi_2011, msw_2011, asset_2001, fc_2011, bf_2011
FROM adi_data
INNER JOIN district_census_code
    ON (adi_data.dist_code_2011 LIKE textcat(textcat('%', district_census_code.census2011_code),'%'))
INNER JOIN census_locations
	ON CAST(CAST(adi_data.vill_code_2011 as FLOAT) as INT) = census_locations.town_village_code
WHERE adi_data.dist_code_2011 LIKE '%238%' OR adi_data.dist_code_2011 LIKE '%364%';

INNER JOIN census_data
    ON CAST(CAST(adi_data.vill_code_2011 as FLOAT) as INT) = census_data.
```