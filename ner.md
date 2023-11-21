You are an expert nlp data labeler in named entity recognition, your task is to manually recognize and extract specific types of entities from a text document and identify all entities them in the sentences where they are mentionned.

As you are an expert you must carefully think about the correctness of the labeling of identified entities and discard them is they are not part of the list of entity type of interest.
Also you must label all entities of the requested types present in the document, even if they look to be less important.
Note that each entity can be refered to with different lexical surface form, or referred to indirectly (implicitely). It must not prevent you from understanding that they are refering to the same unique entity.

The input are:
1) The complete list of entity types to label. Not necessarily all entity types can be found in the document.
2) The text document to label, delimited by triple quotes.

The output is a JSON object with the following field for each nested documents:
- 'entity_type', a string corresponding to one of entity types of interest. Containing array of nested objects:
  - 'entity_name', the general, most informative, name of the entity
  - 'excerpts', an array (list) of string of all excerpt (passage) where the  entity is mentionned. Each excerpt should be relatively short, around 10 words The entity must be highlighted in the excerpt text by adding "__" characters around it.

You should follow this abstract example:
```JSON
{
 "entity_1_type":
  [
   {
     "entity_name": "entity_1_name",
     "excerpts": [
       "some context __entity_1 surface_form_1__for the first mention",
       "some other __entity_1 different surface_form_2__ for the second mention",
       "some implicit reference __entity_1 coreference noun phrase__ for the second mention"
     ]
   },
  {
     "entity_name": "entity_2_name",
     "excerpts": [
       "a different context __entity_2 surface_form_1__ for the first mention",
       "some other __entity_2 surface_form_2 with postfix__ for the second mention",
       "some other __entity_2 anaphora_coreference_surface_form__ for the second mention"
     ]
  },
   ...
 ],
 "entity_2_type":
  [
  ...
  ]
}
```

Inputs:
Complete and exhaustive list of entity types to extract from the text:
YOUR LIST OF ENTITY TYPES


Text:
"""
YOUR TEXT
"""

Output (JSON where you must use the higlighting characters "__"):
