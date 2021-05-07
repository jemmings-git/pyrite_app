# Pyrite machine reading application
This is a fork of the [GeoDeepDive](https://geodeepdive.org) text mining application used in Peters, Husson and Wilcots [2017, Geology](http://doi.org/10.1130/G38931.1). 

# PLease see [the original stromatolites application](https://github.com/UW-Macrostrat/stromatolites_demo) for a comprehensive description of the method and depenencies.

This application uses a combination of Python and PostgreSQL.

## Results Summary
The `results` table is a CSV file exported to the folder `/output`. In the file, each row is a stratigraphic name 
that contains pyrite according to the application logic - that is, each row is a "pyrite-stratigraphic name" tuple.
Columns of each row contain information about the extracted tuple, including which document and phrase it came from and the link
between the discovered stratigraphic name and the Macrostrat database (if such a link exists). The columns are detailed below:

Column | Description 
-------|--------
result\_id| identifier for result tuple from the PostgreSQL results table
docid| identifier for the relevant document from the GeoDeepDive database, with metadata for it available through the GeoDeepDive API (i.e., [558dcf01e13823109f3edf8e](https://geodeepdive.org/api/articles?id=558dcf01e13823109f3edf8e))
sentid| identifier for sentence within the specified document where the tuple was extracted
target\_word| pyrite word (i.e., pyritic).
strat\_phrase\_root| "unique" portion of the identified stratigraphic name inferred to contained pyrite (i.e., "Wood Canyon" from the "Wood Canyon Formation")
strat\_flag| word that signified to the strat\_name extractor ([ext_strat_phrase.py](https://github.com/UW-Macrostrat/stromatolites/blob/master/udf/ext_strat_phrases.py)) that a word combination was a likely stratigraphic phrase (i.e., "Formation" for the "Wood Canyon Formation"). Note that this field could be "mention" for informal usage (i.e. "Wood Canyon pyrite"), if a name has been formally defined in the same document.
strat\_name\_id| stratigraphic name id for the Macrostrat database. For example, [this api call](https://macrostrat.org/api/defs/strat_names?strat_name_id=2330) retrieves the definition for the "Wood Canyon Formation" from the Macrostrat database. [This api call](https://macrostrat.org/api/units?strat_name_id=2330) retrieves all lithostratigraphic units linked to the "Wood Canyon Formation" from the Macrostrat database. Note that this field could be "0" if the stratigraphic name describes a rock body outside of Macrostrat's areal coverage. If a name is linked to multiple stratigraphic names in the Macrostrat database, each identifier is separated by a "~" (i.e. "61671~446~2442").
in\_ref| application determination ([ext_references.py](https://github.com/UW-Macrostrat/stromatolites/blob/master/udf/ext_references.py)) if the extracted tuple came from the reference list of the specified document.
source| classifier indicating whether the extraction was from the same sentence ("in\_sent") or from a nearby sentence ("out\_sent").
phrase| full phrase that serves as basis for the determination that the stratigraphic phrase contains stromatolite fossils.

## License
CC-BY 4.0 International
