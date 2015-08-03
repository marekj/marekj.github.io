---
layout: post
title: Unicode for Testers
---

Before UTF-8 encoding of Unicode Standard we were dealing with 8bit encoding schemes that mapped 256 numbers available in a 1 byte space to characters that needed to be represented.

The range of 0..127 was reserved for ASCII but the upper range up to 255 was used by encoding schemes which decided what characters would be there. If you needed Cyrillic letters you would use 'KOI8-R' encoding or 'ISO-8859-5' or 'Windows-1251'. Polish users needed support for letters with diacritics so they used encoding 'ISO-8859-2' aka Latin-2 or 'Windows-1250'. The encodings were incompatible. Let's say the number 163 signified pound sterling currency sign in Latin-1 `£` but in Latin-2 the same number signified polish letter `Ł`. Or position 216 in Latin-1 was Scandianvian `Ø` but in Latin-5 it was a Russian `и`.

Unicode changed all of that. Each character in any language has a unique number, a codepoint. Unicode extends beyond known lanugage sets and specifies unique numbers to graphical characters. We no longer live in an 8bit world. We now have 1 byte, 2 byte and 3 byte unicode codepoints to test and the default implementation is UTF-8 wich encodes them as 1, 2, 3 or 4 bytes.


## Unicode and UTF-8

Humans use letters. Computers use numbers. To communicate using computers we have to agree what numbers represent which letters.

- [good introductory story](http://coding.smashingmagazine.com/2012/06/06/all-about-unicode-utf8-character-sets/)
- [Unicde 6.3 slide show. 110,122 characters in 217 blocks covering 100 script](http://www.babelstone.co.uk/Unicode/unicode.html)

## Why Unicode?

Unicode is an answer to the limits of 8-bit encodings and their incompatibilities with each other. 8-bit, 1 byte, 256 characters.

Examples of 8bit character-set encoding schemes to illustrate the context fixed by Unicode.

- [ASCII](http://en.wikipedia.org/wiki/ASCII)
    - `A-Z`, `a-z`, `0-9`, punctutation.
    - 95 printable characters, 128 characters total.
    - binary `00000000` to `01111111` (using 7 bits, 8th bit untouched)
    - hex represented as `\x00-\x7F`

- Adding 8th bit [16 parts of ISO-8859](http://en.wikipedia.org/wiki/ISO_8859)
    - Using 8th bit to expand to 256 characters.
    - Each part of the standard tried to accomodate different character sets
    - (Different schemes of stuffing characters in spaces `\xA1-\xFF`)[http://en.wikipedia.org/wiki/ISO_8859#Table]

- [Windows Code Pages](http://en.wikipedia.org/wiki/Windows_code_page#List)

## What is [Unicode](http://unicode.org)?

- Standard for representing characters in all writing systems of the world.
    - Every character in every writing system is given a codepoint and a name
        - codepoint is a hexadecimal number in a format that begins with `U+` and hex string: e.g. `U+0065`
    - Codepoint and character are a pair. Every character has exactly one codepoint: `U+0041` for letter `A`.
- Currently unicode describes over 110,000 characters (codepoints)
- Codepoints are organized into 'blocks'. Blocks are named ranges of codepoints.
    - For practical purposes [it's best to view unicode ranges by 'blocks'](http://en.wikipedia.org/wiki/Unicode_block).
- Unicode organizes codepoints into [17 planes. From Plane 0 to Plane 16. Each plane has 2^16 codepoints](http://en.wikipedia.org/wiki/Mapping_of_Unicode_character_planes)
    - For practical purposes we care about 2 areas of planes
        - Basic: 1 and 2 byte codepoints. (i.e. ASCII and most languages of the world)
        - Astral: 3 byte codepoints (i.e. Emoticons, Pictographs, Transport and Map Symbols.)

Recap: a `codepoint` represents a `character`. A `block` names `codepoint range`. `Block` is located in a Unicode `plane`. `Basic` codepoints are 1 and 2 bytes. `Astral` has 3 byte codepoints.

## What is [UTF-8](http://en.wikipedia.org/wiki/UTF-8)?

**Un**iversal Character Set **T**ransformation **F**ormat **8**-bit

Unicode codepoints standard needs to be implemented. `UTF-8` is a variable-width encoding that can represent every character in the Unicode character set. The Internet speaks UTF-8.

- Jan 2014 [79% of website use UTF-8 encoding](http://w3techs.com/technologies/overview/character_encoding/all)
- Feb 2013 [google blog; over 60% UTF-8](http://googleblog.blogspot.com/2012/02/unicode-over-60-percent-of-web.html)
- May 2008 [w3c blog; almost 30%](http://www.w3.org/blog/2008/05/utf8-web-growth/)

WARNING: codepoint hex number may not be the same as UTF-8 hex number and most likely not in the same byte range. Not to worry. Here is a way to think about it.

Range Breakdown Map

codepoints | 1 byte (0000..007F) | 1 byte (0080..00FF) | 2 byte (0100..07FF)  | 2 byte (0800..FFFF) | 3 byte Astral plane
-----------|-------------------- | ------------------- |--------------------- |---------------------|--------------
utf-8      | 1 byte              | 2 byte £ ß ó        | 2 byte ę Я λ         | 3 byte ☃ ✈ ௸      | 4 byte 💩 🄰 𠀀
8bit world | 1 byte              | 1 byte              | n/a                  | n/a                 | n/a

Testing Issue: There could be issues converting from old world 8bit code page number to a new world utf-8 number. [This is quite well descirbed in this post worth reading](http://coding.smashingmagazine.com/2012/06/06/all-about-unicode-utf8-character-sets/)

Examples:

- Basic: (Plane 0 only)
    - 1 byte codepoint, 1 byte utf-8. (0..127) All original ASCII characters preserved and latin extended
        - "A" U+0041 [Basic Latin block letter](http://www.fileformat.info/info/unicode/char/0041/index.htm)
        - "~" U+007E [Tilda](http://www.fileformat.info/info/unicode/char/007E/index.htm)
    - 1 byte codepoint, 2 byte utf-8. (128..255 range) (WARNING: originally 1 byte encoding 8bit encoding schemes)
        - "£" U+00A3 [Pound Sterling](http://www.fileformat.info/info/unicode/char/00A3/index.htm)
        - "ß" U+00DF [German Sharp S](http://www.fileformat.info/info/unicode/char/00DF/index.htm)
        - "ó" U+00F3 [o with acute](http://www.fileformat.info/info/unicode/char/00f3/index.htm)
    - 2 byte codepoint, 2 byte utf-8
        - "ę" U+0119 [Polish e with ogonek](http://www.fileformat.info/info/unicode/char/0119/index.htm)
        - "Я" U+042F [Russian Ya](http://www.fileformat.info/info/unicode/char/042F/index.htm)
        - "λ" U+03BB [Greek lambda](http://www.fileformat.info/info/unicode/char/03BB/index.htm)
    - 2 byte codepoint but 3 byte utf-8 (starts at U+0800)
        - "☃" U+2603 [Snowman](http://www.fileformat.info/info/unicode/char/2603/index.htm)
        - "✈" U+2708 [Airplane](http://www.fileformat.info/info/unicode/char/2708/index.htm)
        - "௸" U+0BF8 [Tamil char](http://www.fileformat.info/info/unicode/char/0BF8/index.htm)
- Astral: Supplementary (Plane 1 and 2)
    - 3 byte codepoints, 4 bytes in utf8
        - "💩" U+1F4A9 [POO](http://www.fileformat.info/info/unicode/char/1F4A9/index.htm)
        - "🄰" U+1F130 [Square Enclosed A](http://www.fileformat.info/info/unicode/char/1F130/index.htm)
        - "🍺" U+1F37A [Beer mug](http://www.fileformat.info/info/unicode/char/1f37a/index.htm)
        - "𠀀" U+20000  [First char from CJK Extension B](http://www.fileformat.info/info/unicode/char/20000/index.htm)


# Test Data Generation Strategies

Entering strings like 'asdf' or '1234' or 'john', 'jane', 'sally' or special character strings '$&*/^#@' is no longer a sufficient test data generation strategy.

## Five classes of characters in strings

Let's partition test strings into 5 classes of characters:

1. ASCII: 0..127 range is fully [backwords compatible in Unicode](http://www.fileformat.info/info/unicode/block/basic_latin/list.htm) and UTF-8. Letter A in ASCII is still the same number in Unicode.
2. Latin Supplemental: 128..255 is encoded in UTF-8 as 2 byte length. There could be issues to watch out if chars originating as UTF-8 characters are improperly re-encoded back to 8bit encodings. This is a small risk but we should include those chars in our testing.
3. The next range is 2 byte unicode codepoints above 255, encoded as 2 byte in UTF-8
4. Or 2 byte codepoints as 3 byte UTF-8. (UTF-8 should not matter here). But for completness we should distinguish those.
5. and finally an Astral plane characters; 3 byte codepoints (UTF-8 4 byte encoded).

## Unicode Test Strings

Try this universal unicode test string

Iñtërnâtiônà£ißætiøn☃💩

WARNING: [Google Chrome does not displays some unicode characters](#google-chrome-unicode-display-issue). You may not see the Chicken and Pile of Poo rendered on Chrome browser.

The above string has upper range 1 byte chars: £ ß and may expose Gibberish caused by [mojibake](http://en.wikipedia.org/wiki/Mojibake) and includes 2 byte and 3 byte codepoints with snowman and poo.

## Data from Users and data to users

Let's look at two main modes of data generation to test:

1. User generated data. Webapps should accept user's input, store that input, make decisions based on that input and display back the data entered by the user.
    - text fields entry
    - file names uploads
    - [Emails](http://en.wikipedia.org/wiki/Unicode_and_email)
        - Email addresses with International Domain Names (see punycode)

2. System generated data. Webapps generate data and send it to the users. What data will your user see?.
    - Emails. (if user gave you email with unicoded domain name can we in fact email that user?)
    - [SMS messages](http://en.wikipedia.org/wiki/GSM_03.38) (international sms)
    - url paths with unicoded characters.
        - http://oscardao.wordpress.com/tag/веды/
        - https://www.google.com/#q=москва
    - and finally text on webpages


## International Domain Names. Punycode.

International Domain names use unicode characters for application layer but [Punycode encoding](http://en.wikipedia.org/wiki/Punycode) into ascii for transport layer.

- Visiting Russian McDonalds site http://макдональдс.рф redirects to `http://xn--80aalb1aicli8a5i.xn--p1ai/` punycode counterpart.
- Users could enter email address as: `hello@макдональдс.рф` (ascii local part but unicode hostname part)
- Your webapp should be prepared that soon you will be able to enter unicode for local part of email. e.g. `jędrek@kraków.pl` or `θσερ@εχαμπλε.ψομ`



## Using Mac OS X built-in tools for Unicode

If you are testing manually here are some tools at your disposal.

## Special Character Viewer

Special Character Viewer allows you to view all unicode codepoints available on your computer. They are groupped by categories for your convenience.

- open with 'Edit > Special Characters' menu option in most apps. (Modal viewer)
- drag and drop character to your editable area
- copy char info (with right click)
- customize list, inlclude unicode, emoji in your categories viewer
- search for codepoint
    - block name (bengali, cyrillic, hangul, katakana)
    - character name (snow) shows you snoflakes, snowman,
- while viewing a particular codepoint you can add it to favorites.

Examples:

- Latin category (1 byte codepoints)
    - Examine letter "A". Notice Unicode codepoint is U+0041, UTF-8 is 41 matching the same hex number 1 to 1.
    - Examine letter "ß". Codepoint is 1 byte U+00DF. UTF-8 is a two byte hex. The actual UTF-8 hex number is not important. All characters in 1 byte codepoint range 128..255 are 2 byte size in UTF-8. What's important in testing is that 1 byte codepoint is encoded as 2 byte char and can be an issue when chars originating in UTF-8 are encoded back into 1 byte representations.
            - See for yourself: utf-8 encoded char "£" U+00a3 shows up as two chars after switching encoding to 'iso-8859-1', "Â£"
- Cyrillic category (2 byte codepoints)
    - Examine letter "А". Notice it looks like Latin A but its codepoint is U+0410. (WARNING: This is not a good letter for testing strings. it will confuse you visually. It is important to know that visual representations of letters may be similar but theyr codpoints are unique. ).
    - Examine letter "Я". U+042f. Notice 2 byte codepoint and 2 byte utf-8 as well.
- Pictographs category (2 byte codepoints)
    - Examine letter? Snowman "☃". U+2603 2 byte codepoint encoded as 3 byte char.
- Emoji category (3 byte codepoints. ASTRAL)
    - Examine Smily "😄" U+1F604. 3 byte codepoint. Notice Unicode entry dislays "U+1F604 (U+D83D U+DE04)". The second set is called a "surrogate pair", a two 2-byte sequence. (You will need this to generate the codepoint with your keyboard as described below)
    - Notice some Emoji are 2 byte codepoints. We want to test the 3 byte Astral plan chars like Poo or Chicken. They all are in U+1Fxxx range
    - I am sorry but there is not unicorn emoji.

## Keyboard Input Sources

In System Prefs Language & Text select Input Sources.

- 'Show input menu in the bar' for easy access from toolbar.
- Turn on 'Keyboard and Character Viewer'
- Enable 'Unicode Hex Input' keyboard.
- Enable keyboards for some languages other than English. i.e. Russian, Polish, Vietnamese
    - Show Keyboard Viewer. (so you don't get lost when entering Russian characters)

### Unicode Hex Input instructions

- Entering 2 byte codepoints.
    - Press ALT/OPTION, type sequence of 4 digits/letters, let go of ALT/OPTION. Your unicode 2 byte code point will appear.
- Entering 3 byte codepoints. (The following may not work for all of them.)
    - Unicode Hex Keyboard has 4 digit limit. (The following may not work in all applications)
    - You need to find surrogate pair for a 3 byte codepoint. Example Alient U+1F47D has surrogate pair D83D+DC7D
    - Press ALT/OPT 4 digits + 4 digits (yes, you have to enter plus sign between them) (Try it in TextEdit)


## Google Chrome Unicode Display Issue

Google Chrome on Mac fails to display some 3 byte unicode characters properly. Firefox does not have this issue.

If you are viewing this doc in Chrome you may not see certain characters. This is a known issue on Chrome rendering html and not all fonts support the entire range of unicode code points.

You can install the 'Symbola' font on your machine and set all fonts in Chrome to use this charset. You should see all the 3 byte codepoints. Or use Firefox which utilizes nice color fonts from Mac OS X.

# Additional Items of Interest

- The replacement character � (U+FFFD): If you ever see this character � it's a [replacement character indicating the unpresentable char](http://en.wikipedia.org/wiki/Specials_(Unicode_block))

- http://www.utf8everywhere.org/ UTF-8 Manifesto

- [Unicode fonts for ancient scripts](http://users.teilar.gr/~g1951d/). Symbola font.

