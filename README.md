# How to:

for converting the output of cdm_xporter/scrape_cdm.py into mods format (especially for ingest into Islandora)

  1) Turn on your python3 virtualenv.  It needs lxml.  Ananconda for Mac & Windows works well.
  
  2) convert_to_mods.py expects to find /Cached_Cdm_files/ (the output of scrape_cdm.py) in a sibling folder beside the cDM_to_mods folder.
  
  3) In the command line, navigate into the cDM_to_mods directory.
  
  4) `python convert_to_mods.py {alias}`
  
  5) `python post_converstion_cleanup.py {alias}`

## Scripts

convert_to_mods.py:
  - applies the cDM_to_mods/mappings_file/{alias}.csv to the Cached_Cdm_files data to create a rough mods file.
  - performs xsl transformations named in the cDM_to_mods/alias_xlsts/{alias}.txt file to refine the mods.
  - validates each mods record against a local copy of the mods schema.
  - complains loudly if anything fails.

The xsl scripts are found in the cDM_to_mods/xsl/ folder.
The final output can be found in cDM_to_mods/output/{alias}_simple/final_format and cDM_to_mods/output/{alias}_compound/final_format.  Seperating simples from compounds facilitates easier uploading into Islandora.  

post_conversion_cleanup.py:
  - verifies that each object in the collection was converted into a mod file.  
  - pulls the binaries from the source_directory into the proper position within the output files.
  - complains if there is not a {.jp2, .mp3, .mp4, .pdf} for each mods.
  - creates a structure file, which is necessary for Islandora Compound Batch Upload.  
  - checks all the mods for access restrictions, and reports those to cDM_to_mods/{alias}_restrictions.txt  
