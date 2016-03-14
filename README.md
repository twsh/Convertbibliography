This script takes a .bib file as argument and makes several changes to that file.
The changes are designed to make the file nice biblatex according to my notion of *nice*.
The changes are made in place if a file is given as the `--input` argument; a backup is made first.
Otherwise the script reads from STDIN and writes to STDOUT.

The script can be run on a file like this:

`$ python3 ./convertbibliography.py --input <file>`

The script depends on the [bibtexparser](https://github.com/sciunto/python-bibtexparser) (0.6.1) and [titlecase](https://pypi.python.org/pypi/titlecase) (0.7.2) modules.

The file `CB_customs.py` contains several functions that are used by bibtexparser.
These are not all used by my script, but I have left them in the file in case they are useful to somebody.

The script prints messages about what it is doing if it is called with the `--verbose` flag.

The script makes the following changes:

* Author and editor names are put in LastName, FirstName order
* LaTeX non-ASCII characters are converted to unicode
* If a record doesn't have a citekey then one is added of the form 'Foo' + a unique numeral
* Spaces are removed from citekeys
* Unicode encoding is set
* Titles are put in Title Case
* 'booktitle' fields are removed
* 'language' fields are removed where the language is English; if there is a 'language' field a 'langid' field is set too
* 'journal' fields are replaced by 'journaltitle'
* Some philosophy journals that are often missing definite articles have them added
* Books and collections have 'pages' fields removed
* Dashes are replaced with double hyphens ('--') in 'pages', 'volume', and 'number' fields
* 'pages' fields have 'pp.', 'p.' removed
* Abstracts are removed
* ISBNs and ISSNs are removed
* EPUB identifiers are removed
* 'copyright' fields are removed
* 'publisher' fields are removed from articles
* 'link' fields are removed
* '&', '%', '_' are escaped
* Ampersands ('&') in titles are replaced by 'and'
* Several fields used by [JSTOR](http://jstor.org) and [CiteULike](http://citeulike.org) are removed
* 'edition' fields are converted from ordinals to numerals
* If a book or collection has a volume number its type is changed to mvbook or mvcollection
* if 'and' occurs in a 'publisher' field it is protected
* The URL for a resolver is removed from 'doi' fields
* Keywords are removed
* Entire fields that are protected have their protection removed
* Quotes are replaced with an appropriate smart single quote
* If there is a colon (':') in a 'title' or 'journaltitle' field it is deleted and the string after it is removed and placed in a 'subtitle' or 'journalsubtitle' field as appropriate
* 'series' fields are removed
* If an article lacks a doi field then an attempt is made to get one through the CrossRef API (the `--no-doi` option suppresses this)
* 'month' fields are removed
* 'numpages' fields are removed
* 'eprint' fields are removed
* 'year' fields are converted to 'date'
* 'issue' fields are converted to 'number'
* 'leading zeroes' are removed from fields with numbers
