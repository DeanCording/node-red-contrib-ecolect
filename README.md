# node-red-contrib-ecolet
A Node Red node that is a wrapper around the [Ecolect](https://www.npmjs.com/package/ecolect) natural language matching library.

This node takes a plain text string and tries to match it to one of the configured intents using fuzzy pattern matching of training phrases.  The node can also extract values from the string such as numbers, dates, times, and sub-strings.

Messages for each topic are sent on separate outputs so that they can be
processed by a flow specific to that topic.  The last output is always for messages
that the processor could not recognise.

The output message contains the matched topic and the extracted values. All other message properties are passed through unchanged.</p>

## Configuration

The node configuration provides the information to train the natural language matcher
about the topics it should attempt to match.  The configuration for each topic consists of:

 - Topic - the name of this topic
 - Values - the *name* and *type* of values to be extracted from the message.
 - Phrases - a list of phrases that would match this topic.  Values to be extracted from the phrase are identified by enclosing the value name in {} brackets.

The natural language matcher uses fuzzy logic to determine the match between the message
text and the specified phrases to identify the best matching topic. Simple variations of
specified phrases should be matched but the more phrases that are specified the better
the matching.  If you use enumerations for values that have a small set of known values, then 
the matching will be much better.


## Example

 - **Topic** switch
 - **Values**
    - room - *enumeration* ("bedroom", "kitchen", "study", "kids bedroom")
    - item - *enumberation* ("light", "fan", "radio", "heater")
    - state - *boolean*
    - when - *date-time*
  - **Phrases**
    - turn {item} {state} in {room} at {when}
    - turn {state} {room} {item}
    - turn {room} {item} {state}

Matches the following phrases:
  - turn on kitchen light at 9pm
  - turn on bedroom light
  - turn bedroom light off

