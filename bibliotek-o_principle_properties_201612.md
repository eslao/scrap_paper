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
| **bf:acquisitionTerms** <br />Datatype that could be object property.<br /> Label: Terms of acquisition<br /> Definition: "Conditions under which the publisher, distributor, etc., will normally supply a resource, e.g., price of a resource." .<br /> Comment: Used with Work or Instance<br />Domain: None<br /> Range: rdfs:Literal<br />URI: http://id.loc.gov/ontologies/bibframe/acquisitionTerms |  **ld4l Activity Pattern ...**<br />This could point to a variety of types of information. This supports making this an Object Property with a new Class for having all the related information in a more granular way (i.e. bf:AcquisitionTerms bf:price “data” ; bf:license [license URI?]; etc.)<br /> LD4All will use a subclass of Activity to cover this needed Class. The generated LD4L Activity class will contain properties according to what type of information is found/contained by this resource (e.g. price, license, format, deal, etc.) |
| **bf:ascensionAndDeclination**<br /> Datatype that is a placeholder for legacy literals.<br /> URI: http://id.loc.gov/ontologies/bibframe/ascensionAndDeclination<br /> Domain: bf:Cartographic<br /> Range: rdfs:Literal<br /> Definition: "System for identifying the location of a celestial object in the sky covered by the cartographic content of a resource using the angles of right ascension and declination." . <br /> Label: "Cartographic ascension and declination" | **TBD** <br /> 
This is an extremely lossy data property when looking at MARC21 to BIBFRAME v.2 conversion. It should be supported by creating the appropriate modeling of an ontology fragment that best serves capturing this information. The legacy literals data is then captured as best able as outlined in the  legacy literals recommendation. <br /> LD4All is leaving the modeling work for this data to the Geospatial extension. |
| bf:awards <br /> Datatype that is an informal note property. <br /> URI: http://id.loc.gov/ontologies/bibframe/awards <br /> Range: rdfs:Literal <br /> Definition: "Information on awards associated with the described resource." .  <br /> Comment: "Used with Work or Instance" .  <br /> Label: "Award note" | **ld4l:Award & ld4l:AwardReceipt** This note contains information about an Award received by the resource described. This is an ideal candidate for improved modeling starting with BIBFRAME v.2 natively-created RDF, albeit the conversion of existing data will be hard. <br /> LD4All is recommending following the Awards pattern, which follows the VIVO Ontology in its approach to Awards - it uses Object Properties with Classes ld4l:Award and ld4l:AwardReceipt. It also recommends following the Legacy Literals recommendation when moving existing data to this Awards modeling construct.




