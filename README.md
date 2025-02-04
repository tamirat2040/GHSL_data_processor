"# GHSL_data_processor" 
# This script processes urban center database (UCDB) infromation from Global Human Settlement Layer GHSL
# Before running the script, please download your own UCDB data from https://human-settlement.emergency.copernicus.eu/ghs_ucdb_2024.php
# And store in you local disk, as I have done here in the code.

# INSTITUTION: Addis Ababa University, Institute of Geophysics, Space Scienc, and Astronomy - IGSSA;
# Unit of Geodesy, Geomatics, and Gravimetry.

# PURPOSE OF THE SCRIPT: This Python script takes in a downloaded Global Human Settlement Layer (for Sub-Saharan Africa)
# from a local Drive and prepares the data using various Python packages and ArcGIS tools from arcpy library.

# AUTHOR: Tamirat Fikre Nebiyu (A PhD student in AAU)
# DATE AND PLACE: January 29, 2025, Addis Ababa, Ethiopia.


# ########################################## Start of the script ###################################################
# ########################################## Start of the script ###################################################
import arcpy
from arcpy import env

# 1) Set the environment up.
# 1.1) Overwrite the results
env.overwriteOutput = "True"
#  1.2)Set working directory
env.workspace = "C:\\Tamirat_2024\\IGSSA_PhD\\Y2_S1\\PhD_Proposal\\PhD_Proposal\\Analysis.gdb"
env.overwriteOutput = True


# 2) Create variables
# 2.1) Create variables for each data set.
years = ["y1970", "y1975", "y1980", "y1985", "y1990", "y1995", "y2000", "y2005", "y2010", "y2015", "y2020", "y2025"]
climate_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
               "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_CLIMATE_GLOBE_R2024A"
emissions_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
                 "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_EMISSIONS_GLOBE_R2024A"
# exposure_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
#                 "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_EXPOSURE_GLOBE_R2024A"
geography_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
                 "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_GEOGRAPHY_GLOBE_R2024A"
ghsl_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
            "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_GHSL_GLOBE_R2024A"
greenness_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
                 "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_GREENNESS_GLOBE_R2024A"
hazard_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
              "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_HAZARD_RISK_GLOBE_R2024A"
infrastructure_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
                      "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_INFRASTRUCTURES_GLOBE_R2024A"
natural_systems_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
                       "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_NATURAL_SYSTEMS_GLOBE_R2024A"
sdg_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
           "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_SDG_GLOBE_R2024A"
socioeconomic_data = "C:/Tamirat_2024/IGSSA_PhD/Y2_S1/PhD_Proposal/PhD_Proposal/Data/" \
                     "GHS_UCDB_REGION_SUB_SAHARAN_AFRICA_R2024A.gpkg/main.GHSL_UCDB_THEME_SOCIOECONOMIC_GLOBE_R2024A"


# 2.2) Create variables for selected field names (attributes)
# 2.2.1) Create variables for GHSL
built_up = total_Built_up_surface = 'GH_BUS_TOT'
built_up_pcp = total_built_up_surface_per_capita = 'GH_BPC_TOT'
built_up_vol = total_Built_up_volume = 'GH_BUV_TOT'
total_pop = total_Population = 'GH_POP_TOT'
growth_rate = compound_Annual_Growth_Rate = 'GH_POP_CAG'
bu_surf_s11 = bu_surface_in_SMOD_11 = 'GH_XST_S11'
bu_surf_s12 = bu_surface_in_SMOD_12 = 'GH_XST_S12'
bu_surf_s13 = bu_surface_in_SMOD_13 = 'GH_XST_S13'
bu_surf_s21 = bu_surface_in_SMOD_21 = 'GH_XST_S21'
bu_surf_s22 = bu_surface_in_SMOD_22 = 'GH_XST_S22'
bu_surf_s23 = bu_surface_in_SMOD_23 = 'GH_XST_S23'
bu_surf_s30 = bu_surface_in_SMOD_30 = 'GH_XST_S30'
area_smod_a11 = area_in_SMOD_11_ = 'GH_XST_A11'
area_smod_a12 = area_in_SMOD_12 = 'GH_XST_A12'
area_smod_a13 = area_in_SMOD_13 = 'GH_XST_A13'
area_smod_a21 = area_in_SMOD_21_ = 'GH_XST_A21'
area_smod_a22 = area_in_SMOD_22 = 'GH_XST_A22'
area_smod_a23 = area_in_SMOD_23 = 'GH_XST_A23'
area_smod_a30 = area_in_SMOD_30 = 'GH_XST_A30'

# 2.2.2) Create variables for Socioeconomic
gdp = 'SC_SEC_GDP'

# 2.2.3) Create variables forEmissions
emission_co2 = total_emission_of_CO2_ = 'EM_CO2_TOT'
emission_ghg = total_emission_of_GHG = 'EM_GHG_TOT'
emission_nox = total_emission_of_NOx_ = 'EM_NOX_TOT'
emission_pm25 = total_emission_of_PM25 = 'EM_PM2_TOT'
emiss_pm25_conc = pm25_population_weighted_average_concentrations = 'EM_PM2_CON'

# 2.2.5) Create variables for Hazard and Risk
conflict_events = total_number_of_events_conflicts = 'HZ_CON_TOT'
hazard_events = total_number_of_event_hazards = 'HZ_CEV_TEV'
total_hazards = total_number_of_hazards = 'HZ_CEV_THZ'

# 2.2.6) Create variables for Greenness
greenness = mean_greenness_in_the_built_up_area = 'GR_AVG_GRN'

# 2.2.7) Create variables for SDG
land_gr_rate = land_consumption_rate__ = 'SD_LUE_LCR'
pop_gr_rate = population_growth_rate__ = 'SD_LUE_PGR'

# 2.2.8) Create variables for Climate
mean_temp = annual_mean_temperature_in_the_decade_reanalysis_data = 'CL_B01_CUR'
mean_precip = annual_precipitation_mean_in_the_decade_reanalysis_data = 'CL_B12_CUR'
max_temp_days = percentage_of_days_with_maximum_temperature = 'CL_WDS_CUR'

# 2.2.9) Create variables for Natural Systems
transgress = wet_bulb_temperature_transgression = 'NS_ESB_WET'

# 2.2.10) Create variables for Infrastructure
road_density = road_network_density = 'IN_ROA_DEN'

# 2.2.11) Create a variable for comprehensive data list.
africa_data_list = [climate_data, emissions_data, geography_data, ghsl_data, greenness_data, hazard_data,
                    infrastructure_data, natural_systems_data, sdg_data, socioeconomic_data]
# africa_data_list_2 = [greenness_data, sdg_data]

# 2.2.12) Create a variable for a comprehensive  list of attributes (variables to be used for Machine Learning later).
africa_variables = [built_up, built_up_pcp, built_up_vol, total_pop, growth_rate, bu_surf_s11, bu_surf_s12, bu_surf_s13,
             bu_surf_s21, bu_surf_s22, bu_surf_s23, bu_surf_s30, area_smod_a11, area_smod_a12, area_smod_a13,
             area_smod_a21, area_smod_a22, area_smod_a23, area_smod_a30, gdp, emission_co2, emission_ghg, emission_nox,
             emission_pm25, emiss_pm25_conc, conflict_events, hazard_events, total_hazards, greenness,
             land_gr_rate, pop_gr_rate, mean_temp, mean_precip, max_temp_days, transgress, road_density]
# africa_variables_2 = [greenness, land_gr_rate, pop_gr_rate]
print("STEP 2 COMPLETED SUCCESSFULLY!")


# 3) Create a list to hold the values.
city_list = []
# Use a SearchCursor to iterate through each row in the feature class
with arcpy.da.SearchCursor(climate_data, ["GC_UCN_MAI_2025"]) as cursor:
    for row in cursor:
        city_list.append(row[0])  # Append the value from the specified field
print("STEP 3 COMPLETED SUCCESSFULLY!")


# 4) Clean the city names and create new City names
city_names = []
for city in city_list:
    if isinstance(city, unicode):
        city_1 = city.encode('utf-8')
        if '/' in city_1 or ' ' in city_1 or "'" in city_1 or "-" in city_1:
            city_name = city_1.replace("/", "_").replace(" ", "_").replace("\\", "_").replace("'", "").replace("-", "_")
            city_names.append(city_name)
        else:
            city_names.append(city_1)
    elif isinstance(city, type(None)):
        city_5 = str(city)
        if '/' in city_5 or ' ' in city_5 or "'" in city_5:
            city_name = city_5.replace("/", "_").replace(" ", "_").replace("\\", "_").replace("'", "")
            city_names.append(city_name)
        else:
            city_name = city_5
            city_names.append(city_5)
    else:
        print(city)
print("City names fields have been cleaned and corrected.")
print("STEP 4 COMPLETED SUCCESSFULLY!")


# 5) Add the "City_name" field and calculate it.
# 5.1) Add "City_name" field.
for data_set in africa_data_list:
    arcpy.AddField_management(data_set, "City_name", "TEXT")

# 5.2)Calculate city  (The "City_name" field for each row)
for data in africa_data_list:
    with arcpy.da.UpdateCursor(data, ["GC_UCN_MAI_2025", "City_name"]) as cursor:
        for row in cursor:
            original_city = row[0]  # Value from the "GC_UCN_MAI_2025" field
            if isinstance(original_city, unicode):
                original_city_2 = original_city.encode('utf-8')
            else:
                original_city_2 = str(original_city)
            for city in city_names:
                if original_city_2[:3] == city[:3]:  # To excludnclude N'Djamena city, for example.
                    row[1] = city  # Update the "City_name" field
                    cursor.updateRow(row)
print("City_name field has been updated.")
print("STEP 5 COMPLETED SUCCESSFULLY!")


# 6) Create a list of data sets
# 6.1) Create list of data sets for big cities.
africa_big_cities_list = []
for data in africa_data_list:
    big_city = arcpy.Select_analysis(data, "C4_" + arcpy.Describe(data).name[21:], "GC_UCA_KM2_2025 > 222")
    africa_big_cities_list.append(big_city)

# 6.2) Create data set for each variable?
africa_big_cities_list_2 = []
for v in africa_variables:
    for data in africa_big_cities_list:
        field_list = arcpy.ListFields(data)
        # d += 1
        for field in field_list:
            # print(field.name)
            # print(v)
            if field.name[:10] == v:
                big_cities = arcpy.Select_analysis(data, "C4_" + v, "GC_UCA_KM2_2025 > 222")  # Select big towns
                africa_big_cities_list_2.append(big_cities)
            else:
                pass
    # print("List of towns has been extracted for " + str(arcpy.Describe(data).name) + ".")
    print("List of towns has been extracted for " + v + ".")
print("Study cities have been selected successfully!")
print("STEP 6 COMPLETED SUCCESSFULLY!")


# 7) Make unique list of variables.
africa_big_cities_list_3 = []
unique_name_list = []
for data in africa_big_cities_list_2:
    fc_name = arcpy.Describe(data).name[-10:]
    if fc_name not in unique_name_list:
        unique_name_list.append(fc_name)
        if fc_name in unique_name_list:
            africa_big_cities_list_3.append(data)
print("We have got " + str(len(unique_name_list)) + " unique elements and "
      + str(len(africa_big_cities_list_3)) + " variables.")
print("STEP 7 COMPLETED SUCCESSFULLY!")


# 8) Calculate years data (the time series data)
for v in africa_variables:
    for data in africa_big_cities_list_3:
        desc = arcpy.Describe(data)
        fc_name = desc.name
        for y in years:
            for f in arcpy.ListFields(data):
                if fc_name[-10:] == v and f.name.startswith(v) and f.name.endswith(y[-2:]):
                    arcpy.CalculateField_management(data, y, '!' + f.name + '!', "PYTHON", "")  # Calculate the fields
    print("Years data of " + v + " has been calculated for all cities.")
print("Times series data has been added successfully!")
print("STEP 8 COMPLETED SUCCESSFULLY!")


# 9) Add and calculate the 'Variable field'.
# 9.1) Add variable field
for data in africa_big_cities_list_3:
    arcpy.AddField_management(data, "Variable", "TEXT")

# 9.2) Calculate variable field.
for v in africa_variables:
    for data in africa_big_cities_list_3:
        desc = arcpy.Describe(data)
        if desc.name[-10:] == v:
            arcpy.CalculateField_management(data, "Variable", "'%s'" % desc.name[-10:], "PYTHON", "")
    print("Variable " + v + " has been inserted for all cities.")
print("Variable field has been calculated successfully!")
print("STEP 9 COMPLETED SUCCESSFULLY!")


# 10) Delete Unnecessary fields
# 10.1) First, identify those fields not to be deleted.
undeletable_fields = ["OBJECTID", "fid", "GC_UCN_MAI_2025", "geom", "geom_Length", "geom_Area", "ID_UC_G0",
                      "GC_UCA_KM2_2025", "GC_POP_TOT_2025","y1970", "y1975", "y1980", "y1985", "y1990", "y1995",
                      "y2000", "y2005", "y2010", "y2015", "y2020", "y2025", "City_name", "Variable"]

# 10.2) And secondly, delete the rest of the fields.
for data in africa_big_cities_list_3:
    deletables = []
    deletable_fields = ""
    lst = arcpy.ListFields(data)
    for field in lst:
        if str(field.name) not in undeletable_fields:
            deletables.append(field.name)  # Populates the empty list.
            deletable_fields = ";".join(deletables)  # Converts list to a semi-colon-separated string.
            arcpy.DeleteField_management(data, str(deletable_fields))
    else:
        pass
    print("Fields deleted for " + str(arcpy.Describe(data).name) + ".")
print("Field deletion completed successfully!")
print("STEP 10 COMPLETED SUCCESSFULLY!")


# 11) Merge the data together and project it to Geographic coordinate system. This is useful for mapping later.
# 11.1) Merge the data
merged_data = arcpy.Merge_management(africa_big_cities_list_3, "C4_all_merged")
print("All data have been merged together successfully!")

# # 11.1) Project it to GCS WGS 1984.
merged_data_2 = arcpy.Project_management(merged_data, "C4_all_merged_3", "GEOGCS['GCS_WGS_1984',DATUM['D_WGS_1984',"
                                                                         "SPHEROID["
                                                           "'WGS_1984',6378137.0,298.257223563]],PRIMEM["
                                                           "'Greenwich',0.0],UNIT['Degree',0.0174532925199433]]",
                         "", "PROJCS['World_Mollweide',GEOGCS['GCS_WGS_1984',DATUM['D_WGS_1984',SPHEROID['WGS_1984',"
                             "6378137.0,298.257223563]],PRIMEM['Greenwich',0.0],UNIT['Degree',0.0174532925199433]],"
                             "PROJECTION['Mollweide'],PARAMETER['False_Easting',0.0],PARAMETER['False_Northing',0.0],"
                             "PARAMETER['Central_Meridian',0.0],UNIT['Meter',1.0]]", "NO_PRESERVE_SHAPE", "",
                         "NO_VERTICAL")
print("Data has been projected to GCS_WGS_1984 successfully!")
print("STEP 11 COMPLETED SUCCESSFULLY!")


# 12) Extract data for each city
city_names_2 = []
# 12.1) Use a SearchCursor to iterate through each row's city name of the first big city or feature class.
with arcpy.da.SearchCursor(africa_big_cities_list_3[0], ["City_name"]) as cursor:
    for row in cursor:
        city_names_2.append(row[0])

# 12.2) Extract data for every city based on the city name field you just listed above.
n = 0
folder = "C:\\Tamirat_2024\\IGSSA_PhD\\Y2_S1\\PhD_Proposal\\PhD_Proposal\\Excel\\"
for city in city_names_2:
    cty = arcpy.Select_analysis(merged_data_2, "C4_city_" + city, "\"City_name\" = '%s'" % city)
    exl = arcpy.TableToExcel_conversion(cty, folder + city + ".xls", "NAME", "CODE")
    n += 1
    print("Data for " + city + " has been extracted and converted to Excel.")
print("City-wise data extraction completed for " + str(n) + " cities successfully!")

# 12.3) Make a list of study cities. This process gives you uniques list of study locations.
cities = arcpy.Statistics_analysis(merged_data_2, "C4_list_of_study_cities", "City_name FIRST;GC_UCN_MAI_2025 FIRST",
                                   "GC_POP_TOT_2025")

# 12.4) Add country names to the cities table.
arcpy.JoinField_management(cities, "FIRST_GC_UCN_MAI_2025", climate_data, "GC_UCN_MAI_2025", "GC_CNT_GAD_2025")
print("Cities table has been created successfully!")

# 12.5) Export data to Excel
output_excel = "C:\\Tamirat_2024\\IGSSA_PhD\\Y2_S1\\PhD_Proposal\\PhD_Proposal\\Excel\\C4_All_Cities.xls"  # Output excel.
arcpy.TableToExcel_conversion(cities, output_excel, "NAME", "CODE")
print("Data converted to Excel successfully!")
print("STEP 12 COMPLETED SUCCESSFULLY!")
# ########################################## End of the script ###################################################
# ########################################## End of the script ###################################################


