# 7-Bit Coded Character Set Encoding

## Preface

* Date: 14/11/2023
* References:
  * [ISO/IEC 646:1991: ISO 7-Bit Coded Character Set for Information Interchange](https://www.iso.org/standard/4777.html)
  * [ISO/IEC 6429:1992: Control functions for coded character sets](https://www.iso.org/standard/12782.html)
  * [Standard ECMA-6: 7-Bit Coded Character Set 6th Edition](https://ecma-international.org/publications-and-standards/standards/ecma-6/)
  * [Standard ECMA-48: Control Functions for Coded Character Sets](https://ecma-international.org/publications-and-standards/standards/ecma-48/)

## Specification

### Structure

The coded character set of the standard shall have the following structure.

* a **C0** control character set of up to **32** control characters;
* the graphic character SPACE (**SP**);
* a **G0** graphic character set of up to **94** graphic characters;
* the character DELETE (**DEL**).

### Control Characters

The C0 set of a version of Standard ECMA-6 shall be a sub-set of the C0 set of Standard ECMA-48. The selected control functions shall be used according to their definitions in Standard ECMA-48. Bit combinations corresponding to control functions not selected shall be declared unused.

If the C0 set of the International Reference Version (IRV) is used, it shall be the C0 set of Standard ECMA-48.

Table below shows, for convenience, the C0 set of ISO 6429. For the definition of these control functions see Standard ECMA-48.

|Symbol|Coded Representation|Hex Code|
|:-:|:-: |:-: |
|NUL|0/0 |0x00|
|SOH|0/1 |0x01|
|STX|0/2 |0x02|
|ETX|0/3 |0x03|
|EOT|0/4 |0x04|
|ENQ|0/5 |0x05|
|ACK|0/6 |0x06|
|BEL|0/7 |0x07|
|BS |0/8 |0x08|
|HT |0/9 |0x09|
|LF |0/10|0x0A|
|VT |0/11|0x0B|
|FF |0/12|0x0C|
|CR |0/13|0x0D|
|SO |0/14|0x0E|
|SI |0/15|0x0F|
|DLE|1/0 |0x10|
|DC1|1/1 |0x11|
|DC2|1/2 |0x12|
|DC3|1/3 |0x13|
|DC4|1/4 |0x14|
|NAK|1/5 |0x15|
|SYN|1/6 |0x16|
|ETB|1/7 |0x17|
|CAN|1/8 |0x18|
|EM |1/9 |0x19|
|SUB|1/10|0x1A|
|ESC|1/11|0x1B|
|IS4|1/12|0x1C|
|IS3|1/13|0x1D|
|IS2|1/14|0x1E|
|IS1|1/15|0x1F|

### Character **SPACE**

The acronym of the character SPACE is SP and it is represented by bit combination **2/0** (0x20).
This character is a graphic character, it has a visual representation consisting of the absence of a graphic symbol.

### Graphics Characters

All graphic characters shall be **spacing characters**, that is, they cause the active position to advance by one character position.

#### Unique Graphic Character Allocations

|Graphic Symbol|Name|Coded Representation|Hex Code|
|:-:|:-|:-:|:-:|
|!|EXCLAMATION MARK|2/1|0x21|
|"|QUOTATION MARK|2/2|0x22|
|%|PERCENT SIGN|2/5|0x25|
|&|AMPERSAND|2/6|0x26|
|'|APOSTROPHE|2/7|0x27|
|(|LEFT PARENTHESIS|2/8|0x28|
|)|RIGHT PARENTHESIS|2/9|0x29|
|*|ASTERISK|2/10|0x2A|
|+|PLUS SIGN|2/11|0x2B|
|,|COMMA|2/12|0x2C|
|-|HYPHEN-MINUS|2/13|0x2D|
|.|FULL STOP|2/14|0x2E|
|/|SOLIDUS|2/15|0x2F|
|0|DIGIT ZERO|3/0|0x30|
|1|DIGIT ONE|3/1|0x31|
|2|DIGIT TWO|3/2|0x32|
|3|DIGIT THREE|3/3|0x33|
|4|DIGIT FOUR|3/4|0x34|
|5|DIGIT FIVE|3/5|0x35|
|6|DIGIT SIX|3/6|0x36|
|7|DIGIT SEVEN|3/7|0x37|
|8|DIGIT EIGHT|3/8|0x38|
|9|DIGIT NINE|3/9|0x39|
|:|COLON|3/10|0x3A|
|;|SEMICOLON|3/11|0x3B|
|<|LESS-THAN SIGN|3/12|0x3C|
|=|EQUALS SIGN|3/13|0x3D|
|>|GREATER-THAN SIGN|3/14|0x3E|
|?|QUESTION MARK|3/15|0x3F|
|A|LATIN CAPITAL LETTER A|4/1|0x41|
|B|LATIN CAPITAL LETTER B|4/2|0x42|
|C|LATIN CAPITAL LETTER C|4/3|0x43|
|D|LATIN CAPITAL LETTER D|4/4|0x44|
|E|LATIN CAPITAL LETTER E|4/5|0x45|
|F|LATIN CAPITAL LETTER F|4/6|0x46|
|G|LATIN CAPITAL LETTER G|4/7|0x47|
|H|LATIN CAPITAL LETTER H|4/8|0x48|
|I|LATIN CAPITAL LETTER I|4/9|0x49|
|J|LATIN CAPITAL LETTER J|4/10|0x4A|
|K|LATIN CAPITAL LETTER K|4/11|0x4B|
|L|LATIN CAPITAL LETTER L|4/12|0x4C|
|M|LATIN CAPITAL LETTER M|4/13|0x4D|
|N|LATIN CAPITAL LETTER N|4/14|0x4E|
|O|LATIN CAPITAL LETTER O|4/15|0x4F|
|P|LATIN CAPITAL LETTER P|5/0|0x50|
|Q|LATIN CAPITAL LETTER Q|5/1|0x51|
|R|LATIN CAPITAL LETTER R|5/2|0x52|
|S|LATIN CAPITAL LETTER S|5/3|0x53|
|T|LATIN CAPITAL LETTER T|5/4|0x54|
|U|LATIN CAPITAL LETTER U|5/5|0x55|
|V|LATIN CAPITAL LETTER V|5/6|0x56|
|W|LATIN CAPITAL LETTER W|5/7|0x57|
|X|LATIN CAPITAL LETTER X|5/8|0x58|
|Y|LATIN CAPITAL LETTER Y|5/9|0x59|
|Z|LATIN CAPITAL LETTER Z|5/10|0x5A|
|_|LOW LINE|5/15|0x5F|
|a|LATIN SMALL LETTER A|6/1|0x61|
|b|LATIN SMALL LETTER B|6/2|0x62|
|c|LATIN SMALL LETTER C|6/3|0x63|
|d|LATIN SMALL LETTER D|6/4|0x64|
|e|LATIN SMALL LETTER E|6/5|0x65|
|f|LATIN SMALL LETTER F|6/6|0x66|
|g|LATIN SMALL LETTER G|6/7|0x67|
|h|LATIN SMALL LETTER H|6/8|0x68|
|i|LATIN SMALL LETTER I|6/9|0x69|
|j|LATIN SMALL LETTER J|6/10|0x6A|
|k|LATIN SMALL LETTER K|6/11|0x6B|
|l|LATIN SMALL LETTER L|6/12|0x6C|
|m|LATIN SMALL LETTER M|6/13|0x6D|
|n|LATIN SMALL LETTER N|6/14|0x6E|
|o|LATIN SMALL LETTER O|6/15|0x6F|
|p|LATIN SMALL LETTER P|7/0|0x70|
|q|LATIN SMALL LETTER Q|7/1|0x71|
|r|LATIN SMALL LETTER R|7/2|0x72|
|s|LATIN SMALL LETTER S|7/3|0x73|
|t|LATIN SMALL LETTER T|7/4|0x74|
|u|LATIN SMALL LETTER U|7/5|0x75|
|v|LATIN SMALL LETTER V|7/6|0x76|
|w|LATIN SMALL LETTER W|7/7|0x77|
|x|LATIN SMALL LETTER X|7/8|0x78|
|y|LATIN SMALL LETTER Y|7/9|0x79|
|z|LATIN SMALL LETTER|7/10|0x7A|

#### Alternative Graphic Character Allocations

|Graphic Symbol|Name|Coded Representation|Hex Code|
|:-:|:-|:-:|:-:|
|\#|NUMBER SIGN|2/3|0x23|
|£|POUND SIGN|2/3|0x23|
|$|DOLLAR SIGN|2/4|0x24|
| (O~O...?)|CURRENCY SIGN|2/4|0x24|

Unless otherwise agreed between sender and recipient, the graphic symbols £, $ and  do not designate the currency of a specific country.

#### National or Application-Oriented Graphic Character Allocations

No specific graphic character is allocated to the ten bit combinations **4/0, 5/11 to 5/14, 6/0 and 7/11 to 7/14**. These bit combinations are available for national or application-oriented use. Either a unique graphic character shall be allocated to each of these bit combinations, or the bit combination shall be declared unused.

### Character DELETE

The acronym of the character DELETE is DEL and it is represented by bit combination **7/15** (0x7F). DEL was originally used to erase or obliterate an erroneous or unwanted character in punched tape. DEL may be used for media-fill or time-fill. DEL characters may be inserted into, or removed from, a data stream without affecting the information content of that stream, but such action may affect the information layout and/or the control of equipment.

### Composite Graphic Characters

Whilst all graphic characters specified in this International Standard are spacing characters, it is possible, by using BACKSPACE or CARRIAGE RETURN to image two or more graphic characters at the same character position.

For example, SOLIDUS and EQUALS SIGN may be combined to image "not equals". The character LOW LINE, that may be used as a free-standing character, may also be associated with other character(s) to represent the graphic rendition "underlined".

Diacritical marks may be allocated to the bit combinations specified in [National or Application-Oriented Graphic Character Allocations](#national-or-application-oriented-graphic-character-allocations) and be available for composing accented letters. For such composition a sequence of three characters, the first or last of which is the letter to be accented and the second of which is BACKSPACE may be used. Furthermore, QUOTATION MARK, APOSTROPHE or COMMA can be associated with a letter by means of BACKSPACE for the composition of an accented letter with a diaeresis, an acute accent or a cedilla, respectively.

### International Reference Version (IRV)

|Graphic Symbol|Name|Coded Representation|Hex Code|
|:-:|:-|:-:|:-:|
|#|NUMBER SIGN|2/3|0x23|
|$|DOLLAR SIGN|2/4|0x24|
|@|COMMERCIAL AT|4/0|0x40|
|[|LEFT SQUARE BRACKET|5/11|0x5B|
|\ |REVERSE SOLIDUS|5/12|0x5C|
|]|RIGHT SQUARE BRACKET|5/13|0x5D|
|^|CIRCUMFLEX ACCENT|5/14|0x5E|
|`|GRAVE ACCENT|6/0|0x60|
|{|LEFT CURLY BRACKET|7/11|0x7B|
|\||VERTICAL LINE|7/12|0x7C|
|}|RIGHT CURLY BRACKET|7/13|0x7D|
|~|TILDE|7/14|0x7E|