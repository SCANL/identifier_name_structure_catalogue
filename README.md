
# Identifier Naming Structure Catalogue
This README will catalogue common source code identifier naming structures derived from research. These will be represented as grammar patterns [1], but for simplicity sake we will refer to them as naming structures. We first describe needed [Linguistic Terminology](#linguistic-terminology) and a [Tagset](#tagset) before presenting the [Naming Structures](#Common-naming-patterns-and-their-definition) themselves. If you're already familiar with these, skip down to the structures themselves!

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

We present each pattern, a definition for the pattern, and examples of the pattern below. The \* symbol means "zero or more" while the + symbol means "one or more" of the token to the corresponding symbol's left. Just like in regular expressions. We will expand this as we discover more patterns through our research, so be sure to check be periodically.

<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky">Grammar_Pattern_sequences</th>
    <th class="tg-0pky">Definition</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">NM* N</td>
    <td class="tg-0pky">Noun Phrase: One or more noun modifiers that appear (typically) to the left of a head-noun. The noun modifiers before the head-noun act as a way to specialize our understanding of the head-noun; they have a hypernymic relationship with the head-noun.<br><br>This is the most common naming pattern for non-function identifiers. It is good practice to be careful in the choice, and number, of noun-modifiers to use before the head-noun. A good identifier will include only enough noun-modifiers to concisely describe the head-noun.<br>
      <table>
       <tr><th colspan=2>Examples</th></tr>
       <tr><td>Identifier name</td><td>Grammar Pattern</td></tr>
       <tr><td><pre lang="C++"> GtkWidget *selection_width_label; </pre></td><td>NM NM N</td></tr>
       <tr><td><pre lang="C++"> int dynamic_Table_Index;</pre></td><td>NM NM N</td></tr>
       <tr><td><pre lang="C++"> ReadBufferOperation *read_Operation;</pre></td><td>NM N</td></tr>
      </table>
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">V NM* N</td>
    <td class="tg-0pky">Verb Phrase: a verb followed by a noun-phrase. The verb typically represents an action and the noun-phrase represents an in-memory entity upon which the action will be applied. In some cases, instead of being an action, the verb is an existential quantifier.<br><br>Identifiers with this pattern are typically either function identifiers or identifiers with a boolean type.<br>
      <table>
       <tr><th colspan=2>Examples</th></tr>
       <tr><td>Identifier name</td><td>Grammar Pattern</td></tr>
       <tr><td><pre lang="C++"> bool create_metadata_array(); </pre></td><td>V NM N</td></tr>
       <tr><td><pre lang="C++"> bool is_First_frame;</pre></td><td>V NM N</td></tr>
       <tr><td><pre lang="C++"> int create_Duplicate_Change_Id();</pre></td><td>V NM NM N</td></tr>
      </table>
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">NM* NPL</td>
    <td class="tg-0pky">Plural noun phrase: This is very similar to a noun phrase except it contains a plural head-noun, which is sometimes purposeful in that the plural head-noun expresses the multiplicity of the data that it represents.<br><br>Identifiers with a plural head-noun are somewhat more likely to have a collection type, based on our data [1]. Some naming conventions (e.g., the Java naming standard) generally consider it good practice to match the plurality of the identifier with whether its type represents a singular, or collection, object.<br>
      <table>
       <tr><th colspan=2>Examples</th></tr>
       <tr><td>Identifier name</td><td>Grammar Pattern</td></tr>
       <tr><td><pre lang="C++"> int training_examples; </pre></td><td>NM NPL</td></tr>
       <tr><td><pre lang="C++"> String[] method_Name_Prefixes;</pre></td><td>NM NM NPL</td></tr>
       <tr><td><pre lang="C++"> std::vector&lt;...&gt; curves;</pre></td><td>NPL</td></tr>
      </table>
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">P NM* N</td>
    <td class="tg-0pky">Prepositonal phrase pattern: The preposition is usually gives us an understand of how the noun phrase is related to an un-specified phrase that, in normal English, would appear before the preposition. This relationship is usually meant to provide the developer with some context related to when, or how, the noun phrase is relevant in the context of the un-specified phrase that should appear before the preposition. <br><br><br>The un-specific phrase is sometimes an implicit verb such as GET (e.g., w/market rate accuracy), CONVERT (e.g., int to string), EXECUTE (e.g., on connect) or some other action. The preposition helps us understand the relationship to the un-specified phrase by telling us HOW something will be represented (int_to_string), an ACTION that requires utilizing the relationship specified by the preposition (w/market rate accuracy), or WHEN something will happen (e.g., on connect).<br>
      <table>
       <tr><th colspan=2>Examples</th></tr>
       <tr><td>Identifier name</td><td>Grammar Pattern</td></tr>
       <tr><td><pre lang="C++"> void On_Connect(); </pre></td><td>P N</td></tr>
       <tr><td><pre lang="C++"> ModelSettings&amp; with_Market_Rate_Accuracy(); </pre></td><td>P NM NM N</td></tr>
       <tr><td><pre lang="C++"> btVector3 from_Local_Aabb_Min;</pre></td><td>N NM NM N</td></tr>
       <tr><td><pre lang="C++"> string to_string();</pre></td><td>P N</td></tr>
      </table>
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">NM* N P NM* N(PL)</td>
    <td class="tg-0pky">Explicit prepositional phrase with leading noun phrase: Same as the prepositional phrase pattern except the noun phrase before the preposition IS specified. <br><br>When the left-hand-side noun-phrase is specified, there is an explicit relationship between the left- and right-hand side noun-phrases that the preposition represents. The preposition helps us understand WHEN something will happen (e.g., generated token on creation), HOW something is represented (e.g., in milliseconds, from JSON), the PURPOSE of something (e.g., for node), and other related concepts.<br>
      <table>
       <tr><th colspan=2>Examples</th></tr>
       <tr><td>Identifier name</td><td>Grammar Pattern</td></tr>
       <tr><td><pre lang="C++"> String generated_Token_On_Creation; </pre></td><td>V N P N</td></tr>
       <tr><td><pre lang="C++"> long query_Timeout_In_Milliseconds; </pre></td><td>NM N P NPL</td></tr>
       <tr><td><pre lang="C++"> class Scroll_Id_For_Node;</pre></td><td>NM N P N</td></tr>
       <tr><td><pre lang="C++"> HttpUrl url_from_json();</pre></td><td>N P N</td></tr>
      </table>
    </td>
    
  </tr>
  <tr>
    <td class="tg-0pky">V P NM* N</td>
    <td class="tg-0pky">Explicit prepositional phrase with leading verb: Same as prepositional phrase pattern but the leading verb is specified. These are similar to regular verb phrases with an action being applied to an object. <br><br>The usage of this pattern is similar to when the verb is implicit. There may still be an implicit noun phrase to the right of the verb and to the left of the preposition. The preposition helps us understand the relationship between the verb, any unspecified phrases, and the explicit phrase on the left hand side. The preposition helps us understand HOW something will be represented (e.g., convert to php namespace, tessellate to mesh, save as quadratic png) or an ACTION that requires understanding the relationship specified by the preposition (e.g., destroy_with_parent).<br><br>Notice that there is an implicit noun phrase in all of these examples. Save WHAT? Convert WHAT? Destroy WHAT? Tessellate WHAT? In each case, the noun phrase is likely the calling object and is implied by the code context.<br>
      <table>
       <tr><th colspan=2>Examples</th></tr>
       <tr><td>Identifier name</td><td>Grammar Pattern</td></tr>
       <tr><td><pre lang="C++"> gboolean destroy_with_parent; </pre></td><td>V N P</td></tr>
       <tr><td><pre lang="C++"> string convert_to_php_namespace(); </pre></td><td>V P NM N</td></tr>
       <tr><td><pre lang="C++"> void tessellate_To_Mesh();</pre></td><td>V P N</td></tr>
       <tr><td><pre lang="C++"> void save_As_Quadratic_Png();</pre></td><td>V P NM N</td></tr>
      </table>
  </tr>
  <tr>
    <td class="tg-0pky">V* DT NM* N(PL)</td>
    <td class="tg-0pky">Plural noun phrase with leading determiner: A determiner followed by a plural noun phrase. The determiner tells us the population, which is related to the noun phrase, that is represented by the identifier. <br><br>Typically, the determiner will tell us that we are interested in ALL, ANY, ONE, A, THE, SEVERAL, etc., of the objects specified by the plural noun phrase. If there is a leading verb, the verb specifies an action to take on the population or it represents existential quantification (e.g., matchesAnyParentCategories)<br>
      <table>
       <tr><th colspan=2>Examples</th></tr>
       <tr><td>Identifier name</td><td>Grammar Pattern</td></tr>
       <tr><td><pre lang="C++"> List&lt;int&gt; all_invocation_matchers; </pre></td><td>DT NM NPL</td></tr>
       <tr><td><pre lang="C++"> String[] all_Open_Indices; </pre></td><td>DT NM NPL</td></tr>
       <tr><td><pre lang="C++"> int is_a_empty;</pre></td><td>V DT N</td></tr>
       <tr><td><pre lang="C++"> boolean matches_any_parent_categories();</pre></td><td>V DT NM NPL</td></tr>
      </table>
  </tr>
  <tr>
    <td class="tg-0pky">V+</td>
    <td class="tg-0pky">Verb pattern: One or more verbs with no noun phrase. These are typically functions that perform some generic functionality, such as sort. <br><br>The noun phrase that this action is applied to is implicit. That is, it is not present in the identifier name. Instead, the noun phrase is implied by the program context (e.g., it is represented by a this-pointer) or it is present in the function parameters. In some cases, these are boolean-type variables that may be missing an existential quantifier (e.g., add 'is' before 'parsing' to make it explicit)<br>
      <table>
       <tr><th colspan=2>Examples</th></tr>
       <tr><td>Identifier name</td><td>Grammar Pattern</td></tr>
       <tr><td><pre lang="C++"> void sort();</pre></td><td>V</td></tr>
       <tr><td><pre lang="C++"> void delete();</pre></td><td>V</td></tr>
       <tr><td><pre lang="C++"> void resume();</pre></td><td>V</td></tr>
       <tr><td><pre lang="C++"> bool *parsing;</pre></td><td>V</td></tr>
      </table>
  </tr>
</tbody>
</table>

# Please cite the paper!

1. Christian D. Newman, Reem S. AlSuhaibani, Michael J. Decker, Anthony Peruma, Dishant Kaushik, Mohamed Wiem Mkaouer, Emily Hill,
On the generation, structure, and semantics of grammar patterns in source code identifiers, Journal of Systems and Software, 2020, 110740, ISSN 0164-1212, https://doi.org/10.1016/j.jss.2020.110740. (http://www.sciencedirect.com/science/article/pii/S0164121220301680) 

Keywords: Program comprehension; Identifier naming; Software maintenance; Source code analysis; Part-of-speech tagging
