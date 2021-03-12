
# Identifier Naming Structure Catalogue
This README will catalogue common source code identifier naming structures derived from research. These will be represented as grammar patterns [1], but for simplicity sake we will refer to them as naming structures. We first describe needed [Linguistic Terminology](#linguistic-terminology) and a [Tagset](#tagset) before presenting the [Naming Structures](Common-naming-patterns-and-their-definition) themselves. If you're already familiar with these, skip down to the structures themselves!

# Linguistic Terminology
First you should be familiar with two simple linguistic concepts.
| Linguistic term |  Definition|
|--|--|
|Noun-adjunct   | Noun-adjuncts are compound-words; typically groups of two-or-more words separated by a dash. Compound-words start with a noun-adjunct; a noun acting as an adjective. For example, in the word employee-name, 'employee' is a noun-adjunct and 'name' is a noun (or, more specifically, a head-noun).  |
|Head-noun|The right-most noun in a noun phrase is typically referred to as a head-noun. This noun is the word that most-closely emobodies the concept that represents the in-memory entity that the identifier is used to describe.

# Tagset
The tagset that we used is a subset of Penn treebank. Each of our annotations and an example can be found below. Further examples and definitions can be found in the paper [1]

| Abbreviation | Expanded Form                           | Examples                                                        |
|--------------|-----------------------------------------|-----------------------------------------------------------------|
| N            | noun                                    | Disneyland, shoe, faucet, mother, bedroom                       |
| DT           | determiner                              | the, this, that, these, those, which                            |
| CJ           | conjunction                             | and, for, nor, but, or, yet, so                                 |
| P            | preposition                             | behind, in front of, at, under, beside, above, beneath, despite |
| NPL          | noun plural                             | Streets, cities, cars, people, lists, items, elements.          |
| NM           | noun modifier (adjective, noun-adjunct) | red, cold, hot, scary, beautiful, happy, faster, small          |
| V            | verb                                    | Run, jump, drive, spin                                          |
| VM           | verb modifier (adverb)                  | Very, loudly, seriously,impatiently, badly                      |
| PR           | pronoun                                 | she, he, her, him, it, we, us, they, them, I, me, you           |
| D            | digit                                   | 1, 2, 10, 4.12, 0xAF                                            |
| PRE          | preamble (e.g., Hungarian)              | Gimp, GLEW, GL, G, p_, m_, b_                                   |

# Common naming patterns and their definition
The grammar patterns below represent different naming structures found in source code; they are represented by sequences of part-of-speech tags. The patterns we present were all empirically derived by looking at a sample of 1,335 identifiers and manually annotating them with part-of-speech. Refer to our paper for more information [1]. In addition, feel free to check out the dataset of 1,335 manually-tagged identifiers associated with the paper -- https://github.com/SCANL/datasets/tree/master/grammar_patterns_data.

We present each pattern, a definition for the pattern, and examples of the pattern below. We will expand this as we discover more patterns through our research, so be sure to check be periodically!

| Grammar_Pattern | Definition | Examples |
|-|-|-|
| NM* N | Noun Phrase: One or more noun modifiers that specialize as single head-noun. The noun modifiers before the head-noun act as a way to specialize our understanding of the head-noun. The head-noun embodies the in-memory concept represented by the identifier. | label_action<br>table_index<br>read_operation |
| V NM* N | Verb Phrase: Verb followed by a noun-phrase. These are typically either function identifiers or identifiers with a boolean type. When this represents a function name, which is what it most commonly represents, the verb is an action that is applied to the object specified by the noun-phrase that follows it. When this represents a boolean identifier, the verb is typically an existential quantifier; checking for the existence of the object specified by the noun phrase that follows the verb. | create_metadata_array<br>trim_parse_trees<br>write_tcp_data |
| NM* NPL | Plural noun phrase: Same as noun phrase but the plural (NPL) is sometimes purposeful; semi-frequently referring to collection identifiers. That is, identifiers with a plural head-noun are somewhat more likely to have a collection type. Some naming conventions generally consider it good practice to match the plurality of the identifier with whether its type represents a singular, or collection, object. | training_examples<br>client_certificates<br>curves |
| P NM* N | Prepositonal phrase pattern: The preposition is usually gives us an understand of how the noun phrase is related to an un-specified phrase that, in normal English, would appear before the preposition. This relationship is usually meant to provide the developer with some context related to when, or how, the noun phrase is relevant in the context of the un-specified phrase that should appear before it. | on_keypress<br>with_market_rate_accuracy<br>to_string |
| NM* N P NM* N | Explicit prepositional phrase with leading noun phrase: Same as the prepositional phrase pattern except the noun phrase before the preposition IS specified | generated_token_on_creation<br>query_timeout_in_milliseconds<br>scroll_id_for_node<br>url_from_json<br>int_to_string |
| V P NM* N | Explicit prepositional phrase with leading verb: Same as prepositional phrase pattern but there is a leading verb. These are similar to regular verb phrases with an action being applied to an object. However, in this case, the preposition helps us understand how the action should, or will, be applied relative to the object. e.g., WITH the object, TO (typically conversion) the object, ON the object (when an event occurs, or physically in a non-local space), etc. | destroy_with_parent<br>convert_to_php_namespace<br>tesselate_to_mesh<br>execute_on_host |
| V* DT NM* NPL | Plural noun phrase with leading determiner: A determiner followed by a plural noun phrase. The determiner tells us the population, which is related to the noun phrase, that is represented by the identifier. E.g., ALL, ANY, ONE, SEVERAL of the objects specified by the plural noun phrase. If there is a leading verb, the verb specifies an action to take on the population or existential quantification. | all_invocation_matchers<br>all_open_indices<br>is_a_empty<br>matches_any_parent_category |
| V+ | Verb pattern: One or more verbs with no noun phrase. These are typically functions that perform some generic functionality, such as sort. While the noun phrase that this action is applied to is not present in the identifier name, it is typically present either as an argument to the corresponding function or as the calling object to a method. In some cases, these are boolean-type variables that may be missing an existential quantifier (e.g., 'is', as in is_parsing) | sort<br>delete<br>resume<br>parsing |


# Please cite the paper!

1. Christian D. Newman, Reem S. AlSuhaibani, Michael J. Decker, Anthony Peruma, Dishant Kaushik, Mohamed Wiem Mkaouer, Emily Hill,
On the generation, structure, and semantics of grammar patterns in source code identifiers, Journal of Systems and Software, 2020, 110740, ISSN 0164-1212, https://doi.org/10.1016/j.jss.2020.110740. (http://www.sciencedirect.com/science/article/pii/S0164121220301680) 

Keywords: Program comprehension; Identifier naming; Software maintenance; Source code analysis; Part-of-speech tagging
