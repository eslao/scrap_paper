bibliotek-o Notes & Annotations Pattern
=======================================

***

NOTE: the following represents the direction taken by the LD4L Labs and LD4P Ontology Group in the development of bibliotek-o and may not be fully formed. This pattern document was used internally to define a direction and is shared with the intention of contextualizing a pattern found within the ontology; terms specified below may not fully align to the ontology as published. Further, discussion of BIBFRAME 2.0 may be out-of-date.
***

2016 December

This paper reviews the use of notes in BIBFRAME 2.0, including general notes, informal notes, and notes with a noteType expressed or implied. For many of the note properties, the adoption of the Web Annotation Model is recommended. A note in the cataloging context can be interpreted as a cataloger-asserted statement about the resource being cataloged, or an existing statement about the resource whose external source is being asserted. A resource does not itself have a summary, but a cataloger or publisher asserts one. Furthermore, the Web Annotation model is an appropriate framework to support use cases, such as those involving rare materials, in which institution-specific notes are needed. By using this model in all cases, we avoid creating multiple, arbitrary patterns for different types of notes or different types of materials, and avoid closing off options that may become useful in a linked open data environment.

We view this an experiment to be evaluated through application in production.

Pattern Overview
----------------

-   The majority of informal note properties have been addressed by related recommendations, see [Object versus DataType Properties](https://docs.google.com/document/d/1Lp9Od8x1XZDvmBkNE2lK7bF_zOsaOL77zgITodV5GM0/edit?usp=sharing), [Content Accessibility](https://docs.google.com/document/d/1VjcGbF3E4xrM_PI5mAR6LFDTlaWmR4llGZDSDYQ7MYU/edit?usp=sharing) and [Activities](https://docs.google.com/document/d/1-UWiCw50Q9s3vAU3FWZcyomRnPe_lp6ZTW868fjpMCQ/edit?usp=sharing).
-   Use Web Annotation Model for: bf:credits, bf:custodialHistory, bf:historyOfWork, bf:natureOfContent, bf:preferredCitation, bf:summary, bf:review, bf:systemRequirements, and bf:tableOfContents. 
-   For properties that we prefer to describe as Activities, such as bf:credits or bf:custodialHistory, we understand that a human-readable note may still have a place in aiding the user’s understanding, and that legacy data may not be easy to parse into discrete Activities.
-   Use existing OA named individuals to describe motivation when possible.
-   Mint additional high level motivations, as narrower terms under oa:describing, oa:assessing or oa:linking, as appropriate.
	- oa:describing
		- bib:summarizing
		- bib:listing
	- ld4:specifying
		- oa:assessing
	- bib:reviewing
		- oa:linking
	- bib:linkingTableOfContents
-   Mint additional named individuals for more specific motivations, as narrower terms under bib:summarizing, ld4:specifying, bib:listing or bib:reviewing, as appropriate. As stated above, this is considered to be an experimental approach; therefore no plans have been put in place to maintain these specific terms.
	- ld4:specifying
		- bib:specifyingPreferredCitation
		- bib:specifyingCustodialHistory
		- bib:specifyingHistoryOfWork
		- bib:specifyingNatureOfContent
		- bib:specifyingSystemRequirements
		- bib:specifyingContents
	- bib:listing
		- bib:listingCredits
-   Create a new object property, bib:isTargetOf, inverse of oa:hasTarget, to link from an annotated resource to an oa:Annotation.


Areas for Future Work
---------------------

-   There are many more motivations for notes in resource descriptions than represented by the informal note properties listed below. [MARC 21 Bibliographic Data, 5xx](https://www.loc.gov/marc/bibliographic/bd5xx.html) , the [MODS note types](http://www.loc.gov/standards/mods/mods-notes.html) list or content standards such as [DCRM(G), area 7](https://rbms.info/dcrm/dcrmg/) may be sources that can be used to identify additional Named Individuals for oa:Motivation that are frequently needed in day-to-day cataloging workflows.

-   The Web Annotations model allows for expression of much more detail than is specified in this recommendation. We leave it to each project to determine which properties to implement in support of their specific use cases, so that their outcomes can inform future recommendations.


Recommended Model
-----------------

##Side by Side Examples of BIBFRAME 2.0 and bibliotek-o


--------------------------------------------------------------------------
    #BIBFRAME2
    
    _:w1 a bf:Work ;
		bf:summary _:s1 .
    _:s1 a bf:Summary ;
		rdfs:label “This film consists of alternating black and white frames.--IMDb” .
    
    _:w2 a bf:Work ;
		bf:hasInstance :inst1 ;
		bf:credits “Strips of film coated with glue and powdered metal were ‘electrocuted’ at the HFA’s Carpenter Center at a performance given by Tony Conrad April 7, 2008. The film was then processed before the audience’s very eyes, dried (with the help of a Harvard student and his hair drier), and given to HFA film conservator Liz Coffey to project from the middle of the theatre using two Eiki slot-load projector with sound.” .

	_:inst1 a bf:Instance ;
		bf:hasItem _:item1 .
    
    _:item1 a bf:Item ;
		bf:note _:n1 ;
		bf:custodialHistory “Donated to the Harvard Film Archive by Tony Conrad.” .
    
    _:n1 a bf:Note ;
		bf:noteType “condition” ;
		rdfs:label “Footage is approximately the height of Tony Conrad. Scratched during performance. One loop was damaged by sprockets during performance.” .
		
--------------------------------------------------------------------------

    # bibliotek-o v.2

    _:w1 a bf:Work ;                                                                                                                           
		bib:isTargetOf _:s1 .                                                
                                                                          
    _:s1 a oa:Annotation ;                                                                                             
		oa:hasTarget _:w1 ;                                                  
        oa:motivatedBy bib:summarizing ;                                      
		oa:hasBody _:b1 ;                                                    
        dcterms:creator                                                       
    <https://library.harvard.edu/hfa/ld4l](https://library.harvard.edu/hfa/ld4l> .

    _:b1 a oa:SpecificResource ;                                         
		oa:hasSource <http://www.imdb.com/title/tt0059182/plotsummary> ;                                                                     
		oa:hasSelector [                                                    
			a oa:TextQuoteSelector ;                                             
			oa:exact "This film consists of alternating black and white frames."  
		]                                                                    
                                                                          
    _:w2 a bf:Work ;                                                                                                                           
		bf:hasInstance :inst1 ;                                               
		bib:isTargetOf _:n1 .                                                
                                                                          
    _:n1 a oa:Annotation ;                                               													
		oa:hasTarget _:w2 ;                                                                                                                        
		oa:motivatedBy bib:listingCredits ;                                                                                                        
		oa:hasBody _:b2 ;                                                                                                                  
		dcterms:creator                                                       
		<https://library.harvard.edu/hfa/ld4l](https://library.harvard.edu/hfa/ld4l)> .                                                                                                             
                                                                          
    _:b2 a oa:TextualBody ;                                              
		rdf:value “Strips of film coated with glue and powdered metal were ‘electrocuted’ at the HFA’s Carpenter Center at a performance given by Tony Conrad April 7, 2008. The film was then processed before the audience’s very eyes, dried (with the help of a Harvard student and his hair drier), and given to HFA film conservator Liz Coffey to project from the middle of the theatre using two Eiki slot-load projector with sound.” .                                              
                                                                          
    _:inst1 a bf:Instance ;                                                                                                        
		bf:hasItem _:item1 .                                                 
                                                                          
    _:n2 a oa:Annotation ;                                                                                                                    
		oa:hasTarget _:item1 ;                                               
		oa:motivatedBy bib:specifyingCustodialHistory ;                       
		oa:hasBody _:b3 ;                                                    
        dcterms:creator <https://library.harvard.edu/hfa/ld4l](https://library.harvard.edu/hfa/ld4l> .                                                                     
                                                                          
    _:b3 a oa:TextualBody ;                                                                                                  
		rdf:value “Donated to the Harvard Film Archive by Tony Conrad.” .     
                                                                          
    _:n3 a oa:Annotation ;                                                                                                                    
		oa:hasTarget _:item1 ;                                               
                                                                          
    oa:motivatedBy oa:describing ;                                                                                                      
		oa:hasBody _:b4 ;                                                                                                         
		dcterms:creator <https://library.harvard.edu/hfa/ld4l](https://library.harvard.edu/hfa/ld4l)> .                                                                                                       
                                                                          
    _:b4 a oa:TextualBody ;                                                                                                        
		rdf:value “Footage is approximately the height of Tony Conrad. Scratched during performance. One loop was damaged by sprockets during performance.” .                                                

-----------------------------------------------------------------------



Recommended classes and properties
----------------------------------

Some recommendations below have been copied from related documents;
please follow links for the most up-to-date details.

| **BIBFRAME 2.0** | **bibliotek-o** |
| :--------------- | :-------------- |
| Involved Classes                   |
| **bf:Note** <br /> Label: Note <br /> URI: http://id.loc.gov/ontologies/bibframe/Note <br /> Definition: Information, usually in textual form, on attributes of a resource or some aspect of a resource. <br /> SubClass Of: http://www.w3.org/2000/01/rdfschema# <br /> Used with: http://id.loc.gov/ontologies/bibframe/note  <br />  | **oa:Annotation** <br /> URI: http://www.w3.org/ns/oa#Annotation <br /> Required Predicates: oa:hasTarget, rdf:type <br /> Recommended Predicates: oa:hasBody, oa:motivatedBy, dcterms:creator, dcterms:created <br /> Other Predicates: oa:bodyValue, oa:styledBy, dcterms:issued, as:generator | **bf:Summary** <br /> Label: Summary <br /> URI:
http://id.loc.gov/ontologies/bibframe/Summary <br /> Definition: Description of the content of a resource, such as an abstract, summary, etc.. <br /> SubClass Of: http://www.w3.org/2000/01/rdfschema#Resource <br /> Used with: http://id.loc.gov/ontologies/bibframe/summary
| **bib:summarizing (Named Individual of oa:Motivation)** <br /> oa:describing > bib:summarizing |
| **bf:TableOfContents** <br /> Label: Table of contents <br /> URI: http://id.loc.gov/ontologies/bibframe/TableOfContents <br />  Definition: Table of contents of a resource. <br /> SubClass Of: http://www.w3.org/2000/01/rdfschema#Resource <br /> Used with:
http://id.loc.gov/ontologies/bibframe/tableOfContents | **bib:specifyingContents (Named Individual of oa:Motivation)**  <br /> oa:describing > bib:specifying > bib:specifyingContents <br /> **bib:linkingTableOfContents (Named Individual of oa:Motivation)** <br /> 
oa:linking > bib:linkingTableOfContents |
| **bf:Review** <br /> Label: Review  <br /> URI: http://id.loc.gov/ontologies/bibframe/Review <br /> Definition: Review of a resource <br /> SubClass Of: http://www.w3.org/2000/01/rdfschema#Resource <br /> Used with: bf:review | **bib:reviewing (Named Individual of
oa:Motivation)** <br /> oa:assessing > bib:reviewing |
| | **oa:Motivation** <br />  IRI: http://www.w3.org/ns/oa#Motivation <br /> SubClass Of: skos:Concept <br /> Range Of: oa:motivatedBy, oa:hasPurpose|
| | **oa:TextualBody** <br /> IRI: http://www.w3.org/ns/oa#TextualBody <br /> Equivalent Classes: as:Note <br /> Required Predicates: rdf:value <br /> Recommended Predicates: dc:format, dc:language, rdf:type |
| Involved Properties                |
| **bf:note** <br /> Label: Note <br /> URI: http://id.loc.gov/ontologies/bibframe/note <br /> Definition: General textual information relating to a resource, such as Information about a specific copy of a resource or information about a particular attribute of a resource. <br /> Comment: Used with Unspecified <br /> Range: http://id.loc.gov/ontologies/bibframe/Note | **bib:isTargetOf** <br />  Definition: The relationship between a Target and an Annotation <br /> **oa:hasTarget**  <br /> IRI: http://www.w3.org/ns/oa#hasTarget
Definition: The relationship between an
Annotation and its Target.
Domain: oa:Annotation
|
|
bf:noteType
Label: Note type
URI:
http://id.loc.gov/ontologies/bibframe/noteType
Definition: Type of note
Used with:
http://id.loc.gov/ontologies/bibframe/Note
Range: Literal
|

|
|

|
oa:motivatedBy
IRI: http://www.w3.org/ns/oa#motivatedBy
Definition: The relationship between an
Annotation and a Motivation that describes
the reason for the Annotation's creation.
Domain: oa:Annotation
Range: oa:Motivation
|
|
oa:hasBody
IRI: http://www.w3.org/ns/oa#hasBody
Definition: The object of the relationship is a
resource that is a body of the Annotation.
Domain: oa:Annotation
|
|

|
oa:hasBody
IRI: http://www.w3.org/ns/oa#hasBody
Definition: The object of the relationship is a
resource that is a body of the Annotation.
Domain: oa:Annotation
|
|
bf:contentAccessibility
Label: Content accessibility note
URI:
http://id.loc.gov/ontologies/bibframe/contentAcc
essibility
Definition: Information that assists those with a
sensory impairment for greater understanding
of content, e.g., captions.
Comment: Used with Work or Instance
Range: Literal
|
See: Content Accessibility discussion paper
|
|
bf:credits
Informal note property
Label: Credits note
URI:
http://id.loc.gov/ontologies/bibframe/credits
Definition: Information in note form of credits
for persons or organizations who have
participated in the creation and/or production of
the resource.
Comment: Used with Work or Instance.
Range: Literal
|
bib:listingCredits (Named Individual of
oa:Motivation)
oa:describing > bib:listing > bib:listingCredits
Definition: The motivation for when the user
needs to record information about one or
multiple agents and their roles in the creation
and/or production of the resource in one
literal instead of parsing it out into discrete
classes and properties.
|
|
bf:awards
Informal note property
Label: Award note
URI:
http://id.loc.gov/ontologies/bibframe/awards
Definition: Information on awards associated
with the described resource.
Comment: Used with Work or Instance
Range: Literal
|
See recommendation in Object versus
DataType Properties document.
notes
from
the
ontology
call
on
2/28:
Conclusion: postpone for future work, use
bf:awards, request changes to
bf:grantingInstitution from LC
|
|
See recommendation in Object versus
DataType Properties document.
notes
from
the
ontology
call
on
2/28:
Conclusion: postpone for future work, use
bf:awards, request changes to
bf:grantingInstitution from LC
|
|
bf:natureOfContent
Informal note property
Label: Content nature
URI:
http://id.loc.gov/ontologies/bibframe/natureOfCo
ntent
Definition: Characterization that epitomizes the
primary content of a resource, e.g., field
recording of birdsong; combined time series
analysis and graph plotting system.
Comment: Used with Work or Instance
Range: Literal
|
bib:specifyingNatureOfContent
(Named Individual of oa:Motivation)
oa:describing > bib:specifying >
bib:specifyingNatureOfContent
Definition:
The motivation for when the user
needs to characterize the primary content of
the resource.
|
|
bf:editionEnumeration
Informal note property
Label: Edition enumeration
URI:
http://id.loc.gov/ontologies/bibframe/editionEnu
meration
Definition: Enumeration of the edition; usually
transcribed.
Domain: Instance
Range: Literal
|
See recommendation in Object versus
dataType Property document
|
|
bf:editionStatement
Informal note property
Label: Edition statement
URI:
http://id.loc.gov/ontologies/bibframe/editionStat
ement
Definition: Information identifying the edition or
version of the resource and associated
statements of responsibility for the edition;
usually transcribed.
Domain: Instance
Range: Literal
|
See recommendation in Object versus
dataType Property document
|
|
bf:historyOfWork
Informal note property
Label: History of the work
URI:
http://id.loc.gov/ontologies/bibframe/historyOfW
ork
Definition: Information about the history of a
Work.
Domain: Work
Range: Literal
|
bib:specifyingHistoryOfWork (Named
Individual of oa:Motivation)
oa:describing > bib:specifying >
bib:specifyingHistoryOfWork
Definition:
The motivation for when the user
needs to record information about the history
of a work.
|
|
bf:preferredCitation
Informal note property
Label: Preferred citation
URI:
http://id.loc.gov/ontologies/bibframe/preferredCi
tation
Definition: Citation to the resource preferred
by its custodian of the resource.
rdfs:comment: Used with Work or Instance
Range: Literal
|
bib:specifyingPreferredCitation
(Named Individual of oa:Motivation)
oa:describing > bib:specifying >
bib:specifyingPreferredCitation
Definition: The motivation for when the user
wants to specify a preferred citation for a resource.
|
|
bf:provisionActivityStatement
Informal note property
Label: Provider statement
URI:
http://id.loc.gov/ontologies/bibframe/provisionAc
tivityStatement
Definition: Statement relating to providers of a
resource; usually transcribed.
Domain: Instance
Range: Literal
|
See recommendation in Activities document
|
|
bf:responsibilityStatement
Informal note property
Label: Creative responsibility statement
URI:
http://id.loc.gov/ontologies/bibframe/responsibili
tyStatement
Definition: Statement relating to any persons,
families, or corporate bodies responsible for the
creation of, or contributing to the content of a
resource; usually transcribed.
Domain: Instance
Range: Literal
|
See recommendation in Object versus
dataType Property document
|
|
bf:review
Label: Review content
URI: http://id.loc.gov/ontologies/bibframe/review
Definition: Review of a resource
Comment: Used with Work or Instance
Range: Review
|
bib:reviewing (Named Individual of
oa:Motivation)
oa:assessing > bib:reviewing
|
|
bf:seriesEnumeration
Informal note property
Label: Series enumeration
URI:
http://id.loc.gov/ontologies/bibframe/seriesEnu
meration
Definition: Series enumeration of the resource;
usually transcribed.
Domain: Instance
Range: Literal
|
See recommendation in Object versus
dataType Property document
|
|
bf:seriesStatement
Informal note property
Label: Series statement
URI:
http://id.loc.gov/ontologies/bibframe/seriesState
ment
Definition: Statement of the series the
resource is in; usually transcribed; includes the
ISSN if applicable.
Domain: Instance
Range: Literal
|
See recommendation in Object versus
dataType Property document
|
|
bf:subseriesEnumeration
Informal note property
Label: Series statement
URI:
http://id.loc.gov/ontologies/bibframe/seriesState
ment
Definition: Statement of the series the
resource is in; usually transcribed; includes the
ISSN if applicable.
Domain: Instance
Range: Literal
|
See recommendation in Object versus
dataType Property document
|
|
bf:subseriesStatement
Informal note property
Label: Subseries statement
URI:
http://id.loc.gov/ontologies/bibframe/subseriesS
tatement
Definition: Statement of the subseries the
resource is in; usually transcribed; includes the
ISSN if applicable.
Domain: Instance
Range: Literal
|
See recommendation in Object versus
dataType Property document
|
|
bf:summary
Informal note property
Label: Summary content
URI:
http://id.loc.gov/ontologies/bibframe/summary
Definition: Summary or abstract of the
resource described.
Domain: Work or Instance
Range: Literal
|
bib:summarizing (Named Individual of
oa:Motivation)
oa:describing > bib:summarizing
Definition: The motivation for when the user
needs to add a summary or abstract to the
resource being described.
|
|
bf:systemRequirements
Informal note property
Label: Equipment or system requirements
URI:
http://id.loc.gov/ontologies/bibframe/systemReq
uirements
Definition: Equipment or system requirements
beyond what is normal and obvious for the type
of carrier or type of file, such as make and
model of equipment or hardware, operating
system, amount of memory, programming
language, other necessary software, any plugins
or peripherals required to play, view, or run
the resource, etc.
Domain: Instance
Range: Literal
|
bib:specifyingSystemRequirements
(Named Individual of oa:Motivation)
oa:describing > bib:specifying >
bib:specifyingSystemRequirements
Definition: The motivation for when the user
needs to record any equipment or system
requirements beyond what is normal and
obvious for the type of carrier or type of file.
|
|
bf:tableOfContents
Informal note property
Label: Table of contents content
URI:
http://id.loc.gov/ontologies/bibframe/tableOfCon
tents
Definition: Table of contents of the described
resource.
Domain: Work or Instance
Range: bf:TableOfContents
|
bib:specifyingContents (Named
Individual of oa:Motivation)
oa:describing > bib:specifying >
bib:specifyingContents
bib:linkingTableOfContents (Named
Individual of oa:Motivation)
oa:linking > bib:linkingTableOfContents
|
|
bf:custodialHistory
Informal note property
Label: Custodial history
URI:
http://id.loc.gov/ontologies/bibframe/custodialHi
story
Definition: Information about the provenance,
such as origin, ownership and custodial history
(chain of custody), of a resource.
Comment: Used with Work, Instance or Item
Range: Literal
|
bib:specifyingCustodialHistory
(Named Individual of oa:Motivation)
oa:describing > bib:specifying >
bib:specifyingCustodialHistory
Definition: The motivation for when the user
needs to record information about the history
of the resource being described in one literal.
|

BF2 Approach to Notes
=====================

For an in-depth explanation on use of notes in BIBFRAME 2.0 see the [BIBFRAME 2.0 Specifications, Notes](https://docs.google.com/document/d/1QrK6khdqxIWtinY0yQ8_1VEd_O18C9IbvML2UAQILIA/edit?usp=sharing).

Notes can be expressed in five different ways in BF 2.0: Informal notes, a plain note with no type expressed; a note with type implied (a bf:note used in conjunction with another property which provides context); as a note with a noteType expressed as a literal and as a note with a type expressed by an external class. BIBFRAME 2.0 does not internally define any note types.


LD4L-O v.1 Approach to Notes
============================

LD4L-O v.1 defines broad range of note properties, all of them subproperties of [ld4l:note](http://bib.ld4l.org/ontology.html#note), with a range of literal, as well as other datatype properties that cover textual information similar to BF2 notes, such as ld4l:custodialHistory and ld4l:accessibilityFeature. It does not define any classes for notes.

LD4L-O v.1 also includes classes and properties from the Web Annotation Model. It also includes an inverse for oa:hasTarget, ld4l:hasAnnotation.
