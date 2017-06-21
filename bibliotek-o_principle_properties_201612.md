LD4L Labs / LD4P Ontology Group
===============================

December 2016

General Recommendations
-----------------------

-   Use object properties where:
    1. The modeling warrants it - i.e., you have or could have something more to say about the object of the triple. Object properties with related classes allow for richer information capture and resource reuse.
	2. There is a (existing or potential) defined set of values (i.e., a controlled vocabulary).

-   If you are not wanting to use object properties because:
    -   there doesn’t exist a controlled vocabulary, Class, or other at present for the object values (which could be many):
        -   Create a controlled vocabulary in a new namespace.
        -   This ‘controlled vocabulary’ could be supported by creating a new Class and then using:
            -   Subclasses
            -   Named Individuals
            -   RDF instance data
    -   there is difficult existing or legacy data:
        -   You do not want to limit the modeling or richer capture of native RDF instance data because of past practices in legacy data. 
        -   Follow the [Legacy Literals recommendation](http://drive.google.com/open?id=1CyUl0tID3c62_klq77B2YFERtFtFrLeHULqWnjzLnvg) by modeling according to what fits best the needs of native RDF instance data. Legacy data will be captured on a string value (see the recommendation for details about precisely how it is captured) for future normalization.
        -   This allows a single query path to get what should be the same type of information, which is good RDF modeling practice **and** makes data normalization and cleaning efforts easier down the road.
    -   there is incomplete existing or legacy data:
        -   This follows the reasons for difficult existing or legacy data (above).
        -   Then, instead of planning for normalization, you’re just focused on enhancement (capturing further information or performing reconciliation).
-   There are cases when a datatype property should genuinely be used. This is when there will be nothing else to say about the object of a triple statement. These include but are not limited to:
    -   Encoded date values
    -   Counts or other numeric values
    -   A label literal
    -   Unstructured text that should remain unstructured (transcriptions, specific note values, etc.)
    -   Transcribed statements as literals that are specifically transcribed for a particular use case.


Other Patterns & Guidelines related to this Recommendation
----------------------------------------------------------

1.  [Legacy Literals](http://drive.google.com/open?id=1CyUl0tID3c62_klq77B2YFERtFtFrLeHULqWnjzLnvg) (Unstructured legacy data)
2.  [Notes and Annotations (including a discussion of bf:Note)](http://drive.google.com/open?id=1aGG19L48ljyHpw-wcGfshakU6DTxORBj6zoX8iygu3Q)
3.  Others as listed below.


Datatype Properties in BF2 versus LD4L-O
----------------------------------------

What follows is a review of datatype properties currently in BIBFRAME v.2 that have been discussed with regards to making (or not) these into object properties. The related LD4All Sprint Group Recommendation or Discussion Status for the property is in the second column.

This is not an exhaustive list of datatype properties, and many of the discussions will link out to other patterns or topic areas currently queued for LD4All Alignment Group discussion.
 
**BIBFRAME v.2**  <br /> Including openness of LC to changing from a datatype to an object property.  |  **LD4All Status** <br /> Including current recommendations from LD4All discussions on using these.
:------------- | :-------------
| **bf:acquisitionSource** <br />Datatype that could be object property. <br />Label: Source of acquisition<br />URI: http://id.loc.gov/ontologies/bibframe/acquisitionSource<br />Definition: "Information about an organization, etc., from which a resource may be obtained."<br />Comment: Used with Work or Instance<br />Domain: None<br />Range: rdfs:Literal<br />Modified: "2016-04-21 (New)" |  **ld4l:AcquisitionActivity bf:source foaf:Agent**<br />This should point to an organization or other type of entity resource, making it an Object Property.<br />In LD4All recommendations, it is seen as part of an Activity resource, namely, the source of ld4l:AcquisitionActivity captured with bf:source pointing to a foaf:Agent instance (URI for the Agent - probably, Organization). |
| **bf:acquisitionTerms** <br />Datatype that could be object property.<br /> Label: Terms of acquisition<br /> Definition: "Conditions under which the publisher, distributor, etc., will normally supply a resource, e.g., price of a resource." .<br /> Comment: Used with Work or Instance<br />Domain: None<br /> Range: rdfs:Literal<br />URI: http://id.loc.gov/ontologies/bibframe/acquisitionTerms |  **ld4l Activity Pattern ...**<br />This could point to a variety of types of information. This supports making this an Object Property with a new Class for having all the related information in a more granular way (i.e. bf:AcquisitionTerms bf:price “data” ; bf:license [license URI?]; etc.)<br /> [LD4All will use a subclass of Activity to cover this needed Class](http://drive.google.com/open?id=1-UWiCw50Q9s3vAU3FWZcyomRnPe_lp6ZTW868fjpMCQ). The generated LD4L Activity class will contain properties according to what type of information is found/contained by this resource (e.g. price, license, format, deal, etc.) |
| **bf:ascensionAndDeclination**<br /> Datatype that is a placeholder for legacy literals. <br /> URI: http://id.loc.gov/ontologies/bibframe/ascensionAndDeclination<br /> Domain: bf:Cartographic<br /> Range: rdfs:Literal<br /> Definition: "System for identifying the location of a celestial object in the sky covered by the cartographic content of a resource using the angles of right ascension and declination." . <br /> Label: "Cartographic ascension and declination" | **TBD** <br /> This is an extremely lossy data property when looking at MARC21 to BIBFRAME v.2 conversion. It should be supported by creating the appropriate modeling of an ontology fragment that best serves capturing this information. The legacy literals data is then captured as best able as outlined in the [legacy literals recommendation0(http://drive.google.com/open?id=1CyUl0tID3c62_klq77B2YFERtFtFrLeHULqWnjzLnvg). <br /> LD4All is leaving the modeling work for this data to the Geospatial extension. |
| bf:awards <br /> Datatype that is an informal note property. <br /> URI: http://id.loc.gov/ontologies/bibframe/awards <br /> Range: rdfs:Literal <br /> Definition: "Information on awards associated with the described resource." .  <br /> Comment: "Used with Work or Instance" .  <br /> Label: "Award note" | **ld4l:Award & ld4l:AwardReceipt** This note contains information about an Award received by the resource described. This is an ideal candidate for improved modeling starting with BIBFRAME v.2 natively-created RDF, albeit the conversion of existing data will be hard. <br /> LD4All is recommending following the [Awards pattern](http://drive.google.com/open?id=1qhTlta2ZlCeo7TinnSlUG_oUEqAqG6x5mIi0scT6aOI), which follows the [VIVO Ontology](http://www.vivoweb.org/downloads) in its approach to Awards - it uses Object Properties with Classes ld4l:Award and ld4l:AwardReceipt. It also recommends following the [Legacy Literals](http://drive.google.com/open?id=1CyUl0tID3c62_klq77B2YFERtFtFrLeHULqWnjzLnvg) recommendation when moving existing data to this Awards modeling construct. |
| **bf:changeDate** <br /> Datatype that should stay so. <br /> URI: http://id.loc.gov/ontologies/bibframe/changeDate <br /> Domain: bf:AdminMetadata <br /> Range: rdfs:Literal <br /> Definition: "Date or date and time on which the metadata was modified." .  <br /> subPropertyOf: bf:date  <br /> Label: Description change date | **TBD**  <br /> Dates should be datatype properties, in particular using [EDTF](https://www.loc.gov/standards/datetime/) encoding for capturing related information (approximate versus exact, date ranges, etc.) about the date in an standardized fashion. <br /> This is part of a forthcoming [Administration Metadata recommendation](http://drive.google.com/open?id=1sPT_r_nZob99pkn3EBkwZlB_rOdtjuW9JEnK5e7Jv-A), where these properties are applied to BIBFRAME RDF instance data in a different fashion. It doesn’t change using a datatype property (for LD4All, dcterms:date) for date, but would change where this assertion would occur. | 
|  **bf:classificationPortion** <br /> Datatype with ‘too many possible values to have any use as an object property’. <br /> URI: http://id.loc.gov/ontologies/bibframe/classificationPortion <br /> Domain: bf:Classification <br /> Range: rdfs:Literal <br />  Definition: "Classification number (single class number or beginning number of a span) that indicates the subject by applying a formal system of coding and organizing resources." .  <br /> Label: "Classification number" |  **bf:classificationPortion** <br />This is a property LD4All will be largely recommending to use as found in BIBFRAME. <br /> There is a [Classification Recommendation document](http://drive.google.com/open?id=1XnI1BoujCZonsGaKCuMUF588mqb_q0QvXev3JSc-QfY) to recommend future exploration if and how one could improve the Classification information captured through use of Object Properties and Classes. This is an area for future exploration, not a current recommendation for change.| 
| 
**bf:code**
Datatype with ‘too many possible values to have any use as an object property’.
URI: http://id.loc.gov/ontologies/bibframe/code
Range: rdfs:Literal
Definition: "String of characters that serves as a code representing information." . 
Comment: "Used with Unspecified"
Label: "Code"
|
 
TBD
[There is uncertainty in the LD4All Sprint Group around how this property would be used.](http://drive.google.com/open?id=1DZ7f8O4hpwGl9Hnw6HMN7gGS7BZAd74_IAxGS-fhhhY)
 
If this is primarily a way to capture MARC21 fixed field codes, it is probably recommended to review the various codes and determine if those shouldn’t have MARC code to URI (bf:Work or bf:Instance subclasses most likely, but other Classes involved) conversion mapping.
|
|
 
bf:coordinates
Datatype that is a placeholder for legacy literals.
URI: http://id.loc.gov/ontologies/bibframe/coordinates
Domain: bf:Cartographic
Range: rdfs:Literal
Definition: "Mathematical system for identifying the area covered by the cartographic content of a resource, expressed either by means of longitude and latitude on the surface of planets or by the angles of right ascension and declination for celestial cartographic content." . 
Label: "Cartographic coordinates"
|
BD
This is an extremely lossy data property when looking at MARC21 to BIBFRAME v.2 conversion. It should be supported by creating the appropriate modeling of an ontology fragment that best serves capturing this information (which would be an Object Property and needed Classes). The legacy literals data is then captured as best able in that fragment through the use of rdfs:label and rdfs:comment. See the [legacy literals recommendation](http://drive.google.com/open?id=1CyUl0tID3c62_klq77B2YFERtFtFrLeHULqWnjzLnvg).
 
LD4All is leaving the modeling work for this data to the Geospatial extension. 
|
|
 
bf:copyrightDate 
Datatype that should stay so.
URI: http://id.loc.gov/ontologies/bibframe/copyrightDate
Range: rdfs:Literal
Definition: "Date associated with a claim of protection under copyright or a similar regime." . 
subPropertyOf: bf:date 
Comment: "Used with Work or Instance" . 
Label: "Copyright date" 
|
 
ld4l:CopyrightActivity dcterms:date literal
Dates should be datatype properties, in particular using [EDTF](https://www.loc.gov/standards/datetime/) encoding for capturing related information (approximate versus exact, date ranges, etc.) about the date in an standardized fashion.
 
LD4All will use a subclass of ld4l:Activity called ld4l:CopyrightActivity. The ld4l:CopyrightActivity class will contain properties according to what type of information is found/contained by this resource, including date.
|
|
 
bf:contentAccessibility 
Datatype that could be object property.
URI: http://id.loc.gov/ontologies/bibframe/contentAccessibility 
Range: rdfs:Literal
Definition: "Information that assists those with a sensory impairment for greater understanding of content, e.g., captions." . 
Comment: "Used with Work or Instance" . 
Label: "Content accessibility note" . 
|
 
ld4l:hasAccessibility ld4l:Accessibility
 
The LD4All Sprint Group recommendation is to make this an Object Property with related Class (and subclasses). This is to manage being able to say more about the accessibility feature or hazard present in the resource. Read more on the [Content Accessibility Sprint Recommendation Document](http://drive.google.com/open?id=1VjcGbF3E4xrM_PI5mAR6LFDTlaWmR4llGZDSDYQ7MYU)
|
|
 
bf:count 
Datatype that should stay so.
URI: http://id.loc.gov/ontologies/bibframe/count
Range: rdfs:Literal
Definition: "Number associated with a measure of units, such as the number of units and/or subunits making up a resource." . 
Comment: "Used with Unspecified" . 
Label: "Number of units" 
|
 
bf:count
LD4All Sprint Group recommendation is to use this property as found in BIBFRAME, and to keep it as a datatype property. In some circumstances this data would be modeled using the recommendation in the [Dimensions Recommendation](https://docs.google.com/document/d/1QCVkYM3NR99NITi-ZWF8HkMYYDSkYrfRhZ30frQD7_w/edit?usp=drive_web). 
|
|
