# LLM prompts
Repository for listing prompts for large language models

# Example
``````
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


###
Inputs:
Complete and exhaustive list of entity types to extract from the text:
- organization: A legal person that can be a company, a holding,  a fund, a NGO, an University, .
- person: A physical person
- product: Commercial product produced by an organization


Text:
"""
Interplay secures $45M for third fund focused on B2B marketplaces, vertical software Christine Hall@christinemhall / 3:02 PM GMT+1•November 13, 2023  Comment Isometric businessman selling ideas to client standing near suitcase full of banknotes, businessman trying to convince client to buy his creative light bulb, creativity and marketing. Image Credits: Getty Images Interplay, a New York-based venture capital firm, closed on its third fund with $45 million in capital commitments.  The firm, focused on investments at the Series A stage, deploys capital into software, including B2B marketplace and vertical software. The new fund is a continuation of the firm’s prior two early-stage funds. We previously covered Interplay LLC in 2022 when it was raising a separate vehicle.  Within the marketplace industry, Interplay sees companies digitizing areas that had not yet entered that stage simply because the economics previously did not make sense, Mark Peter Davis, Interplay founder and managing partner, told TechCrunch.  “The overarching trend is we see a major shift towards specialization, happening particularly over the last decade. New companies are increasingly competing with legacy horizontal platforms by providing specialization to be more tailored and specific to particular industries,” Davis said. “We’ve had a lot of success investing around this shift, and that’s the underlying thesis of this vintage.”  For its first fund, Interplay had what Davis called “an angel vintage,” before securing outside limited partners for the second fund. What’s different with Fund III is that it’s institutional funds, with backing from fund of funds, family offices and founders within Interplay’s portfolio.  Davis called out a few ways that Interplay stands out: The first is that limited partners and founders get the same team of general partners making investment decisions every fund vintage, including Davis, Kevin Tung and Mike Rogers, who have been investing together for over eight years. The firm’s investment committee has now made over 50 deals. The second is its outsized value proposition relative to its check size. And the third is the studio which enables Interplay to incubate and start companies. To Davis, this expands its reach and creates deal flow.   Interplay, venture capital Image Credits: Interplay   The new fund gives Interplay $150 million in assets under management. Davis expects to invest in 20 companies with the fund, injecting $1 million to $2 million per check, with some held back for follow-on deals. The firm has already deployed 40% of the fund so far. It most recently invested in two construction tech companies, OnSiteIQ and Roofr.  Meanwhile, fundraising itself, for both companies and VC firms, was tested this year.  “It was a hard market to raise in,” Davis said. “We’re really proud of the outcome we had despite the market, and I would say it’s a testament to the extremely hard work that the team has been putting into now for a decade-plus.”  Getting to a Series A was challenging as of late, and Davis agrees that this particular stage is affected by the market shift. However, Interplay sees a number of good companies with improving key performance indicators that are raising money “at reasonable valuations.”  Higher valuations during the investment boom continues to keep Interplay disciplined about where it deploys its capital. Davis noted that the firm “ended up passing on a lot of opportunities that reflected over-excitement in the market.”  “This is a very attractive market to be investing into because company valuations have come back down to what we think is reasonable,” Davis said. “In some cases, they’ve overcorrected. That’s also going to have its own problems for entrepreneurs, and we don’t like that, but when they’re healthy and they’re fair, we think they set the company up for continued success.
"""

Output (JSON where you must use the higlighting characters "__"):
``````

## Answer with GPT4 (temp = 0)
```JSON
{
 "organization":
  [
   {
     "entity_name": "Interplay",
     "excerpts": [
       "__Interplay__, a New York-based venture capital firm, closed on its third fund with $45 million in capital commitments.",
       "We previously covered __Interplay LLC__ in 2022 when it was raising a separate vehicle.",
       "Davis called out a few ways that __Interplay__ stands out",
       "The new fund gives __Interplay__ $150 million in assets under management."
     ]
   },
   {
     "entity_name": "OnSiteIQ",
     "excerpts": [
       "It most recently invested in two construction tech companies, __OnSiteIQ__ and Roofr."
     ]
   },
   {
     "entity_name": "Roofr",
     "excerpts": [
       "It most recently invested in two construction tech companies, OnSiteIQ and __Roofr__."
     ]
   }
  ],
 "person":
  [
   {
     "entity_name": "Mark Peter Davis",
     "excerpts": [
       "Mark Peter __Davis__, Interplay founder and managing partner, told TechCrunch.",
       "For its first fund, Interplay had what __Davis__ called “an angel vintage,”",
       "“It was a hard market to raise in,” __Davis__ said.",
       "“This is a very attractive market to be investing into because company valuations have come back down to what we think is reasonable,” __Davis__ said."
     ]
   },
   {
     "entity_name": "Kevin Tung",
     "excerpts": [
       "The first is that limited partners and founders get the same team of general partners making investment decisions every fund vintage, including Davis, __Kevin Tung__ and Mike Rogers"
     ]
   },
   {
     "entity_name": "Mike Rogers",
     "excerpts": [
       "The first is that limited partners and founders get the same team of general partners making investment decisions every fund vintage, including Davis, Kevin Tung and __Mike Rogers__"
     ]
   }
  ],
 "product":
  [
   {
     "entity_name": "B2B marketplace and vertical software",
     "excerpts": [
       "The firm, focused on investments at the Series A stage, deploys capital into software, including __B2B marketplace and vertical software__."
     ]
   }
  ]
}
```

## Answer with GPT3.5 (temp = 0)
```JSON
{
 "organization":
  [
   {
     "entity_name": "Interplay",
     "excerpts": [
       "Interplay secures $45M for third fund focused on B2B marketplaces, vertical software",
       "We previously covered __Interplay LLC__ in 2022 when it was raising a separate vehicle.",
       "Within the marketplace industry, __Interplay__ sees companies digitizing areas that had not yet entered that stage simply because the economics previously did not make sense, Mark Peter Davis, __Interplay__ founder and managing partner, told TechCrunch.",
       "What’s different with Fund III is that it’s institutional funds, with backing from fund of funds, family offices and founders within __Interplay__’s portfolio.",
       "The first is that limited partners and founders get the same team of general partners making investment decisions every fund vintage, including Davis, Kevin Tung and Mike Rogers, who have been investing together for over eight years.",
       "The firm’s investment committee has now made over 50 deals.",
       "The second is its outsized value proposition relative to its check size.",
       "And the third is the studio which enables __Interplay__ to incubate and start companies.",
       "The new fund gives __Interplay__ $150 million in assets under management.",
       "The firm has already deployed 40% of the fund so far.",
       "Meanwhile, fundraising itself, for both companies and VC firms, was tested this year.",
       "“It was a hard market to raise in,” Davis said.",
       "“We’re really proud of the outcome we had despite the market, and I would say it’s a testament to the extremely hard work that the team has been putting into now for a decade-plus.”",
       "However, __Interplay__ sees a number of good companies with improving key performance indicators that are raising money “at reasonable valuations.”",
       "Higher valuations during the investment boom continues to keep __Interplay__ disciplined about where it deploys its capital.",
       "Davis noted that the firm “ended up passing on a lot of opportunities that reflected over-excitement in the market.”",
       "“This is a very attractive market to be investing into because company valuations have come back down to what we think is reasonable,” Davis said.",
       "“In some cases, they’ve overcorrected. That’s also going to have its own problems for entrepreneurs, and we don’t like that, but when they’re healthy and they’re fair, we think they set the company up for continued success."
     ]
   }
 ],
 "person": [],
 "product": []
}
```
