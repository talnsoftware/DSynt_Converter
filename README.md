# DSynt_Converter
This tool is a graph transducer that creates an alternative version of an input dependency treebank in the CoNLL09 format, called deep-syntactic (DSynt) annotation.


----------
  README
----------
Welcome to DSynt Converter!

(v0.7) - 16/09/2015

Authors: Simon Mille, Roberto Carlini (firstname.lastname@upf.edu)


-------------
Requirements
-------------
Java 1.6 or posterior


-------------
Introduction
-------------
* <b>Structures</b>: the deep-syntactic annotation is a connected tree which aims at containing only meaningful words and both predicate-argument (I, II, III, etc.) and syntactic (modifier, coordination) relations. The nodes in the original (superficial) and deep annotations are connected through their IDs. In the FEATS column of the output CoNLL file, "id0" indicates the deep identifier of a word, while "id1" indicates the ID of the superficial node it corresponds to. There are less nodes in DSynt than in SSynt, since the latter contains all the words of a sentence. Hence, a DSynt node can correspond to several superficial nodes; multiple correspondences are indicated by the presence of the "id2" ("id3", "id4", etc) feature in the FEATS column. The features "definiteness", "voice", "tense", "tem_constituency" in the FEATS column capture the information conveyed by determiners and auxiliaries.

* <b>Tool</b>: the tool is developed on two independent aspects:
  (i) the resources (grammars and dictionaries), which contain all the linguistic knowledge; the updates affect the content of the produced file;
  (ii) the graph transduction environment, which uses the resources in order to generate the output; the updates affect the way the file is produced.

* The conversion is not instantaneous; it takes a few minutes to finish. For v0.7, on a 64-bit Intel Core i5-3470 CPU @ 3.20GHz with 8BG RAM, the conversion of a 1,000,000 word file can take about 3 hours.


--------
Releases
--------
<b>v0.7</b>
* English
  - Input: Dependency version of Penn Treebank, as in the 2009 CoNLL Shared Task
  - Version of graph-transduction grammars: v0.6 (see at the bottom of the page)

<b>v0.6</b>
* English
  - Input: Dependency version of Penn Treebank, as in the 2009 CoNLL Shared Task
  - Version of graph-transduction grammars: v0.2/v0.3


--------------------------
How to run the conversion
--------------------------
(0) Download the .jar from the "release" section of the folder; if it does not appear towards the top of this page, click on "DSynt_Converter" in the path "talnsoftware/DSynt_Converter".

(i) <b>IMPORTANT!</b>
* Input file should be in the CoNll 2009 format; see http://ufal.mff.cuni.cz/conll2009-st/task-description.html (predicted columns and the columns after the 14th are ignored).
* All empty lines (betwen two consecutive sentences) must be empty! No spaces or tabs! (The evaluation set of the PTB for example can have these tabs)
* Input file should have .conll extension
* Input file should be encoded as UTF-8 without BOM

(ii) Command line
* Through the command line, go to the directory containing the desired version of converter.jar.
* Execute:
 java -jar converter.jar -i inputfile.conll -o outputFolder

Example: 
  The converter_V0.7.jar is located under 'C:\Users\smille\Desktop\TALN\Corpus\DSynt Converter\export7'.
  So, I go to the directory:
    C:> cd "C:\Users\smille\Desktop\TALN\Corpus\DSynt Converter\export7"
  And then, execute:
    C:\Users\smille\Desktop\TALN\Corpus\DSynt Converter\export7>java -jar converter_V0.7.jar -i CoNLL2009-ST-English-train.conll -o out

* Accepts absolute paths in input file (-i) and output folder (-o) parameters, if the input file is not in the same folder as the .jar.


---------------------------------------
(Optional) Post-processing output file
---------------------------------------
*Add fake predicted columns:
	Search (EditPad Pro 7 RegExp):
	 ^([0-9]+\t[^\t]+\t)([^\t]+\t)[^\t]+\t([^\t]+\t)[^\t]+\t([^\t]+\t)[^\t]+\t([^\t]+\t)[^\t]+\t([^\t]+\t)[^\t]+\t([^\t]+\t[^\t]+)$
	Replace by:
	 $1$2$2$3$3$4$4$5$5$6$6$7

*Save as UTF-8 without BOM.


-----------
References
-----------
* There is no paper yet that describes this tool.

* More about the differences between SSynt and DSynt

Simon Mille, Alicia Burga, and Leo Wanner. AnCora-UPF: A multi-level annotation of Spanish. In Proceedings of the 2nd International Conference on Dependency Linguistics (DepLing), 217-226, Prague, Czech Republic, 2013.
http://www.aclweb.org/anthology/W/W13/W13-37.pdf#page=225

* Examples of uses of the parallel SSynt-DSynt corpora in some applications:

Miguel Ballesteros, Bernd Bohnet, Simon Mille, and Leo Wanner. 2015. Data-driven deep-syntactic parsing. In Natural Language Engineering, 1-36. Cambridge University Press. 
http://journals.cambridge.org/action/displayAbstract?fromPage=online&aid=9914630&fileId=S1351324915000285

Miguel Ballesteros, Bernd Bohnet, Simon Mille, Leo Wanner. 2015. Data-driven sentence generation with non-isomorphic trees. In Proceedings of NAACL-HLT, 387-397, Denver, CO, USA. 
http://www.aclweb.org/anthology/N15-1042

Bernd Bohnet, Simon Mille, Benoît Favre and Leo Wanner. 2011. StuMaBa: From Deep representation to Surface. In Proceedings of ENLG’11, Surface-Generation Shared Task, 232-235. Nancy, France.
https://aclweb.org/anthology/W/W11/W11-2835.pdf


------------------
 Grammar versions
------------------
<b>v0.6</b>

!!!USED FOR DEEP PARSING EXPERIMENTS IN 2014-2015!!!

Updates with respect to v0.5:
* unfixed coordinations so as to keep a better alignment of SSynt and DSynt nodes.
* fixed ID issues: now (hopefully) all SSynt nodes (but punctuations) have an idx in DSynt
* There is still a significant amount of duplicated relations in the structures.
* PTB:
  - train: 711,543 nodes
  - eval: 42,480 nodes

<b>v0.5</b>

Updates with respect to v0.4:
* fixed coordinations: now there's always a coordinating conjunction between two conjuncts (before conjuncts were directly linked by COORD if there was a comma in the sentence)
* PTB:
  - train: 714,791 nodes
  - eval: 42,620 nodes

<b>v0.4</b>

Updates with respect to v0.3:
* we now remove all governed prepositions that we extracted automatically from PropBank and NomBank (around 12,000 predicates and 10,000 prepositions concerned), using disambiguated predicates (for "open.01", we check "open.01" in the dico; for "open.02", we check "open.02", etc.).
* if a predicate is not found in the dictionary (i.e. if it is not disambiguated in the treebank (e.g. "open") or if there is no entry for it with a sense ID (e.g. "open.27")), we remove the prepositions of the sense ID "01" of the corresponding entry (e.g. "open.01" in the case of both examples).
* PTB:
  - train: 711,491 nodes
  - eval: 42,467 nodes

<b>v0.3</b>

Updates with respect to v0.2:
* we now remove some governed prepositions that we extracted automatically from PropBank and NomBank (around 12,000 predicates and 10,000 prepositions concerned).
* we don't look at disambiguated predicates, and always remove the prepositions of the sense ID "01" of a word (e.g. for "open", we check "open.01" in the dico).
* PTB:
  - train: 714,084 nodes
  - eval: 42,619 nodes

<b>v0.2</b>

Updates with respect to v0.1:
* we also remove governed prepositions for the 150 most frequent predicates from PropBank and NomBank (extracted manually).

<b>v0.1</b>

For this version of the corpus, we removed auxiliaries, determiners, THAT complementizers, and TO infinitive markers.
* PTB:
  - train: 734,365 nodes
  - eval: 43,795 nodes

----------
 GENERAL
----------
All SSynt trees are kept as in original annotation (train: 958,167 nodes)
