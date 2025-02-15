# ![](https://0000.us/klatchat/app/files/neon_images/icons/neon_paw.png) Custom Conversations
Create your own or use text script files shared by other users.

## About
Skill, which works using the custom text parsing implementation, provides the functionality to create, share, modify, and use any script files obtained from the shared library.

## Examples
* "Tell me available script files"
* "What scripts are available"
* "Update my conversations"
* "Update my skill files"
* "Set my startup script to"

## Credits
[reginaneon](https://github.com/reginaneon) [NeonDaniel](https://github.com/neondaniel) [neongeckocom](https://neongecko.com/)

## Category
**Configuration**

## Tags
#Neongecko
#Neon
#CustomConversations
#Custom

## Requirements  
  
No special required packages for this skill.  
  

## How to Use

Scripts update automatically when Neon is started; you can start a script by saying:
    
- Run my (script name) skill file
    
If the script has any synonyms specified, you may also use a synonym to start the script.

If the script has any defined [tags](#tag), you can start the script at the tag by saying:
    
- Run my (script name) skill file at (tag name)

You may also update scripts:

- Update my scripts

You can request an emailed copy of a script:
    
- Email me my (script name) script

## What are scripts?  
Scripts are user-constructed text files that contain various Neon commands. 
Using a few simple keywords, described below in the detail, you can specify exactly what Neon should say, do, repeat, 
and answer; you can create new dialogs, routines, loops, and query lookups, while utilizing every skill, 
YAML variable, and other information that Neon knows.   
  

For example:  
  
    Script: "Demo Hello World Input"
    Variable: input
    Neon speak: "Hello World. Say anything or exit"
    voice_input(input)
    Neon speak: "you said {input}"
    Exit
    
Scripts are easily sharable between other users via the Neongecko Scripts Library. You can submit your script there to 
share with all the other Neon users.

You can also find various demo scripts there if you need somewhere to start or something to reference.

## Script Syntax
Command syntax in general includes a Command and some command arguments, in the form:

    Command: argument1, argument2
    
Multiple lines may be assigned to the same command if they are indented 4 spaces from the command declaration. An example
of this with multiple spoken lines is:

    Neon Speak: "This is the first line I will speak."
        "I will also speak this line!"
        
Parentheses are used for some commands to accept parameters, for example voice_input:

    voice_input(var_to_fill)
    
Braces are used to return variable values within a literal string, for example in a speak statement:

    Neon Speak: "You said: {var_to_fill}"

## Starting a Script File
Scripts must begin with a `Script: ` line containing the script name and then any optional 
[Variable](#variable), [Claps](#claps), [Language](#language), and [Synonym](#synonym) lines.
After this, you can continue with any of the other script commands. It is recommended to include a 
[Neon Speak](#neon-speak) statement to tell the user what they can do/how to proceed prior to requesting any 
[user input](#voice_input).

    Script: "Demo Weather Time Population"
    Description: "A Simple example script that can offer the weather, time, and population of different locations."
    Author: Neongecko
    Language: en-au, female
    Variable: response
    Synonym: "WTP"


#### Language
This sets the language Neon will use for script responses while the script is active. This will not affect a user's 
profile or responses they get from Neon outside of the script. The valid language codes are:

zh-zh, da-dk, nl-nl, en-au, en-gb, en-in, en-us, en-gb-wls, fr-fr, fr-ca, de-de, hi-in, is-is, it-it, ja-jp, ko-kr, nb-no,
pl-pl, pt-br, pt-pt, ro-ro, ru-ru, es-es, es-mx, es-us, sv-se, tr-tr, cy-gb, cmn

Any of the above language codes may also be used without the region code (i.e. "en" is equivalent to "en-us"). "male" or 
"female" may also be specified either before or after the language code; the default gender is "female".

    Language: en-us, female

#### Description
This is an optional short description of the script.

#### Author
This is an optional author credit.

#### Timeout
This is an optional parameter to handle situations where a user hasn't responded for a specified period of time. The first 
argument is the duration in seconds to wait for a response (max 3600). The second parameter is interpreted as a [goto](#goto) 
and can be a line number or tag.

Go to the named tag "example" after 10 seconds of inactivity:
```
Timeout: 10, example
...
@example
# Code here will be executed after 10 seconds of inactivity
```

Go to line 7 after 30 seconds of inactivity:
```
Timeout: 30, 7
...
Neon speak: Are you still there?  # If this is line 7 of the script file, this is spoken after 30s of inactivity
```

#### Variable
A variety of parameters to be used later in the script. Can be preset or empty. All variables will be saved as a list
with the most recent value at index 0 and previous values appended. [table_scrape](#table_scrape) 
will build a dictionary of named links as options and save that at the front of the list. Variables
may occupy more than one line; subsequent lines should be indented from the `Variable:` line. Variables used for 
[simple word substitutions](#sub_values) will contain pairs of strings where each pair is separated by a comma. 
Variables used for [conversational responses](#sub_key) will contain multiple quoted string sets with each set 
separated by a comma. 

Simple Variable declarations:
```
    Variable: from_units
    Variable: conversions = weight, volume, length, time, currency
    Variable: options = table_scrape(https://www.neongecko.com/demos)
```
Word Substitutions:
```
Variable: input_sub = dont don't,
    cant can't,
    wont won't,
    recollect remember
```
Conversational Responses:
```
Variable: key_sub = "[sorry] for *" "Please don't apologize about *" "No need to apologize about *",
    "i remember when {1} said {2}" "Do I think of {1} or {2} often?",
    "*" "I don't understand. Please elaborate." " What do you mean by *?"
```
#### Synonym
Specifies a phrase that can be used to start this skill file. After the script is run the first time, the synonym will 
be added to the user's synonyms. After this, the synonym can be used to run the script. For example: 

    Case: "Run My TPW Skill File"  
    Synonym: "T P W 2"

#### Claps
The number of claps that should be associated with a command while the script is active.

    Claps: 2, "what time is it"  

## Script Keywords and Spacing
Neon scripts follow the Python convention of 4 spaces to indent subordinate lines. A line without a command will be considered
a subordinate of the previous line that has one fewer indent; for example, all of the lines below after `Neon speak:` 
would be spoken:

    Neon speak:
        "Say 1 or World Times for world times"
        "Say 2 or World Weather for world weather"
        "Say 3 or World Populations for world populations" 

There are multiple keywords available and new ones are added frequently. The current list, starting with the core example above:

#### Neon speak
Have Neon say something. A single line to speak can be on the same line as `Neon speak:`. If multiple lines 
are to be spoken, they should follow `Neon speak:` and be indented. SSML is supported in `Neon speak` and `Name speak` 
commands if they are supported by the selected TTS engine. Examples of ssml supported by Amazon Polly can be found 
[here](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html).

```
Neon speak: "Hello World. Say anything or exit"
```
```
Neon speak:
    "Say 1 or World Times for world times"
    "Say 2 or World Weather for world weather"
    "Say 3 or World Populations for world populations"
```

#### Name speak
Have Neon say something with the specified name. Name is required, gender and language may optionally be specified 
as comma-separated parameters. If one of gender or language are specified, the other will use the user's profile setting 
or script setting if available. SSML is supported in `Neon speak` and `Name speak` 
commands if they are supported by the selected TTS engine. Examples of ssml supported by Amazon Polly can be found 
[here](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html). SSML is supported in `Neon speak` and `Name speak` 
commands if they are supported by the selected TTS engine. Examples of ssml supported by Amazon Polly can be found 
[here](https://docs.aws.amazon.com/polly/latest/dg/supportedtags.html).

```
Name Speak: Nobody, "Or I can speak as someone else."
```
```
Name Speak: Slow Male, male, "<prosody volume="-2dB" rate="x-slow" pitch="x-low">I can be quiet, slow, and deep</prosody>"
```
```
Name Speak: English Male, male, en-us, "<prosody volume="-2dB" rate="x-slow" pitch="x-low">I can be quiet, slow, and deep</prosody>"
```

#### Reconvey
Reconvey can be used in two ways: one in which a user's prior input to the script is used, and another in which specified
audio files are played back.

In the first method, Neon plays back a user's original audio input (if available) 
and prints the transcription. The last value of the passed variable is used with the last audio_file available for that 
variable. No Text-To-Speech will be generated, so nothing will be played if there is no input audio to use.

*Note: The variable named should not be enclosed in braces unless its value is the name of the variable to be spoken* 
```
voice_input(var_to_use)

...

Reconvey: var_to_use
```

In the second method, a quoted text string or variable is provided along with a specified audio file. The audio file may be a 
public URL, an absolute path to a local file, or the filename of a file saved in `script_audio/{script title}/` within 
the skill directory.
```
Reconvey: var_to_use, "https://my_website/files/audio_to_play.mp3"
```
```
Reconvey: "This literal will be printed", "audio_file_in_script_directory.wav"
```
```
Reconvey: "Literal to print", "~/Music/audio-file.mp3"
```

#### Execute 
String to be executed as if spoken by a user. A single line to execute can be on the same line as `Execute:`. 
If multiple lines are to be executed, they should follow `Execute:` and be indented. When a string is executed, Neon 
will wait several seconds for a response before continuing. *Any commands will be executed as if a wake word was heard,
so "Neon" is not required in front of any commands*.

```
Execute: "What time is it in Athens"
```

```
Execute:
    "translate cherry to russian"
    "tell me my language setting
    "speak to me in english
    "speak to me in Russian and French
    "speak to me in english
```

#### Tag
Lines with `@` as the first non-whitespace character will be indexed as Goto tag lines. A tag line should be one word 
and contain only a string label.

```
@test_tag
```

####Goto
Command to go to the specified line or [tag](#tag) in the script. The argument to this command can be either a file line
number (the line specified will be executed next) or a [tag](#tag) that is defined in the script (the line following the
tag will be executed next). Tags may reference a line at any position in the file. *When writing a script with Goto 
commands, avoid entering your script at [case](#case) option lines and consider where variables are set to avoid using
a variable before setting it (i.e. with [voice_input](#voice_input).*

```
Goto: test_tag
```
```
Goto: 5
```

#### Comments
Lines with `#` as the first non-whitespace character will not be executed. Comment lines are useful for annotating a script
or removing problematic lines when troubleshooting.
(ex. you remove a line and leave a comment for why the line is commented out). Block comments are also allowed where 
blocks start and end with `"""`.

```
# Removed speak to troubleshoot voice_input
    # Neon speak:
    #     "Say your first name or exit" voice_input{first_name}
```
```
"""
    This is inside a block comment.
You can put in longer comment strings here, or surround a section of code
"""
```

#### voice_input
Specifies when a variable needs to be filled with user input. If an optional list of options is provided, the script will 
wait for user input to match one of those options, otherwise the next user input will be used. Once the variable has 
been assigned a value, the script will continue at the following line. Variables will preserve a history of previous 
values in a list.
*It is recommended that any voice_input follows a `Neon speak:` prompting the user to say something.*

```
    Variable: input
    Neon speak: "Hello World. Say anything or exit"
    voice_input(input)
    Neon speak: "you said {input}"
```
```
    Neon speak:
        "Say 1 or World Times for world times"
        "Say 2 or World Weather for world weather"
        "Say 3 or World Populations for world populations"
    voice_input(response)
    Case {response}:
```
```
Variable: selected
Variable: conversions = weight, volume, length, time, currency
Neon speak: "Say convert {select_one(conversions)} to convert or exit when done"
voice_input(selected,conversions)
```

#### If/Else
If statements can be used to evaluate variable values. If a comparator is not given, the variable will be evaluated as a 
boolean, where values of `0`, `false`, `none`, `null`, `no`, and `""` are false and any other value is true. 
Valid numeric comparators are: `>` ,`<`, `>=`, `<=`, `==`, `!=`.
Valid string/list comparators are: `CONTAINS`, `IN`, `STARTSWITH`, `ENDSWITH` and their inverses when prefixed with `!`
 (i.e. `!CONTAINS`). Variables may be compared to other variables 
or to static values. For string/list comparators, the first argument will be treated as a string, and the second
will be treated as a list of strings. 
`CONTAINS` returns true if the left value (string) contains any element the right value (list). 
`IN` returns true if an exact match of the left value (string) exists in the right value (list).
`STARTSWITH` returns true if the left value (string) begins with any element in the right value (list).
`ENDSWITH` returns true if the left value (string) ends with any element in the right value (list).
Strings passed to numeric comparisons follow 
[Python3 rules](https://docs.python.org/3/tutorial/datastructures.html#comparing-sequences-and-other-types).

```
Variable: {test}: "False"

if {test}:
    Neon speak: "True"
else:
    Neon speak: "False"
```

```
Neon speak: "Say 'continue' to continue"
voice_input{test}

if {test} == "continue":
    Neon speak: "Okay, lets continue"
else:
    Neon speak: "Okay, exiting."
```

```
Neon speak: "Say {conditional} to continue"
voice_input(test)

if {test} != {conditional}:
    Neon speak: "Not matched, exiting"
    Exit
else:
    Neon speak: "Okay, lets continue."

```
```
if {input} CONTAINS "help":
    # This block is executed if input contains the word 'help'
```
```
if {input} CONTAINS help, assist, what:
    # This block is executed if input contains any of the words: "help", "assist", "what"
```
#### Case
Case statements can be used to execute different commands based on some variable value. Case statements often follow a 
voice_input to assign a variable value, but may also use a variable that has already been defined. Case options should
follow the `Case:` line and be indented from that line by one. Commands to run if the case option is satisfied should be
indented from that case option. A `Case:` will execute the first matched option and then continue at the next line 
following the end of the case; if no option is matched, the script will go back to the voice_input line immediately 
before the case. Case options containing "or" will compare the Case variable against the string on either side of "or".

```
Neon speak:
    "Say A or Athens for Athens's time"
    "Say B or Bombay for Bombay's time"
    "Say S or Seattle for Seattle's time"
voice_input(response)
Case {response}:
    "A or Athens"
        Execute: "What time is it in Athens"
    "B or Bombay"
        Execute: "What time is it in Bombay"
    "S or Seattle"
        Execute: "What time is it in Seattle"
# Script continues here
Exit
```

#### Python
Any line of python 3 readable code will be executed in the order it was specified in the script file. These lines will 
execute and then continue to the next line of the script; you may assign the output of a valid Python operation to a 
script variable to use later in your script. The `time` module is available in addition to any builtin methods.

Currently, the following math operations are supported: `+`, `-`, `/`, `*`,
`%` (modulus), `**` (power), `ln(x)`, `log(x)` (log base 10), `sqrt(x)`, and common trigonometry functions (sin, cos, 
tan, sinh, cosh, tanh, asin, acos, atan). The constants `e` and `pi` are also available.

```
Python: time.sleep(5)
```
```
Python:
    hypotenuse = sqrt({side_1}**2 + {side_2}**2)
```

#### Exit
Exit command to finish the script. Could be positioned anywhere in the document.
*Note: your script should always exit somewhere, this is often simply on the last line and indented by 0.*

    Exit  

#### Loop
Loops are defined by a starting line and an ending line with optional conditions. A loop begins where `LOOP` is declared 
with a name and ends where a `LOOP END` or `LOOP UNTIL` line is reached. A loop can always be exited when the user says 
"exit", otherwise the loop will run until the `UNTIL` condition is met or indefinitely if no `UNTIL` condition is met.
Provides familiar functionality as loops. Can be nested and combined with other statements. Positioned at the beginnings
and end of a [Case](#case) statement. A loop is started with the keyword `LOOP` followed by a name and terminated by 
name with the keyword `END` as noted below.

No end condition defined, this loop will continue until the user says "exit".
```
LOOP WW
    Neon speak:
        "Say A or Athens for the weather in Athens"
        "Say B or Bombay for the weather in Bombay"
        "Say S or Seattle for the weather in Seattle"
        "Say exit to exit"
    voice_input(location)
    Case {location}:
        "A or Athens"
            Execute: "What is the weather in Athens"
            Neon speak: "about to repeat A"
        ...
     Neon speak:
         "End of loop WW."
LOOP WW END
```


A loop until a specified variable has been filled in with the defined values:

```
LOOP WW
    Neon speak:
        "Say A or Athens for the weather in Athens"
        "Say B or Bombay for the weather in Bombay"
        "Say S or Seattle for the weather in Seattle"
        "Say exit to exit"
    voice_input(location)
    Case {location}:
        "A or Athens"
            Execute: "What is the weather in Athens"
            Neon speak: "about to repeat A"
        ...
     Neon speak:
         "End of loop WW."
LOOP WW UNTIL location == "b"
```

#### Set
You may set variables equal to other variable values or static values with the keyword `Set` anywhere in a script. This 
can be useful to save a variable value before modifying the variable.

        Set: user_speech = {input}
        
If this command is not indented from the previous line, it may be inferred

        Neon Speak: "Setting variable"
        user_speech = {input}

#### Email
You may draft and send an email to the user running a script if their email is available. The title may be a quoted literal 
or a variable reference. The body may be a variable or a string literal.

    Email: "My Email Title", variable_body

#### Run
You may execute a different script from within your script. Users will be notified when the requested script is started 
or if a requested script isn't available. Upon exit from the script called via `Run`, the original script will resume at 
the following line and continue normally. Variables from the calling script will be available to the called script if 
that script does not have a variable of the same name defined or the variable is defined with no value.
When the original script is resumed, any defined variables are restored to the same state they were in prior to the `Run` command.

    Run: demo guess number

#### sub_values
Accepts a variable string to modify and a list variable containing substitution pairs. Any word matched to the first word of a 
substitution pair will be replaced with the second word of that pair in the passed variable.

```
Variable: input_sub = dont don't,
    cant can't,
    wont won't,
    recollect remember
...
sub_values(input, input_sub)
```

#### sub_key
sub_key is a simplified method for matching a user's input to a pattern and then generating a response associated with 
that pattern. sub_key accepts a string variable to match and a list variable containing `strings to match` and `associated 
responses`. Wildcards (`*`) and new variables (`{var_1}`) may be used in the `strings to match` and the `associated 
responses` to include parts of the input in the response. Multiple wildcards will be evaluated positionally 
(i.e. the first wildcard in the input will be used as the first wildcard in the output). sub_key executes such that the 
first matched response in the `associated responses` list will be used; this allows a catch-all wildcard to be used at 
the end of the list to handle any input that doesn't match a specified pattern. Bracketed strings (`[string]`) reference 
a list variable so that any word in the variable is accepted as a match. *Note: wildcard variables are only available for
the current substitution; named variables are available for use later in the script until overwritten.*

```
Variable: sorry = sorry, i apologize
Variable: key_sub = "[sorry] for *" "Please don't apologize about *" "No need to apologize about *",
    "i remember when {1} said {2}" "Do I think of {1} or {2} often?",
    "*" "I don't understand. Please elaborate." " What do you mean by *?"
...
sub_key(input, key_sub)

```
### Functions (Used for variable assignment or inline text substitution):
The following functions accept one or more arguments in parentheses and return values that can be assigned to a variable
or, in some cases, a value that can be substituted within a quoted string.

#### select_one
Specifies that the value will have to be filled in by user's choice from the provided list of items before proceeding. 
Used in [Neon speak](#neon-speak) statements to speak the available options for a variable.

    Variable: conversions = weight, volume, length, time, currency  
    Neon speak:  
    "Say convert {select_one(conversions)} to convert or exit when done"

#### table_scrape
Uses beautiful soup to give back a readable and searchable dictionary of text/link pair of any HTML table element 
on a provided web page. 

    Variable: options = table_scrape(https://www.neongecko.com/demos)

#### random
Returns random elements of the given list variable. If used to set a [variable](#variable), one value will be assigned; 
in a [Neon speak](#neon-speak), 2-3 examples will be provided and spoken.
```
Variable: numbers = 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
Variable: neon_num = random(numbers)
```
```
    Variable: options = table_scrape(https://www.neongecko.com/demos)  
    Variable: chosen
    Neon speak:  
        "Please tell me what kind of help video you would like to see. You can say things like {random(options)}"
```

#### closest
Returns the closest element of a list to the specified variable (generally used in combination with table scrape). 
Optimized for string processing.

    Execute: "av play {closest(chosen,options)}"

#### profile
Lookup a value from a user profile to use in a speak or execute command.

    Variable: email = profile(user.email)


#### skill
Call a skill and extract a some specific data from it. You must pass a valid skill intent 
(same as you would to [Execute](#execute)), as well as a valid key from that intent's dialog data. In general, this option 
should only be used if you maintain the required skill and script, as these parameters may change.

    Neon Speak: "It is currently {skill("what is the weather", weather)}"
    
*Note: "weather" is defined in the weather skill dialog for this intent*
 
## How to Use Scripts 

Scripts can be downloaded from Neongecko's library or drafted and added to the script_txt folder in the skill directory.
The demo skill files (found in the library at neongecko.net) and descriptions above provide a summary and examples 
of the available functionality and formatting requirements.


  
## Contact Support  
  
Use the [link](https://neongecko.com/ContactUs) or [submit an issue on GitHub](https://help.github.com/en/articles/creating-an-issue)