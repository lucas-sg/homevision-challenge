# HomeVision Challenge Preliminary Analysis

Started working on the challenge on September 2 at around 11pm.

1. Opened the file in the Notion page, copied its content and pasted it into this repo. Skimmed the file over to get a first glance of any visual cues I could find in repeating chunks of text.
2. Started looking for existing tools to better analyze the file, such as a hex editor and any system tools that might already be available to me. I'd already used a hex editor once for a cryptography course I had in college in order to find out how to decrypt an unknown image file.
3. Used the `strings -a sample.env` command in order to better read the contents of the file (the `-a` is to scan the entire file, not just the data sections, just in case). I noticed the `**%%DOCU` pattern repeating a couple of times, followed by what seemed like headers indicating the contents that followed until the next `**%%DOCU` chunk.
4. Tried opening the file with ImHex but couldn't find much of interest, just that the entropy is rather low, so it's not like the file is mostly encrypted (I didn't read the entired file by sight, just skimmed over it, maybe I had missed some big chunks that were encrypted).
5. Used `binwalk` in order to possibly extract some files out of this file and got this as a result:

```txt
Scan Time:     2025-09-03 00:34:34
Target File:   /home/lucas/Documents/Repos/homevision-challenge/sample.env
MD5 Checksum:  83f48f1e8cbaddc3cb95881c018f50f9
Signatures:    411

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
73140         0x11DB4         XML document, version: "1.0"
79193         0x13559         XML document, version: "1.0"
87478         0x155B6         Neighborly text, "NeighborhoodLocationType="Suburban" _BuiltupRangeType="Over75Percent" _GrowthPaceType="Stable" _PropertyValueTrendType="Stable" rcent" _GrowthPaceType="Stable" _PropertyValueTrendType="Stable" _DemandSupplyType="Shortage" _TypicalMarketingTimeDurationType="
88421         0x15965         Neighborly text, "NeighborhoodBoundariesDescription="" /></NEIGHBORHOOD_EXTENSION_SECTION_DATA></NEIGHBORHOOD_EXTENSION_SECTION></NEIGHBORHOOD_EXT_SECTION_DATA></NEIGHBORHOOD_EXTENSION_SECTION></NEIGHBORHOOD_EXTENSION></NEIGHBORHOOD><_TAX _YearIdentifier="" _TotalTaxAmount="
89285         0x15CC5         Neighborly text, "Neighborhood" _Comment="" /><PROPERTY_ANALYSIS _Type="UtilitiesAndOffSiteImprovementsConformToNeighborhood" _ExistsIndicator="N"AndOffSiteImprovementsConformToNeighborhood" _ExistsIndicator="N" _Comment="test" /><PROPERTY_ANALYSIS _Type="AdverseSiteConditi"
89379         0x15D23         Neighborly text, "Neighborhood" _ExistsIndicator="N" _Comment="test" /><PROPERTY_ANALYSIS _Type="AdverseSiteConditions" _Comment="" /><PROPERTY_ANANALYSIS _Type="AdverseSiteConditions" _Comment="" /><PROPERTY_ANALYSIS _Type="PropertyCondition" _Comment="" /><_OWNER _Name="""
95797         0x17635         XML document, version: "1.0"
105757        0x19D1D         XML document, version: "1.0"
112064        0x1B5C0         eCos RTOS string reference: "ECOST><TOT></TOT></INDICATEDVALUECOST><ESTSITEVALUE></ESTSITEVALUE><DWELLINGONE><SQFT></SQFT><SQFTAMT></SQFTAMT><AMTTOT></AMTTOT"
112096        0x1B5E0         eCos RTOS string reference: "ECOST><ESTSITEVALUE></ESTSITEVALUE><DWELLINGONE><SQFT></SQFT><SQFTAMT></SQFTAMT><AMTTOT></AMTTOT></DWELLINGONE><DWELLINGTWO DESC"
```

This seems like the biggest finding so far since it seems like there are a couple of XML files embedded inside, plus some chunks of text which have some pattern but can't be deciphered just yet (these are the *Neighborly text* rows) and more importantly some eCos references, which indicate that there are some contents related to that OS.
