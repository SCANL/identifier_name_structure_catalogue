
# Identifier Naming Structure Catalogue
This README will catalogue common source code identifier naming structures derived from research. These will be represented as grammar patterns [1], but for simplicity sake we will refer to them as naming structures. The goal of this document is to act as a resource for researchers, students, and developers seeking more information on identifier naming structures, their meanings, and common usage patterns. We are currently looking into other types of identifier charactheristics that should be included in this document. **This is a living document**, we will expand this as we discover more patterns and characteristics through our, and possibly others', research. Check back periodically for more information!

We first describe needed [Linguistic Terminology](#linguistic-terminology) and a [Tagset](#tagset) before presenting the [Naming Structures](#Common-naming-patterns-and-their-definition) themselves. If you're already familiar with these, skip down to the structures themselves!

# Linguistic Terminology
First you should be familiar with some simple linguistic concepts.
| Linguistic-terminology | Definition                                                                                                                                                                                                                                                                                            |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Noun-adjunct           | Noun-adjuncts are defined as a noun acting (i.e., being used as) as an adjective. These are found in certain types of compound-words which, in English, are often groups of two-or-more words separated by a dash. For example, in the word employee-name, 'employee' is a noun-adjunct and 'name' is a noun (or, more specifically, a head-noun). |
| Head-noun              | The right-most noun in a noun phrase is typically referred to as a head-noun. This noun is the word that most-closely emobodies the concept that represents the in-memory entity that the identifier is used to describe.                                                                             |
| Hypernym               | A word with a broad meaning that more specific words fall under; a superordinate. For example, color is a hypernym of red. [Definition from Oxford Languages](https://languages.oup.com/google-dictionary-en/)                                                                                                                                                                         |
| Hyponym                | a word of more specific meaning than a general or superordinate term applicable to it. For example, spoon is a hyponym of cutlery. [Definition from Oxford Languages](https://languages.oup.com/google-dictionary-en/)                                                                         |

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
The grammar patterns below represent different naming structures found in source code; they are represented by sequences of part-of-speech tags. The patterns we present were all empirically derived by looking at a sample of 1,335 identifiers and manually annotating them with part-of-speech. Refer to the paper for more information [1]. In addition, feel free to check out the dataset of 1,335 manually-tagged identifiers associated with the paper -- https://github.com/SCANL/datasets/tree/master/grammar_patterns_data.

We present each pattern, a definition for the pattern, and examples of the pattern below. The \* symbol means "zero or more" while the + symbol means "one or more" of the token to the corresponding symbol's left. Just like in regular expressions. 

<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Grammar_Pattern_sequences</th>
    <th style="text-align:center;font-weight:bold" class="tg-0pky">Definition</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">NM* N</td>
    <td class="tg-0pky">Noun Phrase: One or more noun-modifiers that appear (typically) to the left of a head-noun. The noun-modifiers before the head-noun act as a way to specialize our understanding of the head-noun; taking the general concept that the head-noun represents and reducing it to a more concise, specific concept. For example, in the identifier 'issueDescription' the head-noun is 'Description', which is the general concept. The noun-adjunct, 'issue', specializes our understanding of the description by specifying what kind of 'description' we are talking about; the 'description' for some 'issue'. <br><br>This is the most common naming pattern for non-function identifiers. It is good practice to be careful in the choice, and number, of noun-modifiers to use before the head-noun. A good identifier will include only enough noun-modifiers to concisely define the concept represented by the head-noun.<br><br> These are typically non-function identifier names.<br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <tr><td><pre lang="javascript"> GtkWidget *selection_width_label; </pre></td><td style="text-align:center">NM NM N</td></tr>
       <tr><td><pre lang="C++"> int dynamic_Table_Index;</pre></td><td style="text-align:center">NM NM N</td></tr>
       <tr><td><pre lang="javascript"> ReadBufferOperation *read_Operation;</pre></td><td style="text-align:center">NM N</td></tr>
      </table>
    </td>
  </tr>
    <tr>
    <td class="tg-0pky">NM* NPL</td>
    <td class="tg-0pky">Plural noun phrase: When the head-noun of a noun-phrase is plural, it is a plural noun-phrase. The plural is sometimes purposeful in that the plural head-noun expresses the multiplicity of the data that the identifier represents.<br><br>
Identifiers with a plural head-noun are somewhat more likely to have a collection type [1]. Some naming conventions (e.g., the Java naming standard) generally consider it good practice to match the plurality of the identifier with whether its type represents a singular, or collection, object.<br><br> These are typically non-function identifier names. <br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <tr><td><pre lang="C++"> int training_examples; </pre></td><td style="text-align:center">NM NPL</td></tr>
       <tr><td><pre lang="java"> String[] method_Name_Prefixes;</pre></td><td style="text-align:center">NM NM NPL</td></tr>
       <tr><td><pre lang="java"> vector&lt;Handle&lt;AbcdAtmVolCurve&gt;&gt; curves;</pre></td><td style="text-align:center">NPL</td></tr>
      </table>
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">V NM* N(PL)</td>
    <td class="tg-0pky">Verb Phrase: The addition of a verb to a noun phrase creates a verb phrase. A verb phrase is typically active in that verb is an action being applied to, or with, the concept embodied by the noun phrase that follows it. In some cases, instead of being an action, the verb is an existential quantifier.<br><br>Identifiers with this pattern are typically either function identifiers or identifiers with a boolean type.<br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <tr><td><pre lang="C++"> bool create_metadata_array(); </pre></td><td style="text-align:center">V NM N</td></tr>
       <tr><td><pre lang="C++"> bool is_First_frame;</pre></td><td style="text-align:center">V NM N</td></tr>
       <tr><td><pre lang="C++"> int create_Duplicate_Change_Id();</pre></td><td style="text-align:center">V NM NM N</td></tr>
      </table>
    </td>
  </tr>

  <tr>
    <td class="tg-0pky">P NM* N(PL)</td>
    <td class="tg-0pky">Prepositonal phrase pattern: A noun-phrase with a leading preposition is a prepositional phrase. The preposition in a prepositional phrase typically explains how the entity (or entities) represented by the accompanying noun-phrase are related in terms of order, space, time (e.g., on_connect), ownership, causality, or representation (e.g., to_string). In the case of this specific grammar pattern, there is oftentimes an un-specified verb on the left-hand-side of the preposition. <br><br>The un-specified verb is usually an action such as the following: GET, CONVERT (e.g., to string), EXECUTE (e.g., on connect) or some other action. The action is implied; developers understand the action because of experience or domain knowledge, such as understanding that event-driven functions often begin with the preposition 'on'. There may also be noun-phrase to the left of the preposition. We discuss these in another grammar pattern below.<br><br> These can be functions or non-function identifier names.<br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <tr><td><pre lang="C++"> void On_Connect(); </pre></td><td style="text-align:center">P N</td></tr>
       <tr><td><pre lang="C++"> ModelSettings&amp; with_Market_Rate_Accuracy(); </pre></td><td style="text-align:center">P NM NM N</td></tr>
       <tr><td><pre lang="java"> btVector3 from_Local_Aabb_Min;</pre></td><td style="text-align:center">P NM NM N</td></tr>
       <tr><td><pre lang="C++"> String to_string();</pre></td><td style="text-align:center">P N</td></tr>
      </table>
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">NM* N P NM* N(PL)</td>
    <td class="tg-0pky">Prepositional phrase with leading noun phrase: Sometimes a noun phrase is explicitly present on both the left and right of the preposition. When the left-hand-side noun-phrase is specified, there is an explicit relationship between the left- and right-hand side noun-phrases. This relationship is expressed through the preposition. The preposition helps us understand how the entity (or entities) represented by both noun-phrases are related in terms of order, space, time (e.g., generated_token_on_creation), ownership (e.g., scroll_id_for_node), causality, or representation (e.g., url_from_json, query_timeout_in_milliseconds).<br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <tr><td><pre lang="javascript"> String generated_Token_On_Creation; </pre></td><td style="text-align:center">V N P N</td></tr>
       <tr><td><pre lang="C++"> long query_Timeout_In_Milliseconds; </pre></td><td style="text-align:center">NM N P NPL</td></tr>
       <tr><td><pre lang="java"> class Scroll_Id_For_Node;</pre></td><td style="text-align:center">NM N P N</td></tr>
       <tr><td><pre lang="C++"> HttpUrl url_from_json();</pre></td><td style="text-align:center">N P N</td></tr>
      </table>
    </td>
    
  </tr>
  <tr>
    <td class="tg-0pky">V P NM* N(PL)</td>
    <td class="tg-0pky">Prepositional phrase with leading verb: Same as prepositional phrase pattern but the leading verb, or verb phrase, is specified. As usual, the preposition helps us understand how the entity (or entities) represented by the verb- and noun-phrases are related in terms of order, space, time, ownership, causality (e.g., destroy_with_parent), or representation (e.g., save_as_quadratic_png, tessellate_to_mesh, convert_to_php_namespace).<br><br> The usage of this pattern is similar to when the verb is implicit. There may still be an implicit noun phrase to the right of the verb and to the left of the preposition.<br><br> These can be functions or non-function identifier names.<br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <tr><td><pre lang="C++"> gboolean destroy_with_parent; </pre></td><td style="text-align:center">V P N</td></tr>
       <tr><td><pre lang="C++"> string convert_to_php_namespace(); </pre></td><td style="text-align:center">V P NM N</td></tr>
       <tr><td><pre lang="C++"> void tessellate_To_Mesh();</pre></td><td style="text-align:center">V P N</td></tr>
       <tr><td><pre lang="C++"> void save_As_Quadratic_Png();</pre></td><td style="text-align:center">V P NM N</td></tr>
      </table>
  </tr>
  <tr>
    <td class="tg-0pky">V* DT NM* N(PL)</td>
    <td class="tg-0pky">Noun phrase with leading determiner: The addition of a determiner tells us how much of the population, which is specified by the noun-phrase, is represented, or acted on, by the identifier. <br><br>Typically, the determiner will tell us that we are interested in ALL, ANY, ONE, A, THE, SEVERAL, etc., of the population of objects specified by the noun phrase. If there is a leading verb, the verb specifies an action to take on the population or it represents existential quantification (e.g., matchesAnyParentCategories)<br><br>These can be functions or non-function identifier names.<br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <tr><td><pre lang="C++"> List&lt;int&gt; all_invocation_matchers; </pre></td><td style="text-align:center">DT NM NPL</td></tr>
       <tr><td><pre lang="java"> String[] all_Open_Indices; </pre></td><td style="text-align:center">DT NM NPL</td></tr>
       <tr><td><pre lang="C++"> int is_a_empty;</pre></td><td style="text-align:center">V DT N</td></tr>
       <tr><td><pre lang="C++"> boolean matches_any_parent_categories();</pre></td><td style="text-align:center">V DT NM NPL</td></tr>
      </table>
  </tr>
  <tr>
    <td class="tg-0pky">V+</td>
    <td class="tg-0pky">Verb pattern: One or more verbs with no noun phrase. These are typically functions that perform some generic functionality, such as Sort(). <br><br>The noun phrase that this action is applied to is implicit. That is, it is not present in the identifier name. Instead, the noun phrase is implied by the program context (e.g., it is represented by a this-pointer) or it is present in the function parameters. In some cases, these are boolean-type variables that may be missing an existential quantifier (e.g., add 'is' before 'parsing' to make it explicit)<br><br>These are typically function names or identifiers with a boolean type.<br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <tr><td><pre lang="C++"> void sort();</pre></td><td style="text-align:center">V</td></tr>
       <tr><td><pre lang="C++"> void delete();</pre></td><td style="text-align:center">V</td></tr>
       <tr><td><pre lang="C++"> void resume();</pre></td><td style="text-align:center">V</td></tr>
       <tr><td><pre lang="C++"> bool *parsing;</pre></td><td style="text-align:center">V</td></tr>
      </table>
  </tr>
</tbody>
</table>

# References

1. Christian D. Newman, Reem S. AlSuhaibani, Michael J. Decker, Anthony Peruma, Dishant Kaushik, Mohamed Wiem Mkaouer, Emily Hill,
On the generation, structure, and semantics of grammar patterns in source code identifiers, Journal of Systems and Software, 2020, 110740, ISSN 0164-1212, https://doi.org/10.1016/j.jss.2020.110740. (http://www.sciencedirect.com/science/article/pii/S0164121220301680) 

# Webpage
This page is currently supported by [SCANL lab](https://scanl.org/). If other research labs join this effort, we will put their webpages down here as well.

# Contribute
If you are interested in correcting something in this document, make an issue! If you would like to add, or otherwise somehow contribute, or if you're just interested in our research and want to ask questions, please email: scanl.lab@gmail.com
