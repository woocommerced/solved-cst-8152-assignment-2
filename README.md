Download Link: https://assignmentchef.com/product/solved-cst-8152-assignment-2
<br>
<strong><u>Purpose:</u> Building a Lexical Analyzer (Scanner)</strong>

<h1>Part 1: Regular Expressions, Transition Diagram, Transition Table</h1>




In this course you will have a pleasurable experience to write the front-end of a compiler for a programming language named <strong><em>PLATYPUS</em></strong>. The <strong><em>PLATYPUS</em></strong> informal language specification is given in <strong>PlatypusILS_F18 </strong>document. In order to proceed the informal language specification mat be converted to a formal language specification.  Since <strong><em>PLATYPUS</em></strong> is a simple, yet complete, programming language it can be described formally with a context-free grammar (BNF) notation.




The <strong><em>PLATYPUS</em></strong> grammar will have two parts: a lexical grammar and a syntactic grammar. The lexical grammar will define the lexical part of the language: the character set and the input elements such as white space, comments and tokens. In Part 2 of the assignment you will use the lexical grammar to implement a lexical analyzer (scanner).   The syntactic grammar for <strong><em>PLATYPUS</em></strong> has the tokens defined by the lexical grammar as its terminal symbols. It defines a set of productions. The productions, starting from the start symbol &lt;program&gt;, describe how a sequence of tokens can form syntactically correct <strong><em>PLATYPUS</em></strong> statements and programs. This part of the grammar will be used to implement a syntax analyzer (parser) in one of the following assignments.




You will find the complete lexical grammar and syntactic grammar for the <strong><em>PLATYPUS</em></strong> language in the document <strong>PlatypusBNFGR_F18</strong>. Read very carefully the informal language specification (<strong>PlatypusILS_F18</strong>) and then see how the informal language specification has been converted to a formal specification using a BNF grammar notation.




Part of the Scanner will be implemented using a Deterministic Finite Automaton (DFA) based on a Transition Table (TT) (see below Part 2). In order to create TT you must undergo a few preliminary steps. First, you must convert the lexical grammar into Regular Expressions. Then using the regular expressions you must draw a Transition Diagram (TD). Finally, using the Transition Diagram you can create a Transition Table.




Most of the work is done for you (see <strong>RETDTT_F18</strong> document). In this part you have to write a regular expression for the string literals, complete the TD including the string literal regular expression, and finally modify the TT to accommodate the string literal.




The PLATYPUS language has some design flaws. Try to identify them but do not change the language specification.










<h1>Part 2: Implementing a Lexical Analyzer (Scanner)</h1>




<strong> </strong>

In Part 1, you had the pleasure to read and analyze the grammar for the PLATYPUS programming language. In Part 2, you are to create a lexical analyzer (scanner) for this language. This assignment implements and tests the foundation of your lexical analyzer: <strong>The Scanner</strong>. The scanner reads a source program from a text file and produces a stream of token representations. Actually, the scanner does not need to recognize and produce all the tokens before next phase of the compilation (the parsing) takes action. That is why, in almost all compilers, the scanner is actually a function that recognizes language tokens and produces token representations one at a time when called by the syntax analyzer (the parser).




In your implementation, the input to the lexical analyzer is a source program seen as a stream of characters (symbols) loaded into an input buffer. The output of each call to the Scanner is a single Token, to be requested and used, in a later assignment, by the Parser. You are to use a data structure to represent the Token. Your scanner will be a mixture between transition-table driven (DFA) and token driven scanner. Transitiontable driven scanners are easy to implement with scanner generators. In token-driven scanner every token is processed as a separate exceptional case (exception or case driven scanners). They are difficult for modifications and maintenance (but in some cases could be faster and more efficient). You will take the middle road.




Transition-table driven part of your scanners is to recognize only variable identifiers (including keywords), integer literals (decimal constants), and floating-point literals. To build transition table for those three tokens you have to transform their grammar definitions into regular expressions, and create the corresponding transition diagram(s) and transition table. As you already know, Regular Expressions are a convenient means of specifying certain simple set of strings.




In Part 2 your task is to write a scanner program (set of functions). Three files are provided for you on Brightspace LMS:<strong> token.h</strong>,<strong> table.h</strong>, and<strong> scanner.c</strong> (see <strong>Assignment2PF.zip</strong>). Where required, you have to write a C code that provides the specified functionality. Your scanner program (project) consists of the following components:




<strong>platy_st.c</strong> – The main function. This program and the test files are provided for you on Brightspace in a separate file (<strong>Assignment2MPTF.zip</strong>). .




<strong>buffer.h</strong> – Completed in Assignment 1. It contains buffer structure declarations, as well as function prototypes for the buffer structure. <strong>buffer.c</strong> – Completed. It contains the function definitions for the functions written in Assignment 1. <strong>token.h</strong> – Provided complete. It contains the declarations and definitions describing different tokens. Do not modify the declarations and the definitions. <strong>Do not add anything to that file</strong>.




<strong>table.h</strong> – Provided incomplete. It contains transition table declarations necessary for the scanner. All of them are incomplete. You must initialize them with proper values. It must also contain the function prototypes for the accepting functions. You are to complete this file. You will find the additional requirements within the file. If you need named constants you can add them to that file.




<strong>scanner.c</strong> – Provided incomplete. It contains a few declarations and definitions necessary for the scanner. You will find the additional requirements within the file.

<strong>     </strong>

<strong>      </strong>The definition of the <strong><em>scanner_init() </em></strong>is complete and you must not modify it. The function performs the initialization of the scanner input buffer and some other scanner components.




You are to write the function <strong><em>malar_next_token() </em></strong> which performs the token recognition (<strong><em>malar</em></strong> stands for <strong>m</strong>atch-<strong>a</strong>–<strong>l</strong>exeme-<strong>a</strong>nd<strong>-r</strong>eturn) . It “reads” the lexeme from the input stream (in our case from the input buffer) one character at a time, and returns a token structure any time it finds a token pattern (as defined in the lexical grammar) which matches the lexeme found in the stream of input symbols. The token structure contains the token code and the token attribute. The token attribute can be an attribute code, an integer value, a floating-point value (for the floatingpoint literals), a lexeme (for the variable identifiers and the errors), an offset (for the string literals), an index (for the keywords), or source-end-of file value. The scanner ignores the white space. The scanner ignores the comments as well. It ignores all the symbols of the comment including line terminator. The function consists of two implementation parts: token driven (special case or exception driven) processing and transition table driven processing. You are to write both parts. The tokens which must be processed one by one (special cases or exceptions) are defined above and in <strong>table.h</strong>. You must build the transition table for recognizing the variable identifiers (including keywords), integer literals, floating-point literals, and strings.




The scanner is to perform some rudimentary error handling – error detection and error recovery.




<strong><em><u>Error handling in comments</u></em></strong>. If the comment construct is not lexically correct (as defined in the grammar), the scanner must return an error token. For example, if the scanner finds the symbol <strong>!</strong> but the symbol is not followed by the symbol <strong>!</strong> it must return an error token. The attribute of the comment error token is a C-type string containing the <strong>! </strong>symbol and the symbol following <strong>!</strong>. Before returning the error token the scanner must ignore all of the symbols of the wrong comment including the line terminator.




<strong><em><u>Error handling in strings</u></em>.</strong> In the case of illegal strings, the scanner must return an error token. The erroneous string must be stored as a C-type string in the attribute part of the error token (<strong><em>not in the string literal table</em></strong>). If the erroneous string is longer than 20 characters, you must store the first 17 characters only and then append three dots (…) at the end.




<strong><em><u>Error handling in case of illegal symbols</u></em>.</strong> If the scanner finds an illegal symbol (out of context or not defined in the language alphabet) it returns an error token with the erroneous symbol stored as a C-type string in the attribute part of the token.




<strong><em><u>Error handling of runtime errors</u>.</em></strong> In a case of run-time error, the function must store a non-negative number into the global variable <strong>scerrnum</strong> and return a runtime error token. The error token attribute must be the string “RUN TIME ERROR: “.




The definition of the <strong><em>get_next_state()</em></strong> is complete and you must not modify it.




The function <strong><em>char_class () </em></strong>must return the column index for the column in the transition table that represents a character or character class (for example, [a-zA-Z], [1-9]). You must complete that function.




Additionally, you have to write the definitions of the accepting functions and some other functions (see <em>scanner.c</em>). (You may implement your own functions if needed.)




<strong><u>INPORTANT NOTE</u>:  </strong>

<strong>In the scanner implementation you are not allowed to manipulate directly any of the Buffer structure data members. You must use appropriate functions provided by the buffer implementation. Direct manipulation of data members will be considered a crime </strong>J<strong> against the functional specifications and will render your Scanner non-working.</strong>

<strong> </strong>

<strong><u>INPORTANT NOTE</u>: </strong>

You are allowed (but not required) to work on this assignment in teams. If you decide to work alone, you cannot switch to a team-work later. If you decide to work in a team, be aware that you will be not allowed to change the team later on, but you can dissolve the team and continue working alone on later assignments. A team can have <strong>two</strong> members <strong>only</strong>. Both members must be officially registered in the course and must have submitted their Assignment #1. <strong>By the end of the first week </strong>of the assignment period each team must send me a notification e-mail with the names (first, last) and the student numbers of the team members. Without a proper and timely notification I will not accept any team work. Each team must submit one assignment only. The envelope label and the cover page must contain information about both members as required by the Assignment Submission Standard.

Additionally, on a <strong>separate page</strong> (team page) containing the name and the student ID# of the team members, each of the team members must give a brief description of the work done by her/him on this assignment (including the names of the functions written by the member). The page must be signed by the members of the team. Each and every member must be involved in some coding and testing. Each member must code and test at least three functions (one of them must be an accepting function) and the name of the member must be indicated in the function header. The function <strong><em>malar_next_token() </em></strong>may have two authors.

Be aware that all of the conditions above <strong>must be met</strong> in order to have your assignment accepted and marked.




<strong>What to Submit (Part 1 and Part 2):</strong>




<h1>For Part 1</h1>

<strong> </strong>

Hand in, on paper, the Regular Expressions, the Transition Diagram and the Transition

Table for the DFA implementation of the Scanner. You can modify the

<strong>RETDTT_F18.doc</strong> document or write your own documents. They must have <strong>your name (or team names) on the top of each page</strong>.  Anonymous materials will not be accepted for evaluation.

<strong> </strong>

<h1>For Part 2</h1>




Hand in, on paper:




<ol>

 <li>The fully documented source listing of the modified <strong>c </strong>and<strong> table.h</strong> source files containing the Scanner and its supporting declarations, definitions, and functions.</li>

 <li><strong>The marking sheet for the assignment with your name filled in</strong>.</li>

</ol>




<h1>Digital Drop Box Submission</h1>

<strong><u>Compress</u></strong> into a <strong>zip</strong> file the following files: all of the project <strong>.h</strong> files, and <strong>.c</strong> files, all <strong>.pls</strong> files, and<strong> the output files produced by your program </strong>using the provided standard test files. Include your additional input/output test files if you have any and they are not too large (MB). Upload the <strong>zip</strong> file on Brightspace LMS (BS). The file must be submitted prior or on the due date as indicated in the assignment. The name of the file must be Your Last Name followed by the last three digits of your student number followed by cA2. For example: Lname345_cA2.zip. If your last name is long, you can truncate it to the first 5 letters. <strong>Teams</strong> must submit one .zip file only. The name of the file must contain the names of both members e.g. FLname345_SLname123_cA2.zip. <strong><u>In case of emergency (BS LMS is not working)</u></strong> submit your zip file via e-mail.

Make sure all printed materials are placed into an unsealed envelope and are deposited into the assignment box prior to the end of the due date. If you are late, you must submit the assignment envelope directly to the professor. The submission must follow the course submission standards. You will find the Assignment Submission Standard as well as the Assignment Marking Guide (<strong>CST8152_ASSAMG)</strong> for the Compilers course on the BS LMS.

<strong>Assignments will not be marked if the source files are not submitted on time.</strong>

Assignments could be late, but the lateness will affect negatively your mark: see the Course Outline and the Marking Guide. All assignments must be successfully completed to receive credit for the course, even if the assignments are late.

Enjoy the assignment. And do not forget that:




<em>“There are two kinds of people, those who do the work and those who take the credit. </em>

<em>Try to be in the first group; there is less competition there.”</em>

Indira Gandhi







And remember to remember:




Murphy’s laws (1 and 2)




<ol>

 <li>If anything can go wrong, it will.</li>

 <li>If there is a possibility of several things going wrong, the one that will cause the most damage will be the first one to go wrong.</li>

</ol>




O’Toole’s commentary on Murphy’s laws: “Murphy was an optimist.”




Ginsber’s Theorems

T1. You can’t win.

T2. You can’t break even. T3. You can’t even quit the game.


