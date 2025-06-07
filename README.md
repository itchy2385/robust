# PhD project on "Robust user-based concept hierarchy generation"  (CODENAME: #robust)

robust/

├── data/                    # Raw and/or processed data

├── results/			     # Results, figures, tables

├── docs/				     # Documentation

└── misc/                    # Miscellaneous files

## Leuven Concept Data

Dutch norms for 129 animal names from 5 different categories and 166 artifact names from 6 categories. For each category there's an extensive set of exemplar judgments (e.g., typicality, age-of-acquisition, familiarity, etc.), pairwise similarities, and exemplar feature judgments. 

*De Deyne, S., Verheyen, S., Ameel, E., Vanpaemel, W., Dry, M., Voorspoels, W., Storms, G. (2008). Exemplar by feature applicability matrices and other Dutch normative data for semantic concepts. Behavior Research Methods, 40 (4), 1030-1048.*

The original data set is available here: https://www.dropbox.com/scl/fi/h71t5mgbctmew3cj3rvkj/BRM2008_LeuvenConceptData.zip?rlkey=icm5fn2z0mvq0qgbnmxceyh3y&e=1

The feature-by-exemplar matrix used was created in two steps with separate tasks. Step 1 comprised a feature generation task and Step 2 a feature applicability judgement task. While the objects of this matrix were in any case the exemplars of each category, their attributes were determined by a feature generation task performed by 1003 participants. For each semantic category, attributes with two levels of abstraction were generated, namely exemplar level and category level. To generate exemplar-level attributes, the participants in the study were presented with an exemplar term (e.g., “Pelikan”) as a stimulus and asked to name attributes based on it. At the category level, the stimulus consisted of a category term that contained all exemplars of the category as keywords (e.g., “birds (Pelikan,...)”). After preprocessing the named attributes, an ordered overall list was created that contained 156 to 382 attributes at the exemplar level and 21 to 39 at the category level.

On the basis of the list of attributes and objects, a feature applicability task was applied. At the exemplar level, incidence data were collected between object-attribute relations in a study with 51 participants. Each participant described the incidence data for one to three matrices, resulting in a total of four matrices for each semantic category. At the category level, four participants filled in the incidence for all semantic categories, which also resulted in four matrices per semantic category. Each cell in a given matrix has the structure of a usual formal context \( \mathbb{K} := (G,M,I) \) with unique incidence values \( I \), which contain either a 1 or a 0 and indicate the applicability of each attribute-object relation.

## Extended Leuven Concept Data

We extend the Leuven concept data by calculating first and second order attribute derivations as metrics of incidence-based attribute weighting. For this purpose, the Leuven concept data, originally based on several Excel files, was merged into a single JSON format.

You can find the file here: https://github.com/itchy2385/robust/blob/main/data/leuven-extended_master.json

Currently, the extended Leuven concept data only contains “Feature Listing, Frequency, and Importance” (PART2) “Exemplar by Feature Applicability Matrices” (PART3) and omits “Exemplar Judgments and Characteristics” (PART1). The selected data under `exemplar_feature_judgments` are first grouped by `category` or `area` (hereafter called `categories`, e.g. birds or clothing), which also includes the abstracted `categories` animal and artifacts.

Based on all fundamental parts of an object-attribute-matrix domain data grouped into `attributes`, `objects` and `incidences`. 

### Attributes

The attribute data under `attributes` is referenced by a unique key, e.g. `a_0_233`. Each attribute is expressed by an English label (e.g. `"label_en": "is sometimes kept as a pet"`) and a Dutch label (e.g. `"label_du": "wordt soms as huisdier gehouden"`). The abstraction level is represented by `grouping`, while the category level is expressed by `grouping": 2` and the exemplar level by `grouping": 1`. If an attribute was mentioned at both the category and specimen level, we assigned "grouping": [1, 2]`

Leuven Concept Data contains two measures of user-based attribute weighting, Feature Generation Frequencies and Feature Importance Ratings, which were described in PART 2 of the original paper by (DeDeyne, et. al. 2008). **Feature generation frequencies** can be determined when a feature generation task has been performed with multiple, usually many, study participants. A list of features created using a multi-participant feature generation task can be counted for the number of identical features between them. In general, the frequency of feature generation is a distribution measure that indicates how often an attribute is generated among participants. The more participants associate an attribute with a stimulus, the more important the attribute is. Each attribute contains the associated attribute generation frequency with the key "frequency". Although attributes can also be named at the category level, we represent the frequency of attribute generation with a list of key-value pairs (e.g. `"2": 4" means that the attribute was generated four times at the category level).

To apply the feature importance rating, the user is asked to rate how important or relevant they consider the attributes to be for the category on a 7-point Likert scale, ranging from -3 (for a very unimportant feature) to +3 (for a very important feature). Feature importance ratings are structured in the same direction as feature generation frequencies. Leuven Concept Data captures feature importance ratings by 12 participants for each domain and abstraction level. To allow a stringent data analysis we recoded the Likert scale from \( [-3, +3] \) to \( [1, 7] \) (e.g. `"importance" > "2" > "1": 1` is a feature importance rating on category level by participant 1 with an original Likert scale value of `-3`). We add arithmetic mean value over all feature importance ratings of an attribute having the key `0` (e.g. `"importance" > "2" > "0": 4.666666666666667`) 

**Please note!** The more abstract categories `animals` and `artifacts` include only feature generation frequencies but no feature importance ratings. For these attributes include an empty map of feature importance ratings (e.g. `"importance": {}`)

### Objects

Objects are structured in the same way as attributes does. Each object got a unique key indicating its domain and running number. It has a dutch and an english label and somekind of grouping values. Whereas the object set was present at category and exemplar level as well. For that reason has grouping 1 and 2.

### Incidences

Incidence data is represented compatible to a many valued context proposed by ConExp CLJ (https://github.com/tomhanika/conexp-clj/blob/master/doc/REST-API-usage.md#many-valued-context). Each incidence is counted having a value other than `0` whereas the object and attribute keys are used to position the incidence value (e.g. `["a_0_0", "o_0_3", 3]`). 
For each domain and each grouping exist four binary incidence matrizes and one summed many valued incidence matrix which summed up incidence data of the other four binary once.
