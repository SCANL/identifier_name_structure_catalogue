
# Identifier Naming Structure Catalogue
This README will catalogue common source code identifier naming structures, best practices, and semantics derived from research. The goal of this document is to act as a resource for researchers, students, and developers that want to learn about what is scientifically known about naming identifiers. We are currently looking into other types of identifier characteristics that should be included in this document. **This is a living document**, we will expand this as we discover more patterns and characteristics through our, and possibly others', research. Check back periodically for more information!

This document is broken down into the following sections:
- [Linguistic Terminology](#linguistic-terminology) used throughout the document.

- [Part-of-speech Tagset](#tagset) used throughout the document.

- [Common Naming Structures](#Common-naming-patterns-and-their-definition) derived by analyzing identifier names and deriving part-of-speech sequences called grammar patterns. This section discusses common identifier naming patterns and their meaning.

- [Linguistic Antipatterns](#Linguistic-Antipatterns), which are recurring, detrimental practices in the naming, documentation, and/or choice of identifier. In this section we provide the antipattern name, a definition, an example, and several options for resolving the antipattern.

- [Naming Styles](#Naming-Styles), which are practices that dictate how identifiers should be lexically formed. The three most common naming styles: camelCase, under_score, and PascalCase are pivotal to developer comprehension.

# Linguistic Terminology
First you should be familiar with some simple linguistic concepts.
| Linguistic-terminology | Definition                                                                                                                                                                                                                                                                                            |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Head-noun              | The right-most noun in a noun phrase is typically referred to as a head-noun. This noun is the word that most-closely embodies the concept that represents the in-memory entity that the identifier is used to describe.|
| Noun-adjunct           | Noun-adjuncts are defined as a noun acting as (i.e., being used as) an adjective. These are found in certain types of compound-words which, in English, are often groups of two-or-more words separated by a dash. For example, in the word employee-name, 'employee' is a noun-adjunct and 'name' is a noun (or, more specifically, a head-noun). |
| Hypernym               | A word with a broad meaning that more specific words fall under; a superordinate. For example, color is a hypernym of red. [Definition from Oxford Languages](https://languages.oup.com/google-dictionary-en/)                                                                                                                                                                         |
| Hyponym                | a word of more specific meaning than a general or superordinate term applicable to it. For example, spoon is a hyponym of cutlery. [Definition from Oxford Languages](https://languages.oup.com/google-dictionary-en/)                                                                         |

# Tagset
The tagset that we use is a subset of Penn treebank. Each of our annotations and an example can be found below. Further examples and definitions can be found in the paper [1]

| Abbreviation | Expanded Form                           | Examples                                                        |
|--------------|-----------------------------------------|-----------------------------------------------------------------|
| N            | noun                                    | Disneyland, shoe, faucet, mother, bedroom                       |
| DT           | determiner                              | the, this, that, these, those, which                            |
| CJ           | conjunction                             | and, for, nor, but, or, yet, so                                 |
| P            | preposition                             | behind, in front of, at, under, beside, above, beneath, despite |
| NPL          | noun plural                             | streets, cities, cars, people, lists, items, elements.          |
| NM           | noun modifier (adjective)               | red, cold, hot, scary, beautiful, happy, faster, small          |
| NM           | noun modifier (noun-adjunct *italicized*) | *employee*Name, *file*Path, *font*Size, *user*Id              |
| V            | verb                                    | run, jump, drive, spin                                          |
| VM           | verb modifier (adverb)                  | very, loudly, seriously, impatiently, badly                     |
| PR           | pronoun                                 | she, he, her, him, it, we, us, they, them, I, me, you           |
| D            | digit                                   | 1, 2, 10, 4.12, 0xAF                                            |
| PRE          | preamble (e.g., Hungarian)              | Gimp, GLEW, GL, G, p_, m_, b_                                   |

# Common naming patterns and their definition
The grammar patterns below represent different naming structures found in source code; they are represented by sequences of part-of-speech tags. The patterns we present are all empirically derived from a manually-tagged sample of 1,335 identifiers. Refer to the paper [1] for more information. The manually tagged  dataset is freely available [here](https://github.com/SCANL/datasets/tree/master/grammar_patterns_data).

We present each pattern, a definition for the pattern, and examples of the pattern below. We use regular expression synax, where the \* symbol means "zero or more" while the + symbol means "one or more" of the token.

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
    <td class="tg-0pky"><b>Noun Phrase</b>: Zero or more noun-modifiers appear to the left of a head-noun. Noun-modifiers that appear before the head-noun act as a way to specialize our understanding of the head-noun by taking the general concept the head-noun represents and reducing it to a more concise, specific concept. For example, in the identifier 'issueDescription' the head-noun is 'Description', which is the general concept. The noun-adjunct, 'issue', specializes our understanding of the description by specifying what kind of 'description' we are talking about. <br><br>This is the most common naming pattern for non-function identifiers. It is good practice to be careful in the choice, and number, of noun-modifiers to use before the head-noun. A good identifier will include only enough noun-modifiers to concisely define the concept represented by the head-noun.<br><br> These are typically non-function identifier names.  Here are examples following the pattern:<br>
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
    <td class="tg-0pky"><b>Plural noun phrase</b>: This is identical to <I>NM* N</I>, except the head noun is plural.  The plural is often purposeful in that the plural head-noun expresses the multiplicity of the data that the identifier represents and it likely has a collection type [1].<br><br>
Some naming conventions (e.g., the Java naming standard) generally consider it good practice to match the plurality of the identifier with whether its type represents a singular or collection, object.<br><br> These are typically non-function identifier names.  Here are examples following the pattern:<br>
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
    <td class="tg-0pky">V NM* (N|NPL)</td>
    <td class="tg-0pky"><b>Verb Phrase</b>: The addition of a verb to a noun phrase creates a verb phrase. The verb in a verb phrase is an action being applied to (or with) the concept embodied by the noun phrase that follows. In some cases, instead of being an action, the verb is an existential quantifier.<br><br>These are typically either function identifiers or identifiers with a boolean type. Here are examples following the pattern:<br>
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
    <td class="tg-0pky">P NM* (N|NPL)</td>
    <td class="tg-0pky"><b>Prepositonal phrase pattern</b>: A noun or verb-phrase with a leading preposition is a prepositional phrase. The preposition in a prepositional phrase typically explains how the entity (or entities) represented by the accompanying noun or verb-phrase are related in terms of order, space, time (e.g., <I>on_enter</I>), ownership, causality, or representation (e.g., <I>to_string</I>). In the case of this specific grammar pattern, there is oftentimes an un-specified verb on the left-hand-side of the preposition. <br><br>The un-specified verb is usually an action such as the following: <I>GET</I>, <I>CONVERT</I> (e.g., to string), <I>EXECUTE</I> (e.g., on enter) or some other action. Developers understand the implied action because of experience or domain knowledge, for example, understanding event-driven functions beginning with the preposition 'on'. There may also be noun-phrase to the left of the preposition. We discuss these in another grammar pattern below.<br><br> These can be functions or non-function identifier names. Here are examples following the pattern:<br>
      <table style="margin-left:auto;margin-right:auto;">
       <tr><th style="text-align:center;font-weight:bold" colspan=2>Examples</th></tr>
       <tr><th style="text-align:center;font-weight:bold">Identifier Name</th><th style="font-weight:bold">Grammar Pattern</th></tr>
       <!-- <tr><td><pre lang="C++"> void On_Connect(); </pre></td><td style="text-align:center">P V</td></tr> -->
       <tr><td><pre lang="C++"> ModelSettings&amp; with_Market_Rate_Accuracy(); </pre></td><td style="text-align:center">P NM NM N</td></tr>
       <tr><td><pre lang="java"> btVector3 from_Local_Aabb_Min;</pre></td><td style="text-align:center">P NM NM N</td></tr>
       <tr><td><pre lang="C++"> String to_string();</pre></td><td style="text-align:center">P N</td></tr>
      </table>
    </td>
  </tr>
  <tr>
    <td class="tg-0pky">NM* N P NM* (N|NPL)</td>
    <td class="tg-0pky"><b>Prepositional phrase with leading noun phrase</b>: Sometimes a noun phrase is explicitly present on both the left and right of the preposition. When the left-hand-side noun-phrase is specified, there is an explicit relationship between the left- and right-hand side noun-phrases. This relationship is expressed through the preposition. The preposition helps us understand how the entity (or entities) represented by both noun-phrases are related in terms of order, space, time (e.g., <I>generated_token_on_creation</I>), ownership (e.g., <I>scroll_id_for_node</I>), causality, or representation (e.g., <I>url_from_json</I>, <I>query_timeout_in_milliseconds</I>).<br><br> These can be functions or non-function identifier names. Here are examples following the pattern:<br>
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
    <td class="tg-0pky">V P NM* (N|NPL)</td>
    <td class="tg-0pky"><b>Prepositional phrase with leading verb</b>: Same as prepositional phrase pattern but the leading verb, or verb phrase, is specified. As before, the preposition helps us understand how the entity (or entities) represented by the verb- and noun-phrases are related in terms of order, space, time, ownership, causality (e.g., <I>destroy_with_parent</I>), or representation (e.g., <I>save_as_quadratic_png</I>, <I>tessellate_to_mesh</I>, <I>convert_to_php_namespace</I>).<br><br> The usage of this pattern is similar to when the verb is implicit. There may still be an implicit noun phrase to the right of the verb and to the left of the preposition.<br><br> These can be functions or non-function identifier names. Here are examples following the pattern:<br>
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
    <td class="tg-0pky">V* DT NM* (N|NPL)</td>
    <td class="tg-0pky"><b>Noun phrase with leading determiner</b>: The addition of a determiner tells us how much of the population, which is specified by the noun-phrase, is represented, or acted on, by the identifier. <br><br>Typically, the determiner will tell us that we are interested in <I>ALL</I>, <I>ANY</I>, <I>ONE</I>, <I>A</I>, <I>THE</I>, <I>SEVERAL</I>, etc., of the population of objects specified by the noun phrase. If there is a leading verb, the verb specifies an action to take on the population or it represents existential quantification (e.g., <I>matchesAnyParentCategories</I>)<br><br>These can be functions or non-function identifier names. Here are examples following the pattern:<br>
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
    <td class="tg-0pky"><b>Verb pattern</b>: One or more verbs with no noun phrase. Because these are missing a noun phrase (i.e., a target) to act upon, a larger population of these are probably generic functions like Sort (though more data is needed), which can act upon many different types of data and have different behaviors depending on the data being acted upon. <br><br>The noun phrase that this action (i.e., the verb) is applied to is implicit. That is, it is not present in the identifier name. Instead, the noun phrase is implied by the program context (e.g., it is represented by a this-pointer) or it is present in the function parameters. In some cases, these are boolean-type variables that may be missing an existential quantifier (e.g., add 'is' before 'parsing' to make it explicit)<br><br>These are typically function names or identifiers with a boolean type. Here are examples following the pattern:<br>
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

# Linguistic Antipatterns
Linguistic Antipatterns (LAs) in software systems are recurring, detrimental practices in the naming, documentation, and/or choice of identifier in the implementation of an entity; thus impairing program understanding [2]. They typically take the form of an identifier name that incorrectly describes the behavior of the entity that it represents OR an entity that betrays the behavior conveyed linguistically by its corresponding identifier.
<table>
<thead>
  <tr>
    <th><b>Name</b></th>
    <th><b>Definition and Example</b></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Get more than accessor</td>
    <td>A getter that performs actions other than returning the corresponding attribute. Example: method <code>getImageData</code> which always returns a <code>new object</code>.<br/>
      <pre lang="C++">
        ImageData getImageData(){
          final Point size = this.getSize();
          this.imageData = new ImageData(size.x, size.y, 8);
          return this.imageData;
        }
      </pre>
      How to resolve: 
      <ol>
      <li>The method name should change so that it is not a getter or</li>
      <li>the implementation should be corrected to conform to standard get-method behavior</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Is returns more than a Boolean</td>
    <td>The name of a method is a predicate suggesting a true/false value in return. However the return type is not Boolean but rather a more complex type thus allowing a wider range of values without documenting them. Example: method <code>isValid</code> with return type <code>int</code>.<br/>
      <pre lang="Java">
        public int isValid(){
            final long currentTime = System.currentTimeMillis();
            if (currentTime <= this.expires) {
              // The delay has not passed yet -
              // assuming source is valid.
              return SourceValidity.VALID;
            }
          // The delay has passed, prepare for the next interval.
          this.expires = currentTime + this.delay;
          return this.delegate.isValid();
        }
      </pre>
      How to resolve:
      <ol>
        <li>The type should be changed to boolean to reflect the function's behavior as a binary predicate.</li>
        <li>Consider changing the name such that it does not imply a yes/no question and provides some indication of n-ary return values.
        <li>Carfully document the meaning of each value that can be returned. Thoroughly test each value.</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Set method returns</td>
    <td>A set method having a return type different than void without proper documentation of the return type/values. Example: method <code>setBreadth</code> has a non-<code>void</code> return type.
    <pre lang="Java">
      public Dimension setBreadth(final Dimension target, final int source) {
        if (this.orientation == Orientation.VERTICAL) {
          return new Dimension(source, (int) target.getHeight());
        } else {
          return new Dimension((int) target.getWidth(), source);
        }
      }
    </pre>
      How to resolve:
      <ol>
        <li>The word <i>set</i>, when used in this manner, has a specific definition in the programming domain. Consider using a different term, such as <i>change</i>.</li>
        <li>Correct the implementation such that it works like a stereotypical set method (i.e., void return, mutates a class attribute)</li>
        <li>Carefully document the reasoning behind using <i>set</i> while also returning a value</li>
      </ol>
    </td>

  </tr>
  <tr>
    <td>Expecting but not getting single instance</td>
    <td>The name of a method indicates that a single object is returned but the return type is a collection. Example: method <code>getExpansion</code>, which ends with a head-noun that is <i>singular</i>, but returns a <code>List</code> object.
      <pre lang="Java">
        /**
          * Returns the expansion state for a tree.
          *
          * @return the expansion state for a tree
        */
        public List getExpansion() {
          return this.fExpansion;
        }
      </pre>
      How to resolve:
      <ol>
        <li>Correct the method name so that it is plural-- <code>getExpansions()</code></li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Not implemented condition</td>
    <td>The comments of a method suggest a conditional behavior that is not implemented in the code. When the implementation is default this should be documented. Example: method <code>getChildren</code> has a comment which indicates there should be a conditional within its body.
      <pre lang="Java">
        /**
        * Returns the children of this object. When this object is
        * displayed in a tree, the returned objects will be this
        * element's children. Returns an empty array if this object
        * has no children.
        *
        * @param object The object to get the children for.
        */
        public Object[] getChildren(final Object o) {
          return new Object[0];
        }
      </pre>
      How to resolve:
      <ol>
        <li>Complete implementation of the method</li>
        <li>Document (i.e., update the comment) that the method is incomplete and does not implement the behavior indicated in its comment</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Validation method does not confirm</td>
    <td>A validation method (e.g., name starting with "validate", "check", "ensure") does not confirm the validation, i.e., the method neither provides a return value informing whether the validation was successful, nor documents how to proceed to understand. Example: method <code>checkCollision</code> returns <code>void</code> despite indicating that it is designed to perform validation.
      <pre lang="Java">
        public void checkCollision(final String before,
                                   final String after) {
          final boolean collision = before != null
              && before.equals(this._shortName) || after != null
              && after.equals(this._shortName);
          if (collision) {
            if (this._longName == null) {
              this._longName = this.getLongName();
            }
            this. _displayName = this._longName;
          }
        }
      </pre>
      How to resolve:
      <ol>
        <li>Change method to return confirmation (i.e., true or false)</li>
        <li>Consider changing the name to avoid implication of validation behavior (i.e., avoid terms like check and is)</li>
        <li>If the previous options are not available then thoroughly document method behavior, consider highlighting irregular validation behavior</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Get method does not return</td>
    <td>The name suggests that the method returns something (e.g., name starts with "get" or "return") but the return type is void. The documentation should explain where the resulting data is stored and how to obtain it. Example: method <code>getMethodBodies</code> has a <code>void</code> return type but its name indicates that it is a getter method.
    <pre lang="Java">
      protected void getMethodBodies(
        final CompilationUnitDeclaration unit,
        final int place) {
          //[Removed some code for conciseness]
          this.parser.scanner
            .setSourceBuffer(
              unit.compilationResult.compilationUnit
              .getContents());
          if (unit.types != null) {
            for (int i = unit.types.length; --i >= 0;) {
              unit.types[i].parseMethod(this.parser, unit);
            }
          }
      }
    </pre>
      How to resolve:
      <ol>
        <li>Change method to return correct entity.</li>
        <li>Consider changing the name to avoid the word <i>get</i></li>
        <li>If the previous options are not available then thoroughly document method behavior, consider highlighting irregular getter behavior</li> 
      </ol>
    </td>
  </tr>
  <tr>
    <td>Not answered question</td>
    <td>The name of a method is in the form of predicate whereas the return type is not Boolean. Example: method <code>isValid</code> with a <code>void</code> return type.
      <pre lang="Java">
        public void isValid(final Object[] selection,
                            final StatusInfo res) {
          // only single selection
          if (selection.length == 1
              && selection[0] instanceof IFile) { 
              res.setOK();
          } else {
              res.setError(""); //$NON-NLS-1$
          }
        }
      </pre>
      <ol>
        <li>Change method to return correct entity.</li>
        <li>Consider changing the name to avoid the word <i>get</i></li>
        <li>If the previous options are not available then thoroughly document method behavior, consider highlighting irregular getter behavior</li> 
      </ol>
    </td>
  </tr>
  <tr>
    <td>Transform method does not return</td>
    <td>The name of a method suggests the transformation of an object but there is no return value and it is not clear from the documentation where the result is stored. Example: method <code>javaToNative</code> has a <code>void</code> return type but indicates that it performs a transformation (i.e., type conversion).
      <pre lang="Java">
        public void javaToNative(final Object object,
                                 final TransferData transferData) {
          final byte[] check =
              LocalSelectionTransfer.TYPE_NAME.getBytes();
          super.javaToNative(check, transferData);
        }
      </pre>
      <ol>
        <li>Change method to return correct entity.</li>
        <li>If the previous option is not available then thoroughly document method behavior, consider highlighting irregular transformation behavior</li> 
      </ol>
    </td>
  </tr>
  <tr>
    <td>Expecting but not getting a collection</td>
    <td>The name of a method suggests that a collection should be returned but a single object or nothing is returned. Example: method <code>getStats</code> with a <code>Boolean</code> return type; making it difficult to understand the reason behind the plurality of the method name.
      <pre lang="Java">
        public boolean getStats() {
          return SAXParserBase._stats;
        }
      </pre>
      <ol>
        <li>Change the name of the method (and any related identifier names) so that it is singular instead of plural</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Method name and return type are opposite</td>
    <td>The intent of the method suggested by its name is in contradiction with what it returns. Example: method <code>disable</code> with return type <code>ControlEnableState</code>. The words "disable" and "enable" having opposite meanings.
      <pre lang="Java">
        public static ControlEnableState disable(Control w) {
          return new ControlEnableState(w);
        }
      </pre>
      <ol>
        <li>Change method name so that it aligns better with the return type (i.e., change <i>disable</i> to <i>enable</i>)</li>
        <li>Change type name to align better with method name (i.e., to ControlDisableState)</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Method signature and comment are opposite</td>
    <td>The documentation of a method is in contradiction with its declaration. Example: method <code>isNavigateForwardEnabled</code> is in contradiction with its comment documenting "a back navigation", as "forward" and "back" are antonyms
      <pre lang="Java">
        /**
        *	Returns true if this listener has a target for a
        *	back navigation. Only one listener needs to return
        *	true for the back button to be enabled.
        */
        public boolean isNavigateForwardEnabled() {
          boolean enabled = false;
          if (this._isForwardEnabled == 1) { 
            enabled = true;
          } else {
            if (this._isForwardEnabled != 0) { enabled =
              this.navigateForward(false) != null;
            }
          }
          return enabled;
        }
      </pre>
      <ol>
        <li>Change the comment to specify that this method is for forward navigation</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Says one but contains many</td>
    <td>The name of an attribute suggests a single instance, while its type suggests that the attribute stores a collection of objects. Example: attribute <code>_target</code> that is of type <code>Vector</code>. It is unclear whether a change aspects one or multiple instances in the collection.
      <pre lang="Java">
        Vector _target;
      </pre>
      <ol>
        <li>Change the identifier name to reflect plurality of its type (i.e., <i>_target</i> -> <i>_targets</i>)</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Name suggests boolean but type is not</td>
    <td>The name of an attribute suggests that its value is true or false, but its declaring type is not Boolean. Example: attribute <code>isReached</code> that is of type <code>int[]</code> where the declared type and values are not documented.
      <pre lang="Java">
        int[] isReached;
      </pre>
      <ol>
        <li>Change the name of the identifier to be more descriptive with respect to what kind of array it represents.</li>
        <li>Consider removing the word <i>is</i> and using a different term unless the array represents a sequence of appropriate (i.e., boolean-like) values</li>
        <li>If appropriate, consider using a boolean array</li>
        <li>Carefully document the data represented by the array, including the reasoning for its integer type and whether different integer values have different meanings</li> 
      </ol>
    </td>
  </tr>
  <tr>
    <td>Says many but contains one</td>
    <td>The name of an attribute suggests multiple instances, but its type suggests a single one. Example: attribute <code>stats</code> that is of type <code>Boolean</code>. Documenting such inconsistencies avoids additional comprehension effort to understand the purpose of the attribute.
      <pre lang="Java">
        private static boolean _stats = true;
      </pre>
      <ol>
        <li>Change identifier name to singular instead of plural</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Attribute name and type are opposite</td>
    <td>The name of an attribute is in contradiction with its type as they contain antonyms. Example: attribute <code>start</code> that is of type <code>MAssociationEnd</code>. The use of antonyms can induce wrong assumptions.
      <pre lang="Java">
        MAssociationEnd start = null;
      </pre>
      <ol>
        <li>Change identifier name to align with type name (i.e., change <i>start</i> to <i>end</i>).</li>
      </ol>
    </td>
  </tr>
  <tr>
    <td>Attribute signature and comment are opposite</td>
    <td>The declaration of an attribute is in contradiction with its documentation. Example: attribute <code>INCLUDE_NAME_DEFAULT</code> whose comment documents an "exclude pattern". Whether the pattern is included or excluded is thus unclear.
      <pre lang="Java">
        /**
        *	Configuration default exclude pattern,
        *	ie .*\/@href|.*\/@action|frame/@src
        */
        public final static String INCLUDE_NAME_DEFAULT
          = ".*/@href=|.*/@action=|frame/@src=";
      </pre>
      <ol>
        <li>Change identifier name to align with comment (i.e., <i>include</i> -> <i>exclude</i>)</li>
        <li>Change comment to align with method name (i.e., <i>exclude</i> -> <i>include</i>)</li>
      </ol>
    </td>
  </tr>
</tbody>
</table>

# Naming Styles
Naming style concerns the lexical structure of an identifier name. The three most common naming styles are camelCase, under_score, and PascalCase. Prior research [3] found that camelCase and under_score do not significantly differ in terms of improving or degrading the comprehension abilities of developers as long as the developer had training or experience using the given style. It is worth noting that this same paper found that camelCase has a slight edge in terms of comprehension for shorter identifier names. This observation is supported by [4] and [5]. The importance of naming style was further emphasized in a study of developer opinions on identifier naming practices [6].

Because there has been no data to suggest that one naming style is better than the others, it is most important that development projects pick a naming style and remain consistent in the usage of that naming style throughout the code.

<table class="tg">
<thead>
  <tr>
    <th class="tg-0pky"><b>Naming Style</b></th>
    <th class="tg-0pky"><b>Definition</b></th>
    <th class="tg-0pky"><b>Example</b></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">camelCase</td>
    <td class="tg-0pky">The first letter of each word in an identifier, except the first word, is capitalized</td>
    <td class="tg-0pky"><code>getFullName()</code></td>
  </tr>
  <tr>
    <td class="tg-0pky">under_score</td>
    <td class="tg-0pky">An under_score is placed between each word in the identifier</td>
    <td class="tg-0pky"><code>call_with_default()</code></td>
  </tr>
  <tr>
    <td class="tg-0pky">PascalCase</td>
    <td class="tg-0pky">The first letter of each word in an identifier, including the first word, is capitalized.</td>
    <td class="tg-0pky"><code>NewObject()</code></td>
  </tr>
  <tr>
    <td class="tg-0pky">kebab-case</td>
    <td class="tg-0pky">This is a variant of under_score, used in languages that allow dashes (-) in identifier names, such as Lisp and Forth</td>
    <td class="tg-0pky"><code>employee-name</code></td>
  </tr>
</tbody>
</table>

# References

1. Christian D. Newman, Reem S. Alsuhaibani, Michael J. Decker, Anthony Peruma, Dishant Kaushik, Mohamed Wiem Mkaouer, Emily Hill,
On the generation, structure, and semantics of grammar patterns in source code identifiers, Journal of Systems and Software, 2020, 110740, ISSN 0164-1212, https://doi.org/10.1016/j.jss.2020.110740. (http://www.sciencedirect.com/science/article/pii/S0164121220301680) 

2. Arnaoudova, V., Di Penta, M. & Antoniol, G. Linguistic antipatterns: what they are and how developers perceive them. Empir Software Eng., Vol 21, 104–158 (2016). https://doi.org/10.1007/s10664-014-9350-8

3. Binkley, D., Davis, M., Lawrie, D. et al. The impact of identifier style on effort and comprehension. Empir Software Eng 18, 219–276 (2013). https://doi.org/10.1007/s10664-012-9201-4

4. D. Binkley, M. Davis, D. Lawrie and C. Morrell, "To camelcase or under_score," 2009 IEEE 17th International Conference on Program Comprehension, 2009, pp. 158-167, doi: https://doi.org/10.1109/ICPC.2009.5090039.

5. B. Sharif and J. I. Maletic, "An Eye Tracking Study on camelCase and under_score Identifier Styles," 2010 IEEE 18th International Conference on Program Comprehension, 2010, pp. 196-205, doi: https://doi.org/10.1109/ICPC.2010.41.

6. R. S. Alsuhaibani, C. D. Newman, M. J. Decker, M. L. Collard and J. I. Maletic, "On the Naming of Methods: A Survey of Professional Developers," 2021 IEEE/ACM 43rd International Conference on Software Engineering (ICSE), 2021, pp. 587-599, doi: https://doi.org/10.1109/ICSE43902.2021.00061.

# Acknowledgements
This material is based in part upon work supported by the National Science Foundation under Grant No. 1850412.

# Webpage
This page is currently supported by [SCANL lab](http://www.scanl.org/). If other research labs join this effort, we will put their webpages down here as well.

# Contribute
If you are interested in correcting something in this document, make an issue! If you would like to add, or otherwise somehow contribute, or if you're just interested in our research and want to ask questions, please email: scanl.lab@gmail.com
