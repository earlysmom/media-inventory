# A simple media library manager.

Eventually this will have a UI built on it - maybe even a database
backing it. For now, it's just a JSON doc with attributes I think will
be useful down the road. Keeping this in JSON and using git will make
it fairly easy for family members to inventory stuff before the list is
complete.

# Why github?

git is a utility which supports multiple people working on the same
file independently of each other, and the ability to merge that work
together. So you could be doing an inventory of the "A" movies, while
I'm doing an inventory of the "Z" movies, and git can put those changes
together when we merge. It maintains a history of changes, so we can
see where things broke (if they do).

github is free, provided your repository is public. I don't really care
who knows what movies and albums are in our collection, so we can use
this while maintaining tightwad status, and as an added bonus, the
repo acts as our backup.

# Format Overview

master.json contains the list of media in JSON format. It's just an
array of objects, where each object represents a title. Objects should
be sorted by the title field.  Here's what that might look like:

```json
[
  { "title" : "Dark Side of the Moon" , "type" : "album" , "artist" : "Pink Floyd" },
  { "title" : "The Wall" , "type" : "album" , "artist" : "Pink Floyd" },
  { "title" : "The Wall" , "type" : "movie" , "artist" : "Pink Floyd" }
]
```

As you can see, we can store albums and movies together, and it would
be trivial for a web UI to filter down to just one or the other. It
will also be trivial to add search and the like. A couple of things
to note:

* JSON requires a comma between Title Objects

* JSON forbids a comma after the last entry in an array.

* JSON requires field names to be strings

* JSON requires a comma between object field/value pairs

* JSON requires a colon between each field and its value

* JSON requires a value if you provide a field name.

* JSON forbids a comma after the last field/value pair in an Object.

* If you need to put quotes inside a string, you have to "escape" them.

See http://json.org for full details on JSON.

# Title Object Format

## Required Fields
Each title object MUST have the following fields to be "valid":

### title

The name of the movie/album/game/etc. Example:

```json
{ "title" : "One Flew Over the Cuckoo's Nest", ... }
```

### type

The type. So far, I've identified these types:

  "album"
  "game/ps3"
  "game/ps2"
  "game/ps"
  "game/wii"
  "game/wii-u"
  "software/pc"
  "movie"

Example:

```json
{ ..., "type" : "album", ... }
```
  
### medium

The type of medium it's on. So far, I've identified these types:

  "blu-ray"
  "cd"
  "cd-rom"
  "dvd"
  "vinyl"

Example:

```json
{ ..., "blu-ray", ... }
```

## Semi-Required Fields

### artist

This is required for albums only.

Example:
```json
{ "title" : "Dark Side of the Moon", ..., "artist" : "Pink Floyd" }
```

### set

This is required when a movie came in a set, so that we can easily
find the movie even though the set name might have nothing to do with
the movie name. (For example, if we had a set named "Wesley Snipes
Before the IRS Caught Him Pack", that might be alphabetized under "W"
on the shelf, while a movie in it, say, "Blade", would be under "B" in
the inventory.)

Movies tend to get bundled together with ridiculous rationales ("Zany
Hipster Action Rom-Com Pack! Only $4.95!"). 

## Optional Fields

Each title object MAY have the following fields, too, if the person
putting the record into the inventory is feeling particularly kind

### released

The date, in ISO format ( yyyymmdd, with MMDD = 0000 if not known ),
the album/game/movie was released. Not the date it was released on
whatever format we have it on, but when it was first available. So
Star Wars Episode IV would have "19770000," or "19770525" if you
bother to look up the original release date.

### discs

How many discs are in the package? DOOM 3, for example, comes with 3
discs.

### links

Links to additional information. The value is an object, where the
name of each field is a short name for the link, e.g., "wikipedia" or "imdb",
and the value is the URL of the link.

Example:

```json
{ "title" : "Star Wars: Episode IV - A New Hope" , "links" :
  { "wikipedia" : "http://en.wikipedia.org/wiki/Star_Wars_%28film%29",
    "imdb" : "http://www.imdb.com/title/tt0076759/" }
},
```
