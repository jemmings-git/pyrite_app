# Pyrite machine reading application
This is a fork of the [GeoDeepDive](https://geodeepdive.org) text mining application used in [Peters, Husson and Wilcots 2017, Geology](http://doi.org/10.1130/G38931.1). 

### Please see [the original stromatolites application](https://github.com/UW-Macrostrat/stromatolites_demo) for a comprehensive description of the method and dependencies.

This application uses a combination of Python and PostgreSQL.

### This code accompanies a manuscript submitted to *Science Advances* in May 2021
#### Title: Pyrite meta-analysis reveals modes of anoxia through geological time
#### Authors: Joseph F. Emmings<sup>1,2</sup>, Simon W. Poulton<sup>3</sup>, Joanna Walsh<sup>4,5</sup>, Kathryn A. Leeming<sup>1</sup>, Ian Ross<sup>6</sup>, Shanan Peters<sup>7</sup>

### Affiliations 
<sup>1</sup>British Geological Survey, Keyworth, Nottingham, NG12 5GG, UK.

<sup>2</sup>School of Geography, Geology and the Environment, University of Leicester, Leicester, LE1 7RH, UK.

<sup>3</sup>School of Earth and Environment, University of Leeds, Leeds, LS2 9JT, UK.

<sup>4</sup>Lyell Centre, British Geological Survey, Riccarton, Edinburgh EH14 4AS, UK.

<sup>5</sup>Ordnance Survey, Explorer House, Adanac Drive, Southampton, SO16 0AS, UK.

<sup>6</sup>Department of Computer Sciences, University of Wisconsin–Madison, Madison, Wisconsin 53706, USA. 

<sup>7</sup>Department of Geoscience, University of Wisconsin–Madison, Madison, Wisconsin 53706, USA. 

## Results Summary
The `results` table is a CSV file exported to the folder `/output`. In the file, each row is a stratigraphic name 
that contains pyrite according to the application logic - that is, each row is a "pyrite-stratigraphic name" tuple.

The results table in this fork is a clone of the original stromatolites demo.

The [pyrite results](https://geodeepdive.org/app_output/jemmings_with_pyrite_24Oct2019.zip) are imported and analysed in the accompanying pyrite-stats repository.

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
phrase| full phrase that serves as basis for the determination that the stratigraphic phrase contains pyrite.
