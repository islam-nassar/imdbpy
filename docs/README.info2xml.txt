# INFORMATION IN XML FORMAT

Since version 4.0, IMDbPY can output information of Movie, Person,
Character and Company instances in XML format.
It's possible to get a single information (a key) in XML format,
using the getAsXML(key) method (it will return None if the key is
not found).
E.g.:
  from imdb import IMDb
  ia = IMDb('http')
  movie = ia.get_movie(theMovieID)
  print(movie.getAsXML('keywords'))

It's also possible to get a representation of a whole object,
using the asXML() method:
  print(movie.asXML())

The returned strings are unicode.  The _with_add_keys argument
of the asXML() method can be set to False (default: True) to
exclude the dynamically generated keys (like 'smart canonical title'
and so on).


# XML FORMAT

Keywords are converted to tags, items in lists are enclosed in
a 'item' tag.  E.g.:
  <keywords>
    <item>a keyword</item>
    <item>another keyword</item>
  </keywords>

Except when keys are known to be not fixed (e.g.: a list of keywords),
in which case this schema is used:
  <item key="EscapedKeyword">
     ...
  </item>

In general, the 'key' attribute is present whenever the used tag
doesn't match the key name.

Movie, Person, Character and Company instances are converted like
that (portions enclosed in squares are optionals):
  <movie id="movieID" access-system="accessSystem">
    <title>A Long IMDb Movie Title (YEAR)</title>
    [<current-role>
       <person id="personID" access-system="accessSystem">
         <name>Name Surname</name>
         [<notes>A Note About The Person</notes>]
       </person>
     </current-role>]
     [<notes>A Note About The Movie</notes>]
  </movie>

Every 'id' can be empty.

Actually the returned XML is mostly not pretty-printed.


# REFERENCES

Some text keys can contain references to other movies, persons
and characters.  The user can provide the defaultModFunct function (see
the "MOVIE TITLES AND PERSON/CHARACTER NAMES REFERENCES" section of
the README.package file), to replace these references with their own
strings (e.g.: a link to a web page); it's up to the user, to be sure
that the output of the defaultModFunct function is valid XML.


# DTD

Since version 4.1 a DTD is available; it can be found in this
directory or on the web, at:
  http://imdbpy.sf.net/dtd/imdbpy41.dtd

The version number changes with the IMDbPY version.


# LOCALIZATION

Since version 4.1 it's possible to translate the XML tags;
see README.locale.


# FROM XML TO OBJECTS

Since version 4.6, you can dump the generated XML in a string or
in a file, using it - later - to rebuild the original object.
In the imdb.helpers module there's the parseXML() function which
takes a string as input and return - if possible - an instance of
the Movie, Person, Character or Company classes.

