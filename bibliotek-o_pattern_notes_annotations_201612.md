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
| **bf:Note** <br /> Label: Note <br /> URI: http://id.loc.gov/ontologies/bibframe/Note <br /> Definition: Information, usually in textual form, on attributes of a resource or some aspect of a resource. <br /> SubClass Of: http://www.w3.org/2000/01/rdfschema# <br /> Used with: http://id.loc.gov/ontologies/bibframe/note  <br />  | **oa:Annotation** <br /> URI: http://www.w3.org/ns/oa#Annotation <br /> Required Predicates: oa:hasTarget, rdf:type <br /> Recommended Predicates: oa:hasBody, oa:motivatedBy, dcterms:creator, dcterms:created <br /> Other Predicates: oa:bodyValue, oa:styledBy, dcterms:issued, as:generator |



BF2 Approach to Notes
=====================

For an in-depth explanation on use of notes in BIBFRAME 2.0 see the [BIBFRAME 2.0 Specifications, Notes](https://docs.google.com/document/d/1QrK6khdqxIWtinY0yQ8_1VEd_O18C9IbvML2UAQILIA/edit?usp=sharing).

Notes can be expressed in five different ways in BF 2.0: Informal notes, a plain note with no type expressed; a note with type implied (a bf:note used in conjunction with another property which provides context); as a note with a noteType expressed as a literal and as a note with a type expressed by an external class. BIBFRAME 2.0 does not internally define any note types.


LD4L-O v.1 Approach to Notes
============================

LD4L-O v.1 defines broad range of note properties, all of them subproperties of [ld4l:note](http://bib.ld4l.org/ontology.html#note), with a range of literal, as well as other datatype properties that cover textual information similar to BF2 notes, such as ld4l:custodialHistory and ld4l:accessibilityFeature. It does not define any classes for notes.

LD4L-O v.1 also includes classes and properties from the Web Annotation Model. It also includes an inverse for oa:hasTarget, ld4l:hasAnnotation.
