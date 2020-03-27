***
## Knowledge Graph Construction   
***
***

**Wiki Page:** **[`KG-Construction`](https://github.com/callahantiff/PheKnowLator/wiki/KG-Construction)**  

____

### Purpose
Describe the different parameters and arguments that can be passed when using PheKnowLator to build a knowledge graph. The different options include: [build type](#build-type), [construction approach](#construction-approach), [relation or edge directionality](#relationsedge-directionality), [node metadata](#node-metadata) use, and [decoding of owl semantics](#decoding-owl-semantics). Each of these parameters is explained below.  

<br>

_____


### Build Type   
The knowledge graph build algorithm has been designed to run from three different stages of development: `full`, `partial`, and `post-closure`.

Build Type | Description | Use Cases  
:--: | -- | --   
`full` | Runs all build steps in the algorithm | You want to build a knowledge graph and will not use a reasoner  
`partial` | Runs all of the build steps in the algorithm through adding the edges<br><br> If `node_data` is provided, it will not be added to the knowledge graph, but instead used to filter the edges such that only those edges with valid node metadata are added to the knowledge graph<br><br> Node metadata can always be added to a `partial` built knowledge graph by running the build as `post-closure` | You want to build a knowledge graph and plan to run a reasoner over it<br><br> You want to build a knowledge graph, but do not want to include node metadata, filter OWL semantics, or generate triple lists  
`post-closure` | Assumes that a reasoner was run over a knowledge graph and that the remaining build steps should be applied to a closed knowledge graph. The remaining build steps include determining whether OWL semantics should be filtered and creating and writing triple lists | You have run the `partial` build, ran a reasoner over it, and now want to complete the algorithm<br><br> You want to use the algorithm to process metadata and owl semantics for an externally built knowledge graph

<br> 

_____


### Construction Method   
New data can be added to the knowledge graph using 2 different construciton approaches: (1) `instance-based` or (2) `subclass-based`:  

**Instance-Based:** In this approach, each new edge is added as an `instance` of an existing class (via `rdf:Type`) in the knowledge graph.  
  
EXAMPLE: Adding the edge: Morphine ➞ `isSubstanceThatTreats` ➞ Migraine

Would require adding:
- `isSubstanceThatTreats`(Morphine, `x1`)
- `Type`(`x1`, Migraine)

While the instance of the class Migraines can be treated as an anonymous node in the knowledge graph, we generate a new international resource identifier for each asserted instance.

<br>

**Subclass-Based:**  In this approach, each new edge is added as a subclass of aan existing ontology class (via `rdfs:subClassOf`) in the knowledge graph.

EXAMPLE: Adding the edge: TGFB1 ➞ `participatesIn` ➞ Influenza Virus Induced Apoptosis

Would require adding:
- `participatesIn`(TGFB1, Influenza Virus Induced Apoptosis)
- `subClassOf`(Influenza Virus Induced Apoptosis, Influenza A pathway)   
- `Type`(Influenza Virus Induced Apoptosis, `owl:Class`)  

<br>

**REQUIREMENTS:**  

Method | Inputs 
:--: | ---   
Instance-Based | [`Master_Edge_List_Dict.json`](https://www.dropbox.com/s/4j0vrwx26dh8hd1/Master_Edge_List_Dict.json?dl=1) (Steps 1-3 of [KG Construction](https://github.com/callahantiff/PheKnowLator/wiki/KG-Construction))  
Subclass-Based | [`Master_Edge_List_Dict.json`](https://www.dropbox.com/s/4j0vrwx26dh8hd1/Master_Edge_List_Dict.json?dl=1) (Steps 1-3 of [KG Construction](https://github.com/callahantiff/PheKnowLator/wiki/KG-Construction))<br><br>[`sublass_construction_map.pkl`]()  

<br>

The `subclass_construction_map.pkl` file is a dictionary where keys are node or entity identifiers and values are lists of ontology class URIs to subclass, for example:  

```python
{
  'R-HSA-168277': ['http://purl.obolibrary.org/obo/PW_0001054',
                  'http://purl.obolibrary.org/obo/GO_0046730']
}                  

```

<br> 

_____

### Relations/Edge Directionality   
PheKnowLator can be built using a single set of provided relations (i.e. the `owl:ObjectProperty` or edge which is used to connect the nodes in the graph) with or without the inclusion of each relation's inverse. Please see [this](https://github.com/callahantiff/PheKnowLator/blob/master/resources/relations_data/README.md) README for additional information.  

<br> 

_____


### Node Metadata
The knowledge graph can be built with or without the inclusion of instance node metadata (i.e. labels, descriptions or definitions, and, synonyms). Please see [this](https://github.com/callahantiff/PheKnowLator/blob/master/resources/node_data/README.md) README for additional information. 

<br> 

_____


### Decoding OWL Semantics  
The knowledge graph can be built with or without the inclusion of edges that contain OWL Semantics. Please see [this](https://github.com/callahantiff/PheKnowLator/wiki/OWL-NETS-2.0) Wiki page for additional information. 

<br>

🛑 *<b>ASSUMPTIONS</b>* 🛑  
**The algorithm makes the following assumptions:**
- Edge list data has been created (see [here](https://github.com/callahantiff/PheKnowLator/blob/master/resources/edge_data) for additional information)  
- Ontologies have been preprocessed (see [here](https://github.com/callahantiff/PheKnowLator/blob/master/resources/ontologies/README.md) for additional information)  
- Decisions made and required input documentation provided for each of the parameters described above.     
