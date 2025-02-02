# Supplementary material (SEMANTiCS 2024 poster paper) for preliminary experiments

**NOTE**: Contrary to the submission poster paper, ontology variables (O1, O2) are swapped here. Otherwise, the example is the same.

## Input
Input: 
- O1=https://oaei.ontologymatching.org/2024/conference/data/ekaw.owl
- O2=https://oaei.ontologymatching.org/2024/conference/data/cmt.owl, 
- input_alignment = [https://oaei.ontologymatching.org/2024/conference/data/subset1-cmt-ekaw.rdf](https://oaei.ontologymatching.org/2024/conference/data/subset1-cmt-ekaw.rdf); {O1:Paper=O2:Paper; O1:Person=O2:Person}
- *Class by Attribute Type* (CAT) alignment pattern from [Scharffe09](https://scholar.google.com/scholar_url?url=https://www.academia.edu/download/30806287/manuscript.pdf&hl=en&sa=T&oi=gsb-gga&ct=res&cd=0&d=621239557845776854&ei=9Ut1Zvj6B6aty9YP54e9kAc&scisig=AFWwaeZPfF1N_zmYFPxB4qOSuqY8): this pattern specifies equivalence between a class in O1 and a class in O2 restricted on its scope using existential restriction; in Manchester OWL syntax: *O1:Class1 EquivalentTo O2:Class1 and (O2:property some O2:Class2)*.



## Step 1

SPARQL queries for CAT pattern.

SPARQL query1 for O1: 

``` 
SELECT DISTINCT ?class1 ?property1 ?class2 ?ent1 
	WHERE { 
		?property1 rdfs:domain ?ent1 .
		?property1 rdfs:range ?class1 .
		?class2 rdfs:subClassOf ?class1 
	} 
``` 

SPARQL query2 for O2: 
```
SELECT DISTINCT ?class1 ?ent1

	 WHERE {
	
	 	?class1 rdfs:subClassOf* ?ent1
	
	 }
```

From input_alignment:
```
O1:?ent1=O2:?ent1
```

SPARQL results for SPARQL query1 for O1:

```
?ent1=<http://ekaw#Paper>;?class1=<http://ekaw#Review>;?class2=<http://ekaw#Positive_Review>;?property1=<http://ekaw#hasReview>
?ent1=<http://ekaw#Paper>;?class1=<http://ekaw#Review>;?class2=<http://ekaw#Neutral_Review>;?property1=<http://ekaw#hasReview>
?ent1=<http://ekaw#Paper>;?class1=<http://ekaw#Review>;?class2=<http://ekaw#Negative_Review>;?property1=<http://ekaw#hasReview>
?ent1=<http://ekaw#Paper>;?class1=<http://ekaw#Possible_Reviewer>;?class2=<http://ekaw#PC_Member>;?property1=<http://ekaw#hasReviewer>

?ent1=<http://ekaw#Person>;?class1=<http://ekaw#Document>;?class2=<http://ekaw#Abstract>;?property1=<http://ekaw#authorOf>
?ent1=<http://ekaw#Person>;?class1=<http://ekaw#Document>;?class2=<http://ekaw#Review>;?property1=<http://ekaw#authorOf>
?ent1=<http://ekaw#Person>;?class1=<http://ekaw#Document>;?class2=<http://ekaw#Multi-author_Volume>;?property1=<http://ekaw#authorOf>
?ent1=<http://ekaw#Person>;?class1=<http://ekaw#Document>;?class2=<http://ekaw#Web_Site>;?property1=<http://ekaw#authorOf>
?ent1=<http://ekaw#Person>;?class1=<http://ekaw#Document>;?class2=<http://ekaw#Programme_Brochure>;?property1=<http://ekaw#authorOf>
?ent1=<http://ekaw#Person>;?class1=<http://ekaw#Document>;?class2=<http://ekaw#Flyer>;?property1=<http://ekaw#authorOf>
?ent1=<http://ekaw#Person>;?class1=<http://ekaw#Document>;?class2=<http://ekaw#Paper>;?property1=<http://ekaw#authorOf>
```

SPARQL results for SPARQL query2 for O2:

```
?ent1=<http://cmt#Paper>;?class1=<http://cmt#PaperAbstract>
?ent1=<http://cmt#Paper>;?class1=<http://cmt#PaperFullVersion>

?ent1=<http://cmt#Person>;?class1=<http://cmt#ProgramCommitteeMember>
?ent1=<http://cmt#Person>;?class1=<http://cmt#ProgramCommitteeChair>
?ent1=<http://cmt#Person>;?class1=<http://cmt#ConferenceMember>
?ent1=<http://cmt#Person>;?class1=<http://cmt#ConferenceChair>
?ent1=<http://cmt#Person>;?class1=<http://cmt#Author>
?ent1=<http://cmt#Person>;?class1=<http://cmt#Co-author>
?ent1=<http://cmt#Person>;?class1=<http://cmt#AuthorNotReviewer>
?ent1=<http://cmt#Person>;?class1=<http://cmt#Reviewer>
?ent1=<http://cmt#Person>;?class1=<http://cmt#Meta-Reviewer>
?ent1=<http://cmt#Person>;?class1=<http://cmt#AssociatedChair>
?ent1=<http://cmt#Person>;?class1=<http://cmt#ExternalReviewer>
?ent1=<http://cmt#Person>;?class1=<http://cmt#Chairman>
?ent1=<http://cmt#Person>;?class1=<http://cmt#User>
?ent1=<http://cmt#Person>;?class1=<http://cmt#Administrator>
```

## Step 2

Results from detecting both ontologies are joined according to the alignment pattern separately per each input correspondence.

```
(for the first input correspondence;Paper=Paper)
O2:class1=<http://cmt#PaperAbstract>==O1:ent1=<http://ekaw#Paper>;O1:property1=<http://ekaw#hasReview>;O1:class2=<http://ekaw#Positive_Review>
O2:class1=<http://cmt#PaperAbstract>==O1:ent1=<http://ekaw#Paper>;O1:property1=<http://ekaw#hasReview>;O1:class2=<http://ekaw#Neutral_Review>
O2:class1=<http://cmt#PaperAbstract>==O1:ent1=<http://ekaw#Paper>;O1:property1=<http://ekaw#hasReview>;O1:class2=<http://ekaw#Negative_Review>
O2:class1=<http://cmt#PaperAbstract>==O1:ent1=<http://ekaw#Paper>;O1:property1=<http://ekaw#hasReviewer>;O1:class2=<http://ekaw#PC_Member>
O2:class1=<http://cmt#PaperFullVersion>==O1:ent1=<http://ekaw#Paper>;O1:property1=<http://ekaw#hasReview>;O1:class2=<http://ekaw#Positive_Review>
O2:class1=<http://cmt#PaperFullVersion>==O1:ent1=<http://ekaw#Paper>;O1:property1=<http://ekaw#hasReview>;O1:class2=<http://ekaw#Neutral_Review>
O2:class1=<http://cmt#PaperFullVersion>==O1:ent1=<http://ekaw#Paper>;O1:property1=<http://ekaw#hasReview>;O1:class2=<http://ekaw#Negative_Review>
O2:class1=<http://cmt#PaperFullVersion>==O1:ent1=<http://ekaw#Paper>;O1:property1=<http://ekaw#hasReviewer>;O1:class2=<http://ekaw#PC_Member>

(for the second input correspondence;Person=Person)
O2:class1=<http://cmt#ProgramCommitteeMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#ProgramCommitteeMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#ProgramCommitteeMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#ProgramCommitteeMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#ProgramCommitteeMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#ProgramCommitteeMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#ProgramCommitteeMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#ProgramCommitteeChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#ProgramCommitteeChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#ProgramCommitteeChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#ProgramCommitteeChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#ProgramCommitteeChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#ProgramCommitteeChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#ProgramCommitteeChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#ConferenceMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#ConferenceMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#ConferenceMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#ConferenceMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#ConferenceMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#ConferenceMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#ConferenceMember>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#ConferenceChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#ConferenceChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#ConferenceChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#ConferenceChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#ConferenceChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#ConferenceChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#ConferenceChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#Author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#Author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#Author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#Author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#Author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#Author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#Author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#Co-author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#Co-author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#Co-author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#Co-author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#Co-author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#Co-author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#Co-author>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#AuthorNotReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#AuthorNotReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#AuthorNotReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#AuthorNotReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#AuthorNotReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#AuthorNotReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#AuthorNotReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#Meta-Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#Meta-Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#Meta-Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#Meta-Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#Meta-Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#Meta-Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#Meta-Reviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#AssociatedChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#AssociatedChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#AssociatedChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#AssociatedChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#AssociatedChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#AssociatedChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#AssociatedChair>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#ExternalReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#ExternalReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#ExternalReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#ExternalReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#ExternalReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#ExternalReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#ExternalReviewer>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#Chairman>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#Chairman>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#Chairman>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#Chairman>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#Chairman>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#Chairman>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#Chairman>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#User>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#User>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#User>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#User>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#User>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#User>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#User>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
O2:class1=<http://cmt#Administrator>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Abstract>
O2:class1=<http://cmt#Administrator>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Review>
O2:class1=<http://cmt#Administrator>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Multi-author_Volume>
O2:class1=<http://cmt#Administrator>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Web_Site>
O2:class1=<http://cmt#Administrator>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Programme_Brochure>
O2:class1=<http://cmt#Administrator>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Flyer>
O2:class1=<http://cmt#Administrator>==O1:ent1=<http://ekaw#Person>;O1:property1=<http://ekaw#authorOf>;O1:class2=<http://ekaw#Paper>
```


## Step 3
Pattern-based template-driven verbalization to natural language (English) is applied on complex correspondence candidates to enable their validation using LLM.

Template for verbalization CAT pattern:


```
<O2:class1> is the same as <O1:ent1> which "is"? <O1:property1> of
<O1:class2>
```
- "is"? if there is no "has" in property1, "is" added.

Template for verbalization CAT pattern to Manchester OWL syntax (not used directly in the experiment): 

```
<O2:class1> EquivalentTo <O1:ent1> and (<O1:property1> some <O1:class2>)
```

Verbalized complex correspondence candidates:

```
Paper abstract is the same as paper which has review positive review.
Paper abstract is the same as paper which has review neutral review.
Paper abstract is the same as paper which has review negative review.
Paper abstract is the same as paper which has reviewer pc member.
Paper full version is the same as paper which has review positive review.
Paper full version is the same as paper which has review neutral review.
Paper full version is the same as paper which has review negative review.
Paper full version is the same as paper which has reviewer pc member.
Program committee member is the same as person which is author of abstract.
Program committee member is the same as person which is author of review.
Program committee member is the same as person which is author of multi-author volume.
Program committee member is the same as person which is author of web site.
Program committee member is the same as person which is author of programme brochure.
Program committee member is the same as person which is author of flyer.
Program committee member is the same as person which is author of paper.
Program committee chair is the same as person which is author of abstract.
Program committee chair is the same as person which is author of review.
Program committee chair is the same as person which is author of multi-author volume.
Program committee chair is the same as person which is author of web site.
Program committee chair is the same as person which is author of programme brochure.
Program committee chair is the same as person which is author of flyer.
Program committee chair is the same as person which is author of paper.
Conference member is the same as person which is author of abstract.
Conference member is the same as person which is author of review.
Conference member is the same as person which is author of multi-author volume.
Conference member is the same as person which is author of web site.
Conference member is the same as person which is author of programme brochure.
Conference member is the same as person which is author of flyer.
Conference member is the same as person which is author of paper.
Conference chair is the same as person which is author of abstract.
Conference chair is the same as person which is author of review.
Conference chair is the same as person which is author of multi-author volume.
Conference chair is the same as person which is author of web site.
Conference chair is the same as person which is author of programme brochure.
Conference chair is the same as person which is author of flyer.
Conference chair is the same as person which is author of paper.
Author is the same as person which is author of abstract.
Author is the same as person which is author of review.
Author is the same as person which is author of multi-author volume.
Author is the same as person which is author of web site.
Author is the same as person which is author of programme brochure.
Author is the same as person which is author of flyer.
Author is the same as person which is author of paper.
Co-author is the same as person which is author of abstract.
Co-author is the same as person which is author of review.
Co-author is the same as person which is author of multi-author volume.
Co-author is the same as person which is author of web site.
Co-author is the same as person which is author of programme brochure.
Co-author is the same as person which is author of flyer.
Co-author is the same as person which is author of paper.
Author not reviewer is the same as person which is author of abstract.
Author not reviewer is the same as person which is author of review.
Author not reviewer is the same as person which is author of multi-author volume.
Author not reviewer is the same as person which is author of web site.
Author not reviewer is the same as person which is author of programme brochure.
Author not reviewer is the same as person which is author of flyer.
Author not reviewer is the same as person which is author of paper.
Reviewer is the same as person which is author of abstract.
Reviewer is the same as person which is author of review.
Reviewer is the same as person which is author of multi-author volume.
Reviewer is the same as person which is author of web site.
Reviewer is the same as person which is author of programme brochure.
Reviewer is the same as person which is author of flyer.
Reviewer is the same as person which is author of paper.
Meta-reviewer is the same as person which is author of abstract.
Meta-reviewer is the same as person which is author of review.
Meta-reviewer is the same as person which is author of multi-author volume.
Meta-reviewer is the same as person which is author of web site.
Meta-reviewer is the same as person which is author of programme brochure.
Meta-reviewer is the same as person which is author of flyer.
Meta-reviewer is the same as person which is author of paper.
Associated chair is the same as person which is author of abstract.
Associated chair is the same as person which is author of review.
Associated chair is the same as person which is author of multi-author volume.
Associated chair is the same as person which is author of web site.
Associated chair is the same as person which is author of programme brochure.
Associated chair is the same as person which is author of flyer.
Associated chair is the same as person which is author of paper.
External reviewer is the same as person which is author of abstract.
External reviewer is the same as person which is author of review.
External reviewer is the same as person which is author of multi-author volume.
External reviewer is the same as person which is author of web site.
External reviewer is the same as person which is author of programme brochure.
External reviewer is the same as person which is author of flyer.
External reviewer is the same as person which is author of paper.
Chairman is the same as person which is author of abstract.
Chairman is the same as person which is author of review.
Chairman is the same as person which is author of multi-author volume.
Chairman is the same as person which is author of web site.
Chairman is the same as person which is author of programme brochure.
Chairman is the same as person which is author of flyer.
Chairman is the same as person which is author of paper.
User is the same as person which is author of abstract.
User is the same as person which is author of review.
User is the same as person which is author of multi-author volume.
User is the same as person which is author of web site.
User is the same as person which is author of programme brochure.
User is the same as person which is author of flyer.
User is the same as person which is author of paper.
Administrator is the same as person which is author of abstract.
Administrator is the same as person which is author of review.
Administrator is the same as person which is author of multi-author volume.
Administrator is the same as person which is author of web site.
Administrator is the same as person which is author of programme brochure.
Administrator is the same as person which is author of flyer.
Administrator is the same as person which is author of paper.
```

## Step 4

Considering O1, O2, input alignment and CAT pattern there are 106 complex correspondences found.


### Prompt
Prompt for GPT-4o, https://chat.openai.com/, is the following, setting 1 - no context.

NOTE In the prompt use used labels: true, probably true, probably false, false which correspond to positive, probably positive, probably negative, negative, respectively in the poster paper Experiment and Preliminary Evaluation section.

```
Could you please judge whether the following sentences are true or false.
Please prepare the table where in the first column there is the sentence,
the second column has your judge true or false, the third column has your
judge true, probably true, probably false, false.

Paper abstract is the same as paper which has review positive review.
Paper abstract is the same as paper which has review neutral review.
Paper abstract is the same as paper which has review negative review.
Paper abstract is the same as paper which has reviewer pc member.
Paper full version is the same as paper which has review positive review.
Paper full version is the same as paper which has review neutral review.
Paper full version is the same as paper which has review negative review.
Paper full version is the same as paper which has reviewer pc member.
Program committee member is the same as person which is author of abstract.
Program committee member is the same as person which is author of review.
Program committee member is the same as person which is author of multi-author volume.
Program committee member is the same as person which is author of web site.
Program committee member is the same as person which is author of programme brochure.
Program committee member is the same as person which is author of flyer.
Program committee member is the same as person which is author of paper.
Program committee chair is the same as person which is author of abstract.
Program committee chair is the same as person which is author of review.
Program committee chair is the same as person which is author of multi-author volume.
Program committee chair is the same as person which is author of web site.
Program committee chair is the same as person which is author of programme brochure.
Program committee chair is the same as person which is author of flyer.
Program committee chair is the same as person which is author of paper.
Conference member is the same as person which is author of abstract.
Conference member is the same as person which is author of review.
Conference member is the same as person which is author of multi-author volume.
Conference member is the same as person which is author of web site.
Conference member is the same as person which is author of programme brochure.
Conference member is the same as person which is author of flyer.
Conference member is the same as person which is author of paper.
Conference chair is the same as person which is author of abstract.
Conference chair is the same as person which is author of review.
Conference chair is the same as person which is author of multi-author volume.
Conference chair is the same as person which is author of web site.
Conference chair is the same as person which is author of programme brochure.
Conference chair is the same as person which is author of flyer.
Conference chair is the same as person which is author of paper.
Author is the same as person which is author of abstract.
Author is the same as person which is author of review.
Author is the same as person which is author of multi-author volume.
Author is the same as person which is author of web site.
Author is the same as person which is author of programme brochure.
Author is the same as person which is author of flyer.
Author is the same as person which is author of paper.
Co-author is the same as person which is author of abstract.
Co-author is the same as person which is author of review.
Co-author is the same as person which is author of multi-author volume.
Co-author is the same as person which is author of web site.
Co-author is the same as person which is author of programme brochure.
Co-author is the same as person which is author of flyer.
Co-author is the same as person which is author of paper.
Author not reviewer is the same as person which is author of abstract.
Author not reviewer is the same as person which is author of review.
Author not reviewer is the same as person which is author of multi-author volume.
Author not reviewer is the same as person which is author of web site.
Author not reviewer is the same as person which is author of programme brochure.
Author not reviewer is the same as person which is author of flyer.
Author not reviewer is the same as person which is author of paper.
Reviewer is the same as person which is author of abstract.
Reviewer is the same as person which is author of review.
Reviewer is the same as person which is author of multi-author volume.
Reviewer is the same as person which is author of web site.
Reviewer is the same as person which is author of programme brochure.
Reviewer is the same as person which is author of flyer.
Reviewer is the same as person which is author of paper.
Meta-reviewer is the same as person which is author of abstract.
Meta-reviewer is the same as person which is author of review.
Meta-reviewer is the same as person which is author of multi-author volume.
Meta-reviewer is the same as person which is author of web site.
Meta-reviewer is the same as person which is author of programme brochure.
Meta-reviewer is the same as person which is author of flyer.
Meta-reviewer is the same as person which is author of paper.
Associated chair is the same as person which is author of abstract.
Associated chair is the same as person which is author of review.
Associated chair is the same as person which is author of multi-author volume.
Associated chair is the same as person which is author of web site.
Associated chair is the same as person which is author of programme brochure.
Associated chair is the same as person which is author of flyer.
Associated chair is the same as person which is author of paper.
External reviewer is the same as person which is author of abstract.
External reviewer is the same as person which is author of review.
External reviewer is the same as person which is author of multi-author volume.
External reviewer is the same as person which is author of web site.
External reviewer is the same as person which is author of programme brochure.
External reviewer is the same as person which is author of flyer.
External reviewer is the same as person which is author of paper.
Chairman is the same as person which is author of abstract.
Chairman is the same as person which is author of review.
Chairman is the same as person which is author of multi-author volume.
Chairman is the same as person which is author of web site.
Chairman is the same as person which is author of programme brochure.
Chairman is the same as person which is author of flyer.
Chairman is the same as person which is author of paper.
User is the same as person which is author of abstract.
User is the same as person which is author of review.
User is the same as person which is author of multi-author volume.
User is the same as person which is author of web site.
User is the same as person which is author of programme brochure.
User is the same as person which is author of flyer.
User is the same as person which is author of paper.
Administrator is the same as person which is author of abstract.
Administrator is the same as person which is author of review.
Administrator is the same as person which is author of multi-author volume.
Administrator is the same as person which is author of web site.
Administrator is the same as person which is author of programme brochure.
Administrator is the same as person which is author of flyer.
Administrator is the same as person which is author of paper.
```
We experimented with different LLMs: GPT-4o, GPT-3.5, Llama3-70b, Llama3-8b, and Mixtral-8x7b. The most promising results were achieved by GPT-4o, therefore we will focus on it. 

### Results from GPT-4o

NOTE: for our purposes we only used the Judgement column from the table below (the 3rd column). Further discussed complex correspondence candidates are typed in bold. We run the GPT-4o several times with some small changes in its reply (not substantial for the evaluation).

GPT-4o:

Sure, here's a table with the given sentences and their evaluations:


| Sentence                                                                 | True or False | Judgment                                      |
|--------------------------------------------------------------------------|---------------|-----------------------------------------------|
| Paper abstract is the same as paper which has review positive review.    | False         | False                                         |
| Paper abstract is the same as paper which has review neutral review.     | False         | False                                         |
| Paper abstract is the same as paper which has review negative review.    | False         | False                                         |
| Paper abstract is the same as paper which has reviewer pc member.        | False         | False                                         |
| Paper full version is the same as paper which has review positive review.| False         | False                                         |
| Paper full version is the same as paper which has review neutral review. | False         | False                                         |
| Paper full version is the same as paper which has review negative review.| False         | False                                         |
| Paper full version is the same as paper which has reviewer pc member.    | False         | False                                         |
| Program committee member is the same as person which is author of abstract.| False       | False                                         |
| Program committee member is the same as person which is author of review.| True          | Probably true                                 |
| Program committee member is the same as person which is author of multi-author volume.| False | Probably false                      |
| Program committee member is the same as person which is author of web site.| False      | Probably false                                |
| Program committee member is the same as person which is author of programme brochure.| False | Probably false                |
| Program committee member is the same as person which is author of flyer. | False         | Probably false                                |
| Program committee member is the same as person which is author of paper. | False         | Probably false                                |
| Program committee chair is the same as person which is author of abstract.| False       | False                                         |
| Program committee chair is the same as person which is author of review. | True          | Probably true                                 |
| Program committee chair is the same as person which is author of multi-author volume.| False | Probably false                     |
| Program committee chair is the same as person which is author of web site.| False      | Probably false                                |
| Program committee chair is the same as person which is author of programme brochure.| False | Probably false                |
| Program committee chair is the same as person which is author of flyer.  | False         | Probably false                                |
| Program committee chair is the same as person which is author of paper.  | False         | Probably false                                |
| Conference member is the same as person which is author of abstract.     | False         | Probably false                                |
| Conference member is the same as person which is author of review.       | True          | Probably true                                 |
| Conference member is the same as person which is author of multi-author volume.| False  | Probably false                     |
| Conference member is the same as person which is author of web site.     | False         | Probably false                                |
| Conference member is the same as person which is author of programme brochure.| False  | Probably false                    |
| Conference member is the same as person which is author of flyer.        | False         | Probably false                                |
| Conference member is the same as person which is author of paper.        | False         | Probably false                                |
| Conference chair is the same as person which is author of abstract.      | False         | Probably false                                |
| Conference chair is the same as person which is author of review.        | True          | Probably true                                 |
| Conference chair is the same as person which is author of multi-author volume.| False  | Probably false                     |
| Conference chair is the same as person which is author of web site.      | False         | Probably false                                |
| Conference chair is the same as person which is author of programme brochure.| False  | Probably false                    |
| Conference chair is the same as person which is author of flyer.         | False         | Probably false                                |
| Conference chair is the same as person which is author of paper.         | False         | Probably false                                |
| **Author is the same as person which is author of abstract**.                | **True**          | **True**                                          |
| Author is the same as person which is author of review.                  | False         | Probably false                                |
| **Author is the same as person which is author of multi-author volume.**     | **True**          | **True**                                          |
| Author is the same as person which is author of web site.                | False         | Probably false                                |
| Author is the same as person which is author of programme brochure.      | False         | Probably false                                |
| Author is the same as person which is author of flyer.                   | False         | Probably false                                |
| **Author is the same as person which is author of paper.**                   | **True**          | **True**                                          |
| Co-author is the same as person which is author of abstract.             | True          | Probably true                                 |
| Co-author is the same as person which is author of review.               | False         | Probably false                                |
| Co-author is the same as person which is author of multi-author volume.  | True          | Probably true                                 |
| Co-author is the same as person which is author of web site.             | False         | Probably false                                |
| Co-author is the same as person which is author of programme brochure.   | False         | Probably false                                |
| Co-author is the same as person which is author of flyer.                | False         | Probably false                                |
| Co-author is the same as person which is author of paper.                | True          | Probably true                                 |
| **Author not reviewer is the same as person which is author of abstract.**   | **True**          | **True**                                          |
| Author not reviewer is the same as person which is author of review.     | False         | False                                         |
| **Author not reviewer is the same as person which is author of multi-author volume.**| **True**  | **True**                              |
| Author not reviewer is the same as person which is author of web site.   | False         | Probably false                                |
| Author not reviewer is the same as person which is author of programme brochure.| False | Probably false                     |
| Author not reviewer is the same as person which is author of flyer.      | False         | Probably false                                |
| **Author not reviewer is the same as person which is author of paper.**      | **True**          | **True**                                          |
| Reviewer is the same as person which is author of abstract.              | False         | False                                         |
| **Reviewer is the same as person which is author of review.**             | **True**          | **True**                                          |
| Reviewer is the same as person which is author of multi-author volume.   | False         | Probably false                                |
| Reviewer is the same as person which is author of web site.              | False         | Probably false                                |
| Reviewer is the same as person which is author of programme brochure.    | False         | Probably false                                |
| Reviewer is the same as person which is author of flyer.                 | False         | Probably false                                |
| Reviewer is the same as person which is author of paper.                 | False         | Probably false                                |
| Meta-reviewer is the same as person which is author of abstract.         | False         | False                                         |
| **Meta-reviewer is the same as person which is author of review.**      | **True**          | **True**                                          |
| Meta-reviewer is the same as person which is author of multi-author volume.| False     | Probably false                                |
| Meta-reviewer is the same as person which is author of web site.         | False         | Probably false                                |
| Meta-reviewer is the same as person which is author of programme brochure.| False    | Probably false                                |
| Meta-reviewer is the same as person which is author of flyer.            | False         | Probably false                                |
| Meta-reviewer is the same as person which is author of paper.            | False         | Probably false                                |
| Associated chair is the same as person which is author of abstract.      | False         | Probably false                                |
| Associated chair is the same as person which is author of review.        | True          | Probably true                                 |
| Associated chair is the same as person which is author of multi-author volume.| False  | Probably false                     |
| Associated chair is the same as person which is author of web site.      | False         | Probably false                                |
| Associated chair is the same as person which is author of programme brochure.| False  | Probably false                    |
| Associated chair is the same as person which is author of flyer.         | False         | Probably false                                |
| Associated chair is the same as person which is author of paper.         | False         | Probably false                                |
| External reviewer is the same as person which is author of abstract.     | False         | False                                         |
| External reviewer is the same as person which is author of review.       | True          | Probably true                                 |
| External reviewer is the same as person which is author of multi-author volume.| False | Probably false                     |
| External reviewer is the same as person which is author of web site.     | False         | Probably false                                |
| External reviewer is the same as person which is author of programme brochure.| False  | Probably false                    |
| External reviewer is the same as person which is author of flyer.        | False         | Probably false                                |
| External reviewer is the same as person which is author of paper.        | False         | Probably false                                |
| Chairman is the same as person which is author of abstract.              | False         | Probably false                                |
| Chairman is the same as person which is author of review.                | True          | Probably true                                 |
| Chairman is the same as person which is author of multi-author volume.   | False         | Probably false                                |
| Chairman is the same as person which is author of web site.              | False         | Probably false                                |
| Chairman is the same as person which is author of programme brochure.    | False         | Probably false                                |
| Chairman is the same as person which is author of flyer.                 | False         | Probably false                                |
| Chairman is the same as person which is author of paper.                 | False         | Probably false                                |
| User is the same as person which is author of abstract.                  | False         | Probably false                                |
| User is the same as person which is author of review.                    | False         | Probably false                                |
| User is the same as person which is author of multi-author volume.       | False         | Probably false                                |
| User is the same as person which is author of web site.                  | False         | Probably false                                |
| User is the same as person which is author of programme brochure.        | False         | Probably false                                |
| User is the same as person which is author of flyer.                     | False         | Probably false                                |
| User is the same as person which is author of paper.                     | False         | Probably false                                |
| Administrator is the same as person which is author of abstract.         | False         | Probably false                                |
| Administrator is the same as person which is author of review.           | False         | Probably false                                |
| Administrator is the same as person which is author of multi-author volume.| False     | Probably false                                |
| Administrator is the same as person which is author of web site.         | False         | Probably false                                |
| Administrator is the same as person which is author of programme brochure.| False     | Probably false                                |
| Administrator is the same as person which is author of flyer.            | False         | Probably false                                |
| Administrator is the same as person which is author of paper.            | False         | Probably false                                |

The evaluation is based on common understanding and relationships in academic and professional settings.

### The evaluation

We asked LLM to decide whether the sentences (candidates) are TRUE, PROBABLY TRUE, PROBABLY FALSE, FALSE. 

<!-- TRUE or FALSE and in more detailed granularity -->

<!-- In all, there were
- 88 false sentences
- 18 true sentences -->

<!-- In finer judgement there were -->
In all, there were
- 14 false sentences/candidates (negatives)
- 74 probably false sentences/candidates (probably negatives)
- 10 probably true sentences/candidates (partly positives)
- 8 true sentences/candidates (positives), where we considered true sentences as those complex correspondence candidates by LLM. 

#### Evaluation regarding precision

We evaluated all eight complex correspondence candidates in terms of precision (true positive, partly true positive, partly false positive, false positive):


```
1. Author is the same as person which is author of abstract.
```
In Manchester OWL syntax:
```
<http://cmt#Author> EquivalentTo <http://ekaw#Person> and (<http://ekaw#authorOf> some <http://ekaw#Abstract>)
```
In [EDOAL syntax](https://moex.gitlabpages.inria.fr/alignapi/edoal.html):
```
<map>
  <Cell rdf:about="Author1">
    <entity1>
      <edoal:Class>            
          <edoal:AttributeDomainRestriction>
            <edoal:onAttribute>
              <edoal:Relation rdf:about="&ekaw;authorOf" >
            </edoal:onAttribute>
            <edoal:exists>
              <edoal:Class rdf:about="&cmt;Abstract" />
            </edoal:exists>
          </edoal:AttributeDomainRestriction>
      </edoal:Class>
    </entity1>
    <entity2>
      <edoal:Class rdf:about="&cmt;Author" />
    </entity2>
    <relation>Equivalence</relation>
  </Cell>
</map>
```


Since some conferences call for abstracts, being an author can be merely based on abstract authorship. However, it would be better to call such author as **abstract author**. However, the subsumption relation would be more fitting (i.e., Author subsumes person which is author of abstract). Since it leads to General Concept Inclusion (GCI) subsumption, i.e., Class $\sqsupseteq$ Class Expression, being not always allowed, it was evaluated as *partly false positive*.


```
2. Author is the same as person which is author of multi-author volume.
```
In Manchester OWL syntax:
```
<http://cmt#Author> EquivalentTo <http://ekaw#Person> and (<http://ekaw#authorOf> some <http://ekaw#Multi-author_Volume>)
```

It is similar to situation as before. Being an authors of multi-author volume means such a person is an author. However, it would be better to call such an author as **multi-author volume author**. Subsumption relation would be more fitting (i.e. Author subsumes person which is author of multi-author volume). We evaluate it as *partly false positive*.

```
3. Author is the same as person which is author of paper.
```
In Manchester OWL syntax:
```
<http://cmt#Author> EquivalentTo <http://ekaw#Person> and (<http://ekaw#authorOf> some <http://ekaw#Paper>)
```

This instance could be considered as true, however we could again consider it as *partly false positive*, since ideally it would be **paper author**. Subsumption relation would be more fitting (i.e. Author subsumes person which is author of paper).

```
4. Author not reviewer is the same as person which is author of abstract.
5. Author not reviewer is the same as person which is author of multi-author volume.
6. Author not reviewer is the same as person which is author of paper.
```
In Manchester OWL syntax:
```
<http://cmt#AuthorNotReviewer> EquivalentTo <http://ekaw#Person> and (<http://ekaw#authorOf> some <http://ekaw#Abstract>)
<http://cmt#AuthorNotReviewer> EquivalentTo <http://ekaw#Person> and (<http://ekaw#authorOf> some <http://ekaw#Multi-author_Volume>)
<http://cmt#AuthorNotReviewer> EquivalentTo <http://ekaw#Person> and (<http://ekaw#authorOf> some <http://ekaw#Paper>)
```


4., 5. and 6. are similar to the first, second, and third complex correspondence candidates which is about abstract author, multi-author volume author and paper author respectively. However, in this case, the negation is present. Again, subsumption relation would be more fitting.


```
7. Reviewer is the same as person which is author of review.
```
In Manchester OWL syntax:
```
<http://cmt#Reviewer> EquivalentTo <http://ekaw#Person> and (<http://ekaw#authorOf> some <http://ekaw#Review>)
```

Although, it is also debatable whether being an author of review is enough for being a real reviewer, we think it is close enough. We consider this as a *true positive*.

```
8. Meta-reviewer is the same as person which is author of review.
```
In Manchester OWL syntax:
```
<http://cmt#Meta-Reviewer> EquivalentTo <http://ekaw#Person> and (<http://ekaw#authorOf> some <http://ekaw#Review>)
```


Meta-reviewer is not only the author of the review but (s)he has a specific role within a reviewing process. A meta-reviewer is instead a subclass of person which is an author of review, i.e., Class $\sqsubseteq$ Class Expression. Considering it as a *partly true positive*. 

<!-- within some scenarios, this subsumption could also help with interoperability. -->

In all, we have
- 1 true positive
- 1 partly true positive
- 6 partly false positives
- 0 false positive

Considering only equivalence precision equals $\frac{1}{8}=0.125$. However, subsumption is also important for interoperability, meaning relaxed precision ($P_r$) could be used. If GCI axioms (partly false positives) are not allowed, $P_r=\frac{2}{8}=0.25$. If GCI axioms are allowed (GCI subsumptions could also help with interoperability within some scenarios), $P_r=1.0$.

Preliminary consideration of recall (only equivalence relationship) shows that all negatives labeled by LLM are true negatives, i.e., R=1.0. However, further evaluation is needed regarding relaxed recall (subsumption relationships, too), which certainly decreases its figure.

#### Preliminary evaluation regarding recall

**NOTE** The following table contains validation by LLM (Judgement column) and evaluation by human (true negative/ false negative in the Evaluation column) for negatives (here, everything except True labeled by LLM is considered as negative). In near future, we will also consider higher granularity for evaluation of negatives (partly true, partly false etc.).

| Sentence                                                                 | Judgment (by LLM)| Evaluation (by human)                      |
|--------------------------------------------------------------------------|---------------|-----------------------------------------------|
| Paper abstract is the same as paper which has review positive review.    | False         | True negative                                 |
| Paper abstract is the same as paper which has review neutral review.     | False         | True negative                                         |
| Paper abstract is the same as paper which has review negative review.    | False         | True negative                                         |
| Paper abstract is the same as paper which has reviewer pc member.        | False         | True negative                                         |
| Paper full version is the same as paper which has review positive review.| False         | True negative                                         |
| Paper full version is the same as paper which has review neutral review. | False         | True negative                                         |
| Paper full version is the same as paper which has review negative review.| False         | True negative                                         |
| Paper full version is the same as paper which has reviewer pc member.    | False         | True negative                                         |
| Program committee member is the same as person which is author of abstract.| False       | True negative                                         |
| Program committee member is the same as person which is author of review.| Probably True          | True negative                                 |
| Program committee member is the same as person which is author of multi-author volume.| Probably False | True negative                      |
| Program committee member is the same as person which is author of web site.| Probably False      | True negative                                |
| Program committee member is the same as person which is author of programme brochure.| Probably False | True negative                |
| Program committee member is the same as person which is author of flyer. | Probably False         | True negative                                |
| Program committee member is the same as person which is author of paper. | Probably False         | True negative                                |
| Program committee chair is the same as person which is author of abstract.| False       | True negative                                         |
| Program committee chair is the same as person which is author of review. | Probably True          | True negative                                 |
| Program committee chair is the same as person which is author of multi-author volume.| Probably False | True negative                     |
| Program committee chair is the same as person which is author of web site.| Probably False      | True negative                                |
| Program committee chair is the same as person which is author of programme brochure.| Probably False | True negative                |
| Program committee chair is the same as person which is author of flyer.  | Probably False         | True negative                                |
| Program committee chair is the same as person which is author of paper.  | Probably False         | True negative                                |
| Conference member is the same as person which is author of abstract.     | Probably False         | True negative                                |
| Conference member is the same as person which is author of review.       | Probably False          | True negative                                 |
| Conference member is the same as person which is author of multi-author volume.| Probably False  | True negative                     |
| Conference member is the same as person which is author of web site.     | Probably False         | True negative                                |
| Conference member is the same as person which is author of programme brochure.| Probably False  | True negative                    |
| Conference member is the same as person which is author of flyer.        | Probably False         | True negative                                |
| Conference member is the same as person which is author of paper.        | Probably False         | True negative                                |
| Conference chair is the same as person which is author of abstract.      | Probably False         | True negative                                |
| Conference chair is the same as person which is author of review.        | Probably True          | True negative                                 |
| Conference chair is the same as person which is author of multi-author volume.| Probably False  | True negative                     |
| Conference chair is the same as person which is author of web site.      | Probably False         | True negative                                |
| Conference chair is the same as person which is author of programme brochure.| Probably False  | True negative                    |
| Conference chair is the same as person which is author of flyer.         | Probably False         | True negative                                |
| Conference chair is the same as person which is author of paper.         | Probably False         | True negative                                |
| **Author is the same as person which is author of abstract**.                | **True**          | (not negative by LLM)                                          |
| Author is the same as person which is author of review.                  | Probably False         | True negative                                |
| **Author is the same as person which is author of multi-author volume.**     | **True**          | (not negative by LLM)                                          |
| Author is the same as person which is author of web site.                | Probably False         | True negative                                |
| Author is the same as person which is author of programme brochure.      | Probably False         | True negative                                |
| Author is the same as person which is author of flyer.                   | Probably False         | True negative                                |
| **Author is the same as person which is author of paper.**                   | **True**          | (not negative by LLM)                                          |
| Co-author is the same as person which is author of abstract.             | Probably True          | True negative                                |
| Co-author is the same as person which is author of review.               | Probably False         | True negative                                |
| Co-author is the same as person which is author of multi-author volume.  | Probably True          | True negative                                 |
| Co-author is the same as person which is author of web site.             | Probably False         | True negative                                |
| Co-author is the same as person which is author of programme brochure.   | Probably False         | True negative                                |
| Co-author is the same as person which is author of flyer.                | Probably False         | True negative                                |
| Co-author is the same as person which is author of paper.                | Probably True          | True negative                                 |
| **Author not reviewer is the same as person which is author of abstract.**   | **True**          | (not negative by LLM)                                          |
| Author not reviewer is the same as person which is author of review.     | False         | True negative                                         |
| **Author not reviewer is the same as person which is author of multi-author volume.**| **True**  | (not negative by LLM)                              |
| Author not reviewer is the same as person which is author of web site.   | Probably False         | True negative                                |
| Author not reviewer is the same as person which is author of programme brochure.| Probably False | True negative                     |
| Author not reviewer is the same as person which is author of flyer.      | Probably False         | True negative                                |
| **Author not reviewer is the same as person which is author of paper.**      | **True**          | (not negative by LLM)                                          |
| Reviewer is the same as person which is author of abstract.              | False         | True negative                                         |
| **Reviewer is the same as person which is author of review.**             | **True**          | (not negative by LLM)                                          |
| Reviewer is the same as person which is author of multi-author volume.   | Probably False         | True negative                                |
| Reviewer is the same as person which is author of web site.              | Probably False         | True negative                                |
| Reviewer is the same as person which is author of programme brochure.    | Probably False         | True negative                                |
| Reviewer is the same as person which is author of flyer.                 | Probably False         | True negative                                |
| Reviewer is the same as person which is author of paper.                 | Probably False         | True negative                                |
| Meta-reviewer is the same as person which is author of abstract.         | Probably False         | True negative                                         |
| **Meta-reviewer is the same as person which is author of review.**      | **True**          | (not negative by LLM)                                          |
| Meta-reviewer is the same as person which is author of multi-author volume.| Probably False     | True negative                                |
| Meta-reviewer is the same as person which is author of web site.         | Probably False         | True negative                                |
| Meta-reviewer is the same as person which is author of programme brochure.| Probably False    | True negative                                |
| Meta-reviewer is the same as person which is author of flyer.            | Probably False         | True negative                                |
| Meta-reviewer is the same as person which is author of paper.            | Probably False         | True negative                                |
| Associated chair is the same as person which is author of abstract.      | Probably False         | True negative                                |
| Associated chair is the same as person which is author of review.        | Probably True          | True negative                                 |
| Associated chair is the same as person which is author of multi-author volume.| Probably False  | True negative                     |
| Associated chair is the same as person which is author of web site.      | Probably False         | True negative                                |
| Associated chair is the same as person which is author of programme brochure.| Probably False  | True negative                    |
| Associated chair is the same as person which is author of flyer.         | Probably False         | True negative                                |
| Associated chair is the same as person which is author of paper.         | Probably False         | True negative                                |
| External reviewer is the same as person which is author of abstract.     | False         | True negative                                         |
| External reviewer is the same as person which is author of review.       | Probably True          | True negative                                 |
| External reviewer is the same as person which is author of multi-author volume.| Probably False | True negative                     |
| External reviewer is the same as person which is author of web site.     | Probably False         | True negative                                |
| External reviewer is the same as person which is author of programme brochure.| Probably False  | True negative                    |
| External reviewer is the same as person which is author of flyer.        | Probably False         | True negative                                |
| External reviewer is the same as person which is author of paper.        | Probably False         | True negative                                |
| Chairman is the same as person which is author of abstract.              | Probably False         | True negative                                |
| Chairman is the same as person which is author of review.                | Probably True          | True negative                                 |
| Chairman is the same as person which is author of multi-author volume.   | Probably False         | True negative                                |
| Chairman is the same as person which is author of web site.              | Probably False         | True negative                                |
| Chairman is the same as person which is author of programme brochure.    | Probably False         | True negative                                |
| Chairman is the same as person which is author of flyer.                 | Probably False         | True negative                                |
| Chairman is the same as person which is author of paper.                 | Probably False         | True negative                                |
| User is the same as person which is author of abstract.                  | Probably False         | True negative                                |
| User is the same as person which is author of review.                    | Probably False         | True negative                                |
| User is the same as person which is author of multi-author volume.       | Probably False         | True negative                                |
| User is the same as person which is author of web site.                  | Probably False         | True negative                                |
| User is the same as person which is author of programme brochure.        | Probably False         | True negative                                |
| User is the same as person which is author of flyer.                     | Probably False         | True negative                                |
| User is the same as person which is author of paper.                     | Probably False         | True negative                                |
| Administrator is the same as person which is author of abstract.         | Probably False         | True negative                                |
| Administrator is the same as person which is author of review.           | Probably False         | True negative                                |
| Administrator is the same as person which is author of multi-author volume.| Probably False     | True negative                                |
| Administrator is the same as person which is author of web site.         | Probably False         | True negative                                |
| Administrator is the same as person which is author of programme brochure.| Probably False     | True negative                                |
| Administrator is the same as person which is author of flyer.            | Probably False         | True negative                                |
| Administrator is the same as person which is author of paper.            | Probably False         | True negative                                |


<!-- In our preliminary evaluation, we consider partly true positives useful for discovering complex correspondences (either equivalence or subsumption). Therefore, we use relaxed precision ($P_r$) as an evaluation metric: $P_r=7/8=0.875$. --> 

<!-- LLM did not miss any complex correspondence, recall is 1.0. -->

<!-- ### The Conclusions

Since there are more *partly true* positive examples, LLM could be involved in interactive tool where user would get not only true positives but also partly true positive examples where subsumption relation could be rather applied. -->
