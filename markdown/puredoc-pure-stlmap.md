<a name="doc-pure-stlmap"></a>

pure-stlmap
===========

<a name="module-stlmap"></a>

<a name="module-stlmmap"></a>

<a name="module-stlhmap"></a>

Version 0.4, March 06, 2017

| Peter Summerland &lt;<p.summerland@gmail.com>&gt;

pure-stlmap is a [Pure](http://purelang.bitbucket.org) interface to six
associative containers provided by the [C++ Standard
Library](http://en.cppreference.com/w/cpp): map, set, multimap, multiset,
unordered\_map and unordered\_set.

Copying
-------

| Copyright (c) 2012 by Peter Summerland &lt;<p.summerland@gmail.com>&gt;.

All rights reserved.

pure-stlmap is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR
A PARTICULAR PURPOSE.

pure-stlmap is distributed under a BSD-style license, see the COPYING file for
details.

Introduction
------------

This is pure-stlmap-0.1, the first release of pure-stlmap. It is possible that
some of the functions might be changed slightly or even removed. Comments and
questions would be especially appreciated at this early stage.

### Supported Containers

The Standard C++ Containers Library, often refered to as the standard template
library ("STL"), provides templates for generic containers and generic
algorithms. pure-stlmap provides six mutable containers, "stlmap", "stlset",
"stlmmap", "stlmset", "stlhmap" and "stlhset", that are thin wrappers around
the corresponding associative containers provided by the STL, map, set,
multimap, multiset, unordered\_map and unordered\_set, specialized to hold
pure-expressions. pure-stlmap does not provide wrappers for
unordered\_multimap and unordered\_multiset.

### Interface

pure-stlmap provides a "key-based" interface that can be used to work with the
supported STL containers in a way that should feel natural to Pure
programmers. For example, the (!) function can be used to access values
associated with keys and functions like [`map`](#map/stlmap),
[`foldl`](#foldl/stlmap), [`filter`](#filter/stlmap) and [`do`](#do/stlmap)
can be used to operate on all or part of a container's elements without using
an explict tail recursive loop. In addition, for the ordered containers,
stlmap, stlmmap, stlset and stlmset, pure-stlmap provides an "interator-based"
interface that corresponds to the C++ interface, mostly on a one-to-one basis.

The interface for the unordered or "hash table" containers, stlhmap and
stlhset, is limited compared to that provided for the ordered containers. In
particular iterators, operations on subsequences (ranges) and set operations
are not supported.

In some cases, the STL's associative containers have different semantics than
the the associative containers provided by the Pure standard library. Where
there is a conflict, pure-stlmap follows the STL.

Many of the functions provided by pure-stlmap, such as the constructors,
equivalence and lexicographical comparison operations, insert and erase
operations, and the set operations are just thin wrappers around the the
corresponding C++ functions. Users can consult the C++ Library documentation
to understand the performance characteristics and corner case behavior of any
pure-stlmap function that has a corresponding function in the STL.

The C++ library is sometimes more complicated than the Pure Standard Library.
For example many of the applicable C++ functions, including set operations and
tests for equality, assume that the containers are lexicographically ordered.
The reward for playing by the rules (which occurs automatically for stlmap and
stlset) is O(n) time complexity for comparison and set operations.

Installation
------------

pure-stlmap-0.4 is included in the "umbrella" addon,
[pure-stllib](#pure-stllib) which is available at
<https://bitbucket.org/purelang/pure-lang/downloads>. After you have
downloaded and installed [pure-stllib](#pure-stllib), you will be able to use
pure-stlmap (and [pure-stlvec](#pure-stlvec), as well).

Examples
--------

The pure-stlmap/uts subdirectory contains Pure scripts that are used to test
pure-stlmap. These scripts contain simple tests, each of which consists of a
single line of code followed by a comment that contains the expected output.
E.g.,

    let sm1 = stlmap ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5];
    //- ()

    sm1!stl::smbeg, sm1!"a", sm1!"d", sm1!"e"
    //- 1,1,4,5

    catch id $ sm1!"0";
    //- out_of_bounds

You might consider pasting parts of these scripts into a temporary file that
you can play with if you are curious about how something works.

Two short example programs, anagrams.pure and poly.pure, can be found in the
pure-stlmap/examples subdirectory.

Quick Start
-----------

This section introduces the basic functions you need to get up and running
with pure-stlmap. For a quick look at the other functions provided by
pure-stlmap, you can refer to pure-stllib-cheatsheet.pdf, which can be found
in the pure-stllib/doc directory.

### Example Containers

The code snippets that appear in the examples that follow assume that six
containers have been created by entering the following at the prompt.

    $> pure -q
    > using stlmap, stlhmap, stlmmap;
    > using namespace stl;

    > // Make some maps and sets with default characteristics
    > let sm  = stlmap  ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5];
    > let shm = stlhmap ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5];
    > let smm = stlmmap ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4,"e"=>5];
    > let ss  = stlset  ["a","b","c","d","e"];
    > let shs = stlhset ["a","b","c","d","e"];
    > let sms = stlmset ["a","b","c","c","d"];

The `using` statement imports the three modules provided by pure-stlmap:
[stlmap](#module-stlmap) provides the interface for the stlmap and stlset
containers, [stlmmap](#module-stlmmap) provides the interface the stlmmap and
stlmset containers, and [stlhmap](#module-stlhmap) provides the interface to
the stlhmap and stlhset containers. The `let` statements set up an instance of
each of the containers provided by pure-stlmap, loaded with some sample
elements.

To save typing you can run readme-data.pure, a file that contains the
corresponding source code. It can be found in in the pure-stlmap/examples
directory.

### Constructors

You can construct empty pure-stlmap containers using the
[`emptystlmap`](#emptystlmap/stlmap), [`emptystlset`](#emptystlset/stlmap),
[`emptystlmmap`](#emptystlmmap/stlmap),
[`emptystlmset`](#emptystlmset/stlmap), [`emptystlhmap`](#emptystlhmap/stlmap)
and [`emptystlhset`](#emptystlhset/stlmap) functions.

    > let sm1 = emptystlmap;   // uses (<) to order keys

You can construct a pure-stlmap container and fill it with elements all in one
go using the [`stlmap`](#stlmap/stlmap), [`stlset`](#stlset/stlmap),
[`stlmmap`](#stlmmap/stlmap), [`stlmset`](#stlmset/stlmap),
[`stlhmap`](#stlhmap/stlmap) and [`stlhset`](#stlhset/stlmap) functions.

    > let shm1 = stlhmap ["a"=>1,"b"=>2,"c"=>3]; 

    > members shm1;
    ["c"=>3,"a"=>1,"b"=>2]

    > smh1!"b";
    2

As opposed to the hashed containers (stlhmap and stlhset), the ordered
containers (stlmap, stlset, stlmmap and stlmset) keep their elements ordered
by key.

    > let sm1 = stlmap ["a"=>1,"b"=>2,"c"=>3]; members sm1;
    ["a"=>1,"b"=>2,"c"=>3]

### Ranges

For the ordered containers (stlmap, stlset, stlmmap and stlmset) you can work
with subsequences, called "ranges", of the containers' elements. A range is
specified by a tuple that consists of a container and two keys. If (sm,
first\_key, last\_key) designates a range, the elements of the range are all
of elements of the container sm whose keys are equivalent to or greater than
first\_key and less than last\_key. If first\_key and last\_key are left out
of the tuple, the range consists of all of sm's elements.

    > members sm;               // no range keys - the whole container
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

    > members (sm,"b","e");     // a range from "b" up but not including "e"
    ["b"=>2,"c"=>3,"d"=>4]

    > members (sm,"c1","z");    // keys do not have to be stored
    ["d"=>4,"e"=>5]

    > members shm;              // works on a unordered set (with no range keys)
    ["c"=>3,"d"=>4,"e"=>5,"a"=>1,"b"=>2]

Two special keys, [`stl::smbeg`](#stl::smbeg/stlmap) and
[`stl::smend`](#stl::smend/stlmap) are reserved for use in ranges to designate
the first element in a container and the imaginary "past-end" element.

    > members (sm,smbeg,"d");
    ["a"=>1,"b"=>2,"c"=>3]

    > members (sm,"b",smend);
    ["b"=>2,"c"=>3,"d"=>4,"e"=>5]

Perhaps it should go without saying, but you cannot use either of these
symbols as the keys of elements stored in a pure-stlmap container.

### Inserting and Replacing Elements

You can insert elements and, for the maps (stlmap, stlmmap and stlhmap),
replace the values associated with keys that are already stored in the map,
using the [`insert`](#insert/stlmap), [`replace`](#replace/stlmap) and
[`insert_or_replace`](#insert_or_replace/stlmap) functions. For the maps, the
elements to inserted are specified as (key=&gt;value) hash-pairs.

    > let sm1 = emptystlmap;

    > insert sm1 ("e"=>5);              // returns number of elements inserted
    1
    > members sm1;
    ["e"=>5]

    > replace sm1 "e" 15;               // returns value
    15
    > members sm1;
    ["e"=>15]

    > catch id $ replace sm1 "x" 10;    // replace never inserts new elements
    out_of_bounds

    > insert sm1 ("e"=>25);             // insert never changes existing elements
    0
    > members sm1;
    ["e"=>15]

    > insert_or_replace sm1 ("e"=>25);  // 1 value changed 
    1                                   
    > members sm1;
    ["e"=>25]
    >

The [`insert`](#insert/stlmap) and
[`insert_or_replace`](#insert_or_replace/stlmap) functions are overloaded to
insert or replace elements specified in a list, vector, stlvec or another
pure-stlmap container (of the same type). E.g.,

    > let sm2 = emptystlmap;

    > insert sm2 ["b"=>2,"a"=>1];       // insert from a list
    2

    > insert sm2 (sm,"c","e");          // insert from a range
    2

    > members sm2;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4]

    > insert_or_replace sm2 {"a"=>11,"e"=>15};
    2

    > members sm2;
    ["a"=>11,"b"=>2,"c"=>3,"d"=>4,"e"=>15]

### Access

If you want to see if a key is stored in a container use the
[`member`](#member/stlmap) function. (A key, k, is considered to be "stored"
in a container if there is an element in the container that is equivalent to
k.)

    > member sm "x";  // ("x"=>val) is not an element of sm for any val
    0

    > member sm "a";  // ("a"=>1) is an element with key equivalent to "a"
    1

The value (or values for a multi-key container) associated with a key can be
accessed using the (!) function.

    > sm!"a";   // return the value associated with "a"
    1

    > shm!"b";  // try it with a hashed map
    2

    > smm!"c";  // multimap returns a the list of values associated with "c"
    [31,32]

    > ss!"a";   // with sets, return the key
    "a"

    > sms!"c";  // with multisets, return a list of keys
    ["c","c"]

If the key is not stored in the container, (!) throws an
[`out_of_bounds`](#out_of_bounds) exception.

    > catch id $ sm!"x"; // "x" is not stored as a key in sm
    out_of_bounds

Please note that all access is strictly by keys. For example you cannot use
the [`member`](#member/stlmap) function to determine if ("a"=&gt;1) is an
element stored in sm; you can only ask if the key "a" is stored in sm.

### Erasing Elements

For any pure-stlmap container, you can use the [`erase`](#erase/stlmap)
function to remove all the elements associated with a given key in the
container, all of the elements in the container or, unless the container is a
stlhmap or stlhset, all of the elements in a range defined on the container.

    > let shm1 = stlhmap shm;       // make some copies of maps
    > let smm1 = stlmmap smm;
    > let sm1 = stlmap sm;

    > members smm1;                 // smm1 has multiple values for "c"
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4,"e"=>5]

    > erase (shm1,"c");             // erase "c" keyed elements from a stlmmap
    1
    > members shm1;                 // all the "c" keyed elements are gone
    ["d"=>4,"e"=>5,"a"=>1,"b"=>2]

    > erase shm1;                   // erase all elements
    4
    > empty shm1;
    1

    > erase (sm1,"b","d");         // erase a subsequence       
    2
    > members sm1;
    ["a"=>1,"d"=>4,"e"=>5]

    > erase (sm1,"x");             // attempt to erase something not there
    0

    > erase (smm1,"c");            // erase all elements with key "c"
    2
    > members smm1;
    ["a"=>1,"b"=>2,"d"=>4,"e"=>5]

### Conversions

The elements of an associated container be copied into a list, vector or
stlvec using the [`members`](#members/stlmap),
[`stl::vector`](#stl::vector/stlmap) and [`stlvec`](#stlvec/stlmap) functions.
For ordered containers (stlmap, stlset, stlmmap and stlmset) the list, vector
or stlvec can be built from a range.

    > members ss;
    ["a","b","c","d","e"]

    > members (ss,"b","d"); // list subsequence from "b" up to but not "d" 
    ["b","c"]

    > members (smm,"c","e");
    ["c"=>31,"c"=>32,"d"=>4]

    > members (shm,"b","d"); // fails - ranges not supported for stlhmaps
    stl::members (#<pointer 0x83b4908>,"b","d")

    > members shm;          // ok - all elements are copied
    ["d"=>4,"e"=>5,"a"=>1,"b"=>2,"c"=>3]

    > vector (sm,smbeg,"d");
    {"a"=>1,"b"=>2,"c"=>3}

    > using stlvec;
    > members $ stlvec sm; 
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

You can convert the contents of an ordered container (stlmap, stlset, stlmmap
or stlmset) or a range defined on one to a stream using the
[`stream`](#stream/stlmap) function.

    > let ss1 = stlhset (0..100000);

    > stats -m

    > let xx = drop 99998 $ scanl (+) 0 (stream ss);
    0.3s, 18 cells

    > list xx;
    [704782707,704882705,704982704,705082704]
    0s, 17 cells

### Functional Programming

Most of the Pure list operations, including [`map`](#map/stlmap),
[`do`](#do/stlmap), [`filter`](#filter/stlmap), [`catmap`](#catmap/stlmap),
[`foldl`](#foldl/stlmap) and [`foldl1`](#foldl1/stlmap) can be applied to any
of pure-stlmap's associative containers. E.g.,

    > map (\x->x-32) shs;
    ["D","E","A","B","C"]

    > using system;

    > do (puts . str) (sm,smbeg,"c");
    "a"=>1
    "b"=>2
    ()

List comprehensions also work.

    > [k-32=>v+100 | (k=>v) = smm; k>"a" && k<"e"];
    ["B"=>102,"C"=>131,"C"=>132,"D"=>104]

    > {k-32=>v+100 | (k=>v) = (smm,"b","e")};
    {"B"=>102,"C"=>131,"C"=>132,"D"=>104}

It is highly recommended that you use the functional programming operations,
as opposed to recursive loops, whenever possible.

Concepts
--------

This section describes pure-stlmap's containers, iterators, ranges, elements,
keys, values and how these objects are related to each other. It also
describes a group of functions associated with containers that help define the
container's behavior. E.g., each ordered container (stlmap, stlset, stlmmap or
stlmset) stores a function that it used to order its keys and to determine if
two keys are equivalent.

### Containers and Elements

The six associative containers supported by pure-stlmap can be grouped
together in terms of certain defining attributes.

The three "maps" provided by pure-stlmap, stlmap, stlmmap and stlhmap,
associate values with keys. If a value v is associated with a key, k, in an
map, m, then we say that (k=&gt;v) is an element of m, k is a key stored in m
and v is a value stored in m.

The three "sets" provided by pure-stlmap, stlset, stlmset and stlhset, hold
single elements, as opposed to key value pairs. If an element e is contained a
set, s, we say that e is simultaneously an element, key and value stored s. In
other words, we sometimes speak of a set as if it were a map where each
element, key and value are the same object.

The "ordered" containers, stlmap, stlset, stlmmap and stlmset, each have a
"key-less-than" function that they use keep their elements in a sequence that
is ordered by keys. The default key-less-than function is
[`(<)`](#(%3C)/stlmap), but this can be changed when the container is created.
The elements stored in a stlmap or stlset have unique keys, i.e., two elements
stored in the container will never have equivalent keys. For these purposes,
two keys are "equivalent" if neither key is key-less-than the other. In
contrast, stlmmap and stlmset do not have unique keys. I.e., it is possible
for different elements stored in a stlmmap or stlmset can have equivalent
keys.

The "hashed" containers, sthmap and stlhset do not keep their elements in a
sequence. Instead they store their elments in a hash table using a "key-hash"
function and a "key-equal" function. Currently the key-hash function is always
[`hash`](#hash) and the key-equal function is always (===), both of which are
defined in the Prelude. The elements stored in a hashed container have unique
keys. I.e., two elements stored in the container will never by "key-equal". At
times we say that two keys stored in a hashed container are "equivalent" if
they are key-equal.

The "ordered maps", stlmap and stlmmap, each have a "value-less-than" function
and a "value-equal" function that is used for lexicographical comparisons. The
default functions are [`(<)`](#(%3C)) and (==) respectively, but these can
customized when the container is created.

As is the case for the underlying C++ functions, set operations (i.e., union,
intersection, etc.) and container equivalence for the ordered containers are
based on lexicographical comparisons. For these purposes one element, e1, is
less than another, e2, if (a) e1's key is less-than e2's key and, (b) if the
ordered container is a stlmap or stlmap, e1's value is value-less-than e2's
value. Finally, for purposes of determining if two ordered containers are
equal, e1 and e2 are considered to be equal if (a) their keys are equivalent
and (b), in the case of stlmap or stlmmap, their values are value-equal.

Set operations are not provided for the hashed containers, stlhmap and
stlhset.

### Ranges

For the ordered containers (stlmap, stlset, stlmmap and stlmset), you can work
with a subsequence or "range" of a container's elements. Given an ordered
container, oc, and keys f and l, the range (oc,f,l) consists of all of the
elements in oc starting with the first element that is not less than f up to
but not including the first element that is greater or equal to l. Note that f
and l do not have to be stored in oc.

    > members (sm,"b","e");
    ["b"=>2,"c"=>3,"d"=>4]

    > members (sm,"c1",smend);
    ["d"=>4,"e"=>5]

When a range is passed to a function provided by pure-stlmap, the keys can be
dropped, in which case the range consists of all of the container's elements.

    > members sm;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

Please note that support for ranges is not provided for the unordered
containers (stlhmap and stlhset). Most pure-stlmap functions that act on
ranges can, however, operate on stlhmaps or stlhsets as well, except that, for
stlhmaps and stlhsets, they always operate on all of the container's elements.
Accordingly, whenever the documentation of a function refers to a range, and
the container in question is a a stlhmap or stlhset, the range simply refers
to the container itself.

### Iterators

The native STL interface is based on "iterators" that point to elements in
containers. pure-stlmap provides support for iterators defined on its ordered
containers (stlmap, stlmmap, stlset and stlmset) but not for its unordered
containers (stlhmap and stlhset).

Iterators are most useful when dealing with stlmmaps where elements with
different values can have equivalent keys. In most cases, it is recommended
that you avoid using iterators. The functions that operate on or return
iterators are discussed separately at the end of this document.

### Selecting Elements Using Keys

Throughout pure-stlmap, unless you resort to using iterators, you can only
specify elements and ranges of elements using keys. For example you cannot use
the [`member`](#member/stlmap) function to see if a specific key, value pair
is an element of a stlmap.

    > members sm;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

    > member sm "a";
    1

    > catch id $ member sm (a=>1);
    bad_argument

In the last line of code, [`member`](#member/stlmap) treats (a=&gt;1) as a
key. Because (a=&gt;1) cannot be compared to a string using
[`(<)`](#(%3C)/stlmap), the ersatz key is treated as a bad argument.

This "key access only" approach can be an issue for stlmmaps and because
multiple elements can have equivalent keys. I.e., given a stlmmap, smm, that
containes multiple element with keys equivalent to, say, k, which element
should (!) return? pure-stlmap dodges this issue by returning all on them.
Thus, for stlmmap and stlmset (!) and [`replace`](#replace/stlmap) work with
lists of elements associated with a given key rather than, say, the first
elment with the given key.

    > members smm;
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4];

    > smm!"c";
    "c"=>[31,32]

    > replace smm "c" [31,32,33]; members smm;
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"c"=>33,"d"=>4]

    > replace smm "c" []; members smm;
    ["a"=>1,"b"=>2,"d"=>4,"e"=>5]

If selecting and replacing lists of elements with the same key is not
convenient, you can always use iterators to track down and modify any specific
element.

### C++ Implementation

For those that want to refer to the [C++ standard library
documentation](http://en.cppreference.com/w/cpp), stlmap is (essentially)
map&lt;px\*,px\*&gt;, stlmmap is multimap&lt;px\*,px\*&gt; and stlhmap is
unordered\_map&lt;px\*,px\*&gt;, where px is defined by "typedef pure\_expr
px". I.e., in C++ Containers library speak, key\_type is px\*, mapped\_type is
px\* and value\_type is pair&lt;px\*,px\*&gt;. This might be a bit confusing
because pure-stlmap's (key=&gt;value) "elements" correspond to C++
value\_types, a pair&lt;key\_type,mapped\_type&gt;, and pure-stlmap's values
correspond to mapped\_types. The C++ objects for stlset, stlmset and stlhset
are the same as stlmap, stmmap and stlhmap except that pure-stlmap ensures
that the second member of the C++ value\_type pair is always NULL.

Modules
-------

pure-stlmap provides three separate modules [stlmap](#module-stlmap),
[stlmmap](#module-stlmmap) and [stlhmap](#module-stlhmap).

Importing any one of these modules defines the stl namespace as well as two
important symbols, [`stl::smbeg`](#stl::smbeg/stlmap) and
[`stl::smend`](#stl::smend/stlmap).

<a name="stl::smbeg/stlmap"></a>`stl::smbeg`, <a name="stl::smend/stlmap"></a>`stl::smend`
:   These symbols are used to designate the key of the first element in an
    ordered container (stlmap, stlset, stlmmap or stlmset) and the key of an
    imaginary element that would come immediately after the last element of in
    the constainer. They are used to define ranges over the ordered
    containers.

<!-- -->
E.g.,

    > members sm;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

    > members (sm,"c",smend);
    ["c"=>3,"d"=>4,"e"=>5]

### The stlhmap Module

If all you want is fast insertion and lookup, you don't care about the order
of the elements stored in the container, and you do not want to use set
operations like [`stl::map_intersection`](#stl::map_intersection/stlmap), then
[stlhmap](#module-stlhmap) is probably your best choice. The supported
containers, stlhmap and stlhset are simpler to use and faster than the other
containers provided by pure-stlmap.

The [stlhmap](#module-stlhmap) module defines stlhmaps and stlhsets and
provides functions for dealing with them. You can import it by adding the
following `using` statement to your code.

    > using stlhmap;

The [stlhmap](#module-stlhmap) module defines types two types:

<a name="stlhmap/type"></a>*type* `stlhmap`, <a name="stlhset/type"></a>`stlhset`

:   <!-- -->

Please note that a stlhset is just a stlhmap where the values associated with
keys cannot be accessed or modified. I.e., a stlhset is a specialized kind of
stlhmap.

### The stlmap Module

The [stlmap](#module-stlmap) module provides you with stlmaps and stlsets and
the functions that operate on them. Consider using these containers if you
want their elements to be orderd by key, want to use ranges or if you are
using any set operations ([`stl::map_union`](#stl::map_union/stlmap),
[`stl::map_intersection`](#stl::map_intersection/stlmap), etc).

You can import the stlmap module by adding the following using statement to
your code.

    > using stlmap;

Importing the stlmap module introduces types to describe stlmap and stlset,
their iterators and ranges defined on them.

<a name="stlmap/type"></a>*type* `stlmap`, <a name="stlset/type"></a>`stlset`, <a name="stlmap_iter/type"></a>`stlmap_iter`, <a name="stlmap_rng/type"></a>`stlmap_rng`

:   <!-- -->

Please note that a stlset is just a stlmap where the values associated with
keys cannot be accessed or modified. I.e., a stlset is a specialized kind of
stlmap. Accordingly, it is not necessary, for example, to define a separate
type for iterators on stlsets as opposed to iterators on stlmaps.

### The stlmmap Module

If you need a multi-keyed container, the [stlmmap](#module-stlmmap) module,
which provides support for stlmaps and stlmsets, is your only choice. Set
operations and ranges are supported, but the semantics are more complicated
than is the case for stlmap and stlset. Because the keys stored in multi-keyed
containers are not unique you might have to resort to using iterators when
working with them.

You can import the [stlmmap](#module-stlmmap) module by adding the following
using statement to your code.

    > using stlmmap;

Importing the stlmmap module introduces types to describe stlmmap and stlmset,
along with their iterators and ranges defined on them.

<a name="stlmmap/type"></a>*type* `stlmmap`, <a name="stlmset/type"></a>`stlmset`, <a name="stlmmap_iter/type"></a>`stlmmap_iter`, <a name="stlmmap_rng/type"></a>`stlmmap_rng`

:   <!-- -->

Please note that a stlmset is just a stlmmap where the values associated with
keys cannot be accessed or modified. I.e., a stlmset is a specialized kind of
stlmmap. Accordingly, it is not necessary, for example, to define a separate
type for iterators on stlmsets as opposed to iterators on stlmmaps.

Container Operations
--------------------

Each of the six associative containers supported by pure-stlmap has its own
set of unique characteristics. Because of this the description of functions
that operate on more than one type of container can get a little complicated.
When reading this section it might be helpful to consult
pure-stllib-cheatsheet.pdf which can be found in the pure-stlib/doc directory.

### Container Construction

New empty ordered containers (stlmap, stlset, stlmmap and stlmset) can be
constructed using optional parameters that allow you to specify customized
key-less-than functions, default values, value-less-than and value-equal
functions.

<a name="mkstlmap/stlmap"></a>`mkstlmap (klt,dflt,vlt,veq)`, <a name="mkstlmmap/stlmap"></a>`mkstlmmap (klt,dflt,vlt,veq)`
:   Create a new stlmap or stlmmap where `klt` is the map's key-less-than
    function. dflt is the maps default value (used by replace\_with and
    find\_with\_default). vlt is the map's value-compare function and veq is
    its value-equal function. Only `klt` is required, and the default values
    for dflt, vlt, veq are \[\], (&lt;) and (==) respectively.

<a name="mkstlset/stlmap"></a>`mkstlset klt`, <a name="mkstlmset/stlmap"></a>`mkstlmset klt`
:   Create a new stlset or stlmset where `klt` is the set's key-less-than
    function.

<!-- -->
The internal lookup functions for the ordered containers (stlmap, stlset,
stlmmap and stlmset) are optimized to avoid callbacks if the container's
key-less-than function is is [`(>)`](#(%3E)/stlmap) or [`(<)`](#(%3C)/stlmap)
and the keys being compared are a pair of strings, ints, bigints or doubles.

You can create an empty associative container using default values for using
[`emptystlmap`](#emptystlmap/stlmap) and friends.

<a name="emptystlmap/stlmap"></a>`emptystlmap`, <a name="emptystlmmap/stlmap"></a>`emptystlmmap`, <a name="emptystlset/stlmap"></a>`emptystlset`, <a name="emptystlmset/stlmap"></a>`emptystlmset`
:   Create a new ordered map or set using default values. I.e., emptystlmap is
    the same as mkstlmap [`(<)`](#(%3C)), and so on.

<a name="emptystlhmap/stlmap"></a>`emptystlhmap`, <a name="emptystlhset/stlmap"></a>`emptystlhset`
:   Create a new stlhmap or stlhset with default values. The hash-function is
    hash and the value-equal function is (===).

<!-- -->
Convenience functions are also provided to construct an empty container and
insert elements into it in one go. The source of the elements can be a list,
vector, a stlvec, or a range defined on another container of the same type as
the new container.

<a name="stlmap/stlmap"></a>`stlmap src`, <a name="stlmmap/stlmap"></a>`stlmmap src`, <a name="stlset/stlmap"></a>`stlset src`, <a name="stlmset/stlmap"></a>`stlmset src`, <a name="stlhmap/stlmap"></a>`stlhmap src`

:   

<a name="stlhset/stlmap"></a>`stlhset src`
:   Create an associative constructor using default values and insert elements
    from copied from `src`. `src` can be a list, vector or stlvec of elements
    or a range defined over a container of the same type as the new container.
    If the new container is a stlmap, stlmmap or stlhmap, the elements of src
    must be (key=&gt;val) pairs. If the new container is a stlset, stlmset or
    stlhset they can be any pure expression that can be used as a key (i.e.,
    anything except for [`stl::smbeg`](#stl::smbeg/stlmap) or
    [`stl::smend`](#stl::smend/stlmap)).

<!-- -->
### Information

This group of functions allows you make inquiries regarding the number of
elments in a container, the number of instances of a given key held by a
container, the upper and lower bounds of a range and other information. In
addition this group includes a function that can be used to change the number
of slots used by a stlhmap or stlhset.

<a name="#/stlmap"></a>`# acon`
:   Return the number of elements in `acon`.

<!-- -->

<a name="stl::empty/stlmap"></a>`stl::empty acon`
:   Return true if `acon` is empty, else false.

<a name="stl::distance/stlmap"></a>`stl::distance rng`
:   Returns the number of elements contained in `rng` where rng is a range
    defined on an ordered container (stlmap, stlmmap, stlset, stlmset).

<a name="stl::count/stlmap"></a>`stl::count acon k`
:   Returns the number of elements in an associative container, acon, that
    have a key that is equivalent to `k`.

<a name="stl::bounds/stlmap"></a>`stl::bounds rng`
:   Return a pair of keys, first and last, such that first &lt;= k &lt; last
    for each k, where k is the key of an element in `rng`. If there is no such
    last, the second member of the returned pair will be
    [`stl::smend`](#stl::smend/stlmap). If first is the key of the first
    element of `rng's` container, the first member of the returned pair will
    [`stl::smbeg`](#stl::smbeg/stlmap).

<!-- -->
Here are two examples using the [`stl::bounds`](#stl::bounds/stlmap) function.
Notice that bounds returns [`stl::smbeg`](#stl::smbeg/stlmap) instead of "a"
in the first example.

    > members sm;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

    > bounds sm;
    stl::smbeg,stl::smend

    > bounds (sm,"a1","e");
    "b","e"

<a name="stl::container_info/stlmap"></a>`stl::container_info acon`
:   If `acon` is a stlmap or stlmmap, returns (0, klt, dflt, vlt, veq) where
    klt is `acon`'s key-less-than function, dflt is its default value, vlt is
    its value-less-than function and veq is its value\_equal function. If
    `acon` is a stlset or stlmset, returns (1,klt,\_,\_,\_) where klt is
    `acon`'s key-less-than function. If `acon` is a stlhmap or stlhset,
    returns (is\_set, bucket\_count, load\_factor, max\_load\_factor).

<a name="stl::bucket_size/stlmap"></a>`stl::bucket_size hacon n`
:   Returns the number of elements in `hacon`'s nth (zero-based) bucket where
    `hacon` is a stlhmap or stlhset.

<a name="stl::hmap_reserve/stlmap"></a>`stl::hmap_reserve hacon mlf size`
:   Sets `hacon`'s max\_load\_factor to `mlf`, sets the number of
    `hacon 's buckets to`size`/`mlf\` and rehashes `hacon` where `hacon` is a
    stlhmap or stlhset.

<!-- -->
### Modification

You can insert new items or, for the maps (stlmap, stlmmap and stlhmap),
replace values associated with keys using the [`insert`](#insert/stlmap),
[`replace`](#replace/stlmap) or
[`insert_or_replace`](#insert_or_replace/stlmap) functions.

Please note that when working with the ordered containers (stlmap, stlset,
stlmmap and stlmset) the keys of elements passed to these functions must be
compatible with the container's key-less-than function and keys that are
already inserted. E.g.,

    > members ss;
    ["a","b","c","d","e"]

    > catch id $ insert ss 1;   // e.g., 1<"a" is not defined
    bad_argument

Currently there is no similar restriction for stlhmaps and stlhsets because
(a) they do not have a key-less-than function and (b) the function they do use
for testing equality, the key-equal function is always (===), a function that
can compare any two objects.

    > members shs;
    ["c","d","e","a","b"]

    > insert shs 1;
    1
    > members shs;
    ["c",1,"d","e","a","b"]

Elements can be inserted into a pure-stlmap container individually or en masse
from a list, vector, stlvec or another container of the same type. If there is
a key in the container that is equivalent to the key of the element being
inserted, the element will not be inserted (unless the container is a stlmmap
or stlmset, both of which can hold multiple elements with equivalent keys).

<a name="insert/stlmap"></a>`insert acon src`
:   Attempts to copy elements from `src` a valid "insert source" into `acon`
    which can be any pure-stlmap container. A valid insert source is (a) a
    single element, (b) a list, vector, stlvec of elements or (c), a range
    over an associative container of the same type as `acon`. If `acon` is an
    associative map (stlmap, stlmmap or stlhmap), the `src` itself, or all the
    elements of `src`, must be key value pairs of the form (k=&gt;v). In
    contrast, if `acon` is a stlset, stlmset or stlhset, `src` or all of its
    elements can be any pure object (except [`stl::smbeg`](#stl::smbeg/stlmap)
    or [`stl::smend`](#stl::smend/stlmap)). If `acon` is a stlmap, stlset,
    stlhmap or stlhset, the element will not be inserted if its key is already
    stored in the target container. Returns the number of elements inserted,
    if any.

<!-- -->
If you are dealing with a stlmap or stlhmap and want to override the values of
elements have keys that equivalent to the keys of the items you wan to insert
you can use the [`insert_or_replace`](#insert_or_replace/stlmap) function.

<a name="insert_or_replace/stlmap"></a>`insert_or_replace acon src`
:   The same as [`insert`](#insert/stlmap) except that (a) `acon` must be a
    stlmap or a stlhmap and (b) if an element (key=&gt;newval) is about to be
    inserted and the container already contains an element (key=&gt;oldval)
    the element in the container will be changed to (key=&gt;newval). Returns
    the number of elements inserted or updated.

<a name="replace/stlmap"></a>`replace map key x`
:   `map` must be a stlmap, stlmmap or stlhmap. If `key` is not stored in
    `map` this function throws [`out_of_bounds`](#out_of_bounds). If `map` is
    a stlmap or stlhmap and (oldkey=&gt;oldval) is an element of `map`, where
    oldkey is equivalent to `key`, change the element to (oldkey=&gt;`x`). If
    `map` is a stlmmap and `key` is stored in `map`, change the values of
    elements with key eqivalent to `key`, one by one, to the elements of `x`.
    Add or delete elements as necessary so that, when the smoke clears, the
    values of `map`!`key` are copies of the elements of `x`. In all cases, if
    `key` is stored in `map` returns `x`.

<!-- -->
Here are some examples using [`replace`](#replace/stlmap).

    > members sm1;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

    > replace sm1 "e" 50;
    50

    > members sm1;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>50]

    > members smm1;
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4,"e"=>5]

    > replace smm1 "c" [31,33,35,36] $$ smm1!"c";
    [31,33,35,36]

    > replace smm1 "c" [] $$ smm1!"c";
    []

    > members smm1;
    ["a"=>1,"b"=>2,"d"=>4,"e"=>5]

<a name="replace_with/stlmap"></a>`replace_with fun map (k=>v)`
:   `map` must be a stlmap. The effect of this function is as follows: (a) if
    \~ [`member`](#member/stlmap) `map` `k` then [`insert`](#insert/stlmap)
    `map` (`k`=&gt;dflt) else (), where dflt is `map`'s dflt value, (b)
    [`replace`](#replace/stlmap) `map` `k` nv when nv = `fun` `v` (`map`!`k`)
    end. Returns `map`.

<!-- -->
Here is an example using [`replace_with`](#replace_with/stlmap) in which a
stlmmap is converted to a stlmap.

    > let sm1 = emptystlmap;

    > members smm;
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4,"e"=>5]

    > do (replace_with (:) sm1) smm;
    ()

    > members sm1;
    ["a"=>[1],"b"=>[2],"c"=>[32,31],"d"=>[4],"e"=>[5]]

Here is another example in which items are counted.

    > let sm1 = mkstlmap ( (<), 0 );

    > members sms;
    ["a","b","c","c","d"]

    > do (\x->replace_with (+) sm1 (x=>1)) sms;
    ()

    > members sm1;
    ["a"=>1,"b"=>1,"c"=>2,"d"=>1]

You can remove all the elements in a container, remove all the elements
equivalent to a given key or a remove a range of elements using the
[`erase`](#erase/stlmap) function.

<a name="erase/stlmap"></a>`erase acon`, <a name="erase/stlmap"></a>`erase (acon,k)`, <a name="erase/stlmap"></a>`erase (acon,k1,k2)`
:   The first form erases all elements in `acon` which can be any container
    provided by pure-stlmap. The second erases all elements in `acon` with key
    equivalent to `k`. The third erases the elements in the range
    (`acon`,`k1`,`k2`). The third form only applys to the ordered containers
    (stlmap, stlmmap, stlset and stlmset), not stlhmap or stlhset (because
    ranges are not defined for stlhmaps or stlhsets). Returns the number of
    elements removed from the container.

<!-- -->
Here are some examples using [`erase`](#erase/stlmap).

    > members smm;
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4,"e"=>5]

    > erase (sm,"z");
    0

    > erase (smm,"c");
    2

    > members smm;
    ["a"=>1,"b"=>2,"d"=>4,"e"=>5]

    > erase (smm,"b","e");
    2

    > members smm;;
    ["a"=>1,"e"=>5]

<a name="stl::swap/stlmap"></a>`stl::swap acon1 acon2`
:   Swaps the elements of the two containers, `acon1` and `acon2` where
    `acon1` and `acon2` are the same type of container (E.g., both are stlmaps
    or both are stlmsets).

<!-- -->
### Accessing Elements

You can test if a key is stored in a container and access the value associated
with a key using the familiar [`member`](#member/stlmap) and (!) functions.

<a name="member/stlmap"></a>`member acon k`
:   Returns true if `acon`, any container provided by pure-stlmap, contains an
    element that has a key that is equivalent to `k`.

<a name="!/stlmap"></a>`acon ! k`
:   If `acon` is not a stlmmap then (a) if `acon` has an element with key
    equivalent to `k` return its value, otherwise (b) throw an
    [`out_of_bounds`](#out_of_bounds) exception. If `acon` is a stlmmap
    then (a) if acon has as least one element with key equivalent to `k`
    return a list of values of all the elements with key equivalent to `k`,
    otherwise (b) return an null list.

<!-- -->
E.g.:

    > sm!"c";
    3

    > catch id $ sm!"f";    // "f" is not stored in sm
    out_of_bounds

    > catch id $ sm!100;    // 100 cannot be compared to strings using (<)
    bad_argument

    > smm!"c";              // for stlmmap, return list of values
    [31,32]

    > smm!"f";              // stlmmap returns null list if key is not stored
    []

You can access a sequence of elements in an ordered container (stlmap, stlset,
stlmmap or stlmset) without resorting to iterators using the next\_key and
prev\_key functions.

<a name="stl::next_key/stlmap"></a>`stl::next_key acon k`, <a name="stl::prev_key/stlmap"></a>`stl::prev_key acon k`
:   `acon` must be a stlmap, stlset, stlmmap or stlmmap. Also if `k` is not
    [`stl::smbeg`](#stl::smbeg/stlmap), [`stl::smend`](#stl::smend/stlmap) or
    an element of acon an [`out_of_bounds`](#stl::out_of_bounds) exception
    will be throw. [`next_key`](#stl::next_key/stlmap) returns the key of the
    first element in acon that has a key that is greater than `k`. If no such
    element exists or if `k` is [`stl::smend`](#stl::smend/stlmap), returns
    [`stl::smend`](#stl::smend/stlmap). [`prev_key`](#stl::prev_key/stlmap)
    returns the last element in acon that has a key that is less that `k`, or,
    if no such element exists, throws an
    [`out_of_bounds`](#stl::out_of_bounds) exception.

<!-- -->
For various reasons, it is very common to see a call to (!) or
[`replace`](#replace/stlmap) preceded by a call to [`member`](#member/stlmap)
with the same container and key. E.g.,

    > bump_wc sm w = if member sm w then replace sm w (sm!w + 1)
                     else insert sm (w=>1);

In general, this function would require two lookups to add a new word and
three lookups to bump the count for an existing word. For the ordered
containers, lookups have O(log N) complexity which can be relatively slow for
large containers.

To speed things up, each stlmap or stlset maintains a small cache of (key, C++
iterator) pairs for recently accessed keys. During lookup, the cache is
checked for a matching key, and if the key is found, the element pointed to by
the C++ iterator is used immediately. Thus, when applied to a stlmap or stlset
bump\_wc will use only one O(log N) search, rather than two or three. For
these purposes, a key matches a key in the cache only if it is the same Pure
object (i.e., the test is C++ pointer equality, not Pure's (===) or (==)
functions). For example, the following will result in two O(log N) lookups.

    > if member sm "a" then sm!"a" else insert sm ("a"=>10);

Here each "a" is a distinct Pure object. The two "a"s satisfy (==) and even
(===) but they are not the same internally and the caching mechanism will not
help.

Almost any pure-stlmap function that accepts a stlmap or stlset as an argument
will check the container's cache before doing an O(log N) lookup. Currently
the cache is limited to hold only the most recently used key.

Here are some examples produced by compiling pure-stlmap with a trace function
that shows caching in action.

    > let a_key = "a";

    > members sm;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

    > member sm a_key;           // a_key is not yet in the cache
    1

    > sm!a_key;                  // a_key is found in the cache
    found iterator for: "a"
    1

    > replace sm a_key 10;
    found iterator for: "a"
    10

    > sm!"a";                    // "a" is a new key, not same C++ pointer as k or a_key
    1

    > let k = next_key sm a_key; // now k is in the cache, in front of a_key
    found iterator for: "a"

    > let k1 = next_key sm k;    // now k1 is at the head of the queue
    found iterator for: "b"

    > replace sm k1 30;
    found iterator for: "c"
    30

    > members sm;
    ["a"=>10,"b"=>2,"c"=>30,"d"=>4,"e"=>5]

These examples show that caching can be effective wnen visiting elements of a
stlmap or stlset in order using [`next_key`](#next_key/stlmap) or
[`prev_key`](#prev_key/stlmap).

### Conversions

The contents of a pure-stlmap container can be copied to a list, vector,
stlvec. For stlmaps, stlsets, stlmmaps and stlmsets, these operations act on
ranges as well as on the entire container.

<a name="members/stlmap"></a>`members rng`
:   Returns a list of the elments in the range, `rng`.

<a name="keys/stlmap"></a>`keys rng`, <a name="vals/stlmap"></a>`vals rng`
:   Return the keys and vals of the range's elements.

<!-- -->
Here are some examples using the [`members`](#members/stlmap),
[`keys`](#keys/stlmap) and [`vals`](#vals/stlmap) functions.

    > members shm;   // must do all of shm elements because shm is a stlhmap
    ["d"=>4,"e"=>5,"a"=>1,"b"=>2,"c"=>3]

    > keys (sm,"b","e"); // can ask for a range - sm is an ordered container
    ["b","c","d"]

    > vals (sm,"b","e");
    [2,3,4]

<a name="stl::vector/stlmap"></a>`stl::vector rng`
:   Return a vector containing the elments of in the range, rng.

<!-- -->

<a name="stlvec/stlmap"></a>`stlvec rng`
:   returns a stlvec containing the elments of in the range, rng.

<!-- -->
You can also convert an ordered container (stlmap, stlset, stlmmap or stlmset)
into a stream of elements.

<a name="stream/stlmap"></a>`stream rng`
:   Returns a stream consisting of the range's elements.

<!-- -->
Here is an example using the stream function on a stlmmap.

    > members smm;
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4,"e"=>5]

    > take 3 $ stream smm;
    ("a"=>1):#<thunk 0xb70f438c>

    > list ans;
    ["a"=>1,"b"=>2,"c"=>31]

### Functional Programming

pure-stlmap provides the most commonly used functional programming operations,
implemented to act on ranges as if they were lists.

<a name="do/stlmap"></a>`do fun rng`, <a name="map/stlmap"></a>`map fun rng`, <a name="filter/stlmap"></a>`filter pred rng`, <a name="foldl/stlmap"></a>`foldl fun x rng`, <a name="foldl1/stlmap"></a>`foldl1 fun rng`

:   

<a name="foldr/stlmap"></a>`foldr fun x rng`, <a name="foldr1/stlmap"></a>`foldr1 fun rng`
:   These functions are the same as the corresponding functions provided in
    the Prelude for lists. `rng` is a rng defined on a stlmap, stlset, stlmmap
    or stlmset or `rng` is simply a stlhmap or stlhset.
    [`foldr`](#foldr/stlmap) and [`foldr1`](#foldr1/stlmap) are not defined
    for stlhmaps or stlhsets.

<!-- -->
Here are some examples.

    > members sm;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

    > map (\(k=>v)->k+str v) (sm,"b","e");
    ["b2","c3","d4"]

    > foldr1 (\(k=>v) (ks=>sum)-> (k+ks=>v+sum)) (sm,"b","e");
    "bcd"=>9

    > filter (\(k=>v)->v mod 2) sm;
    ["a"=>1,"c"=>3,"e"=>5]

<a name="listmap/stlmap"></a>`listmap fun rng`, <a name="catmap/stlmap"></a>`catmap fun rng`, <a name="rowmap/stlmap"></a>`rowmap fun rng`, <a name="rowcatmap/stlmap"></a>`rowcatmap fun rng`, <a name="colmap/stlmap"></a>`colmap fun rng`

:   

<a name="colcatmap/stlmap"></a>`colcatmap fun rng`
:   These functions are the same as the corresponding functions provided in
    the Prelude for lists. `rng` is a rng defined on a stlmap, stlset, stlmmap
    or stlmset or simply a stlhmap or stlhset.

<!-- -->
These functions are provided primarily to enable the use of list and matrix
comprehensions over pure-stlmap's containers. E.g.,

    > [ k + str v | (k=>v) = (sm,"b","e")];
    ["b2","c3","d4"]

    > [ k=>v | (k=>v) = sm; v mod 2];
    ["a"=>1,"c"=>3,"e"=>5]

    > { {k;v} |  (k=>v) = sm; v mod 2};
    {"a","c","e";1,3,5}

The functional programming operations work directly on the underlying data
structure.

    > let ints = 0..10000;

    stats -m
    > filter (==99) ints;
    [99]
    0s, 6 cells

### Comparison

Two associative containers of the same type are considered to be equal if they
contain the same number of elements and if each pair of their corresponding
elements are equal. Two elements are equal if their keys are equivalent and,
if the container is a stlmap, stlmap or stlhmap, the values associated with
equal keys are equal (using the container's value-equal function).

<a name="stl::map_equal/stlmap"></a>`stl::map_equal rng1 rng2`

:   <!-- -->

<a name="==/stlmap"></a>`rng1 == rng2`, <a name="~=/stlmap"></a>`rng1 ~= rng2`
:   Test `rng1` and `rng2` for equality or nonequality where `rng1` and `rng2`
    are ranges defined over containers of the same type.

<!-- -->
You need to be careful when using these operators. E.g.,

    > members ss;
    ["a","b","c","d","e"]

    > let xx = stlset ss;

    > xx == ss;
    1

    > (xx,"a","c") == (ss,"a","c");  // oops!
    0

The second comparison was intended to compare identical ranges and return
true. It failed to do so because (==) is defined in the Prelude to compare
tuples element by element, long before it is defined in the stlmap module to
compare ranges. The tuple operation take precedence and determines that the
tuples are not equal because `xx` and `ss` are different (pointers) for
purposes of this comparison. To avoid this issue when using ranges, you can
use the [`stl::map_equal`](#stl::map_equal/stlmap) function.

    > map_equal (xx,"a","c") (ss,"a","c");
    1

The other comparison operators [`(<)`](#(%3C)/stlmap),
[`(<=)`](#(%3C=)/stlmap), [`(>)`](#(%3E)/stlmap) and [`(>=)`](#(%3E=)/stlmap)
are provided only for the ordered containers (stlmap, stlset, stlmmap and
stlmset). These operators reflect lexicographical comparisons of keys and,
then if the keys are equal, lexicographical comparisons of values. I.e., this
is not set inclusion - order matters. Accordingly, these comparison operators
are not defined for a stlhmap or stlhset.

<a name="</stlmap"></a>`rng1 < rng2`
:   Traverse the ranges comparing pairs of elements e1 and e2. If e1 is less
    than e2, stop and return true; if e2 is less than e1 then stop and return
    false. If rng1 is exhausted but rng2 is not, return true, else return
    false. The two ranges must be defined on ordered associative containers of
    the same type.

<a name="&gt;/stlmap"></a>`rng1 > rng2`, <a name="<=/stlmap"></a>`rng1 <= rng2`, <a name="&gt;=/stlmap"></a>`rng1 >= rng2`
:   The these three operators are the same as `rng2` &lt; `rng1`,
    \~(`rng1`&gt;`` rng2`) and ~( ``rng1`<`rng2\`\`) respectively.

<!-- -->
You also have to be careful when using equivalence and comparison operators
with stlmmaps because elements with the same key and different values are not
necessarily ordered by values.

    > let smm2 = stlmmap ["a"=>1,"b"=>2,"c"=>32,"c"=>31,"d"=>4];

    > members smm;
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4]

    > members smm2;
    ["a"=>1,"b"=>2,"c"=>32,"c"=>31,"d"=>4]

    > smm == smm2; // probably not what you want
    0 

These operations do not make much sense for a stlmmap unless elements with
equivalent keys are stored by value, in the order enforced by the stlmmap's
value-comp function. In this regard it is worth noting that, depending on your
implementation, the [`insert`](#insert/stlmap) function may or may not
preserve the order of insertion of elements with equivalent keys (C++11 does
preserve the order).

### Set Algorithms

pure-stlmap provides wrappers for the STL set algorithms that apply to ranges
defined on the four ordered associative containers (stlmap, stlset, stlmmap
and stlmset). These algorithms are very efficient, with linear time
complexity, but they do require that the elements of the two ranges be
ordered. Accordingly, the set algorithms are not applicable to stlhmap or
stlhset. Also, when dealing with stlmmaps, care must be taken to ensure that
items with the equivalent keys are ordered by their values.

<a name="stl::map_merge/stlmap"></a>`stl::map_merge rng1 rng2`
:   Constructs a new ordered container from `rng1` and then insert the elments
    of `rng2` into the new container and return it. `rng1` and `rng2` must be
    defined on the same type of ordered container.

<a name="stl::map_union/stlmap"></a>`stl::map_union rng1 rng2`, <a name="stl::map_difference/stlmap"></a>`stl::map_difference rng1 rng2`, <a name="stl::map_intersection/stlmap"></a>`stl::map_intersection rng1 rng2`, <a name="stl::map_symmetric_difference/stlmap"></a>`stl::map_symmetric_difference rng1 rng2`

:   

<a name="stl::map_includes/stlmap"></a>`stl::map_includes rng1 rng2`
:   Returns a new ordered associative container of the same type as the
    ordered containers underlying `rng1` and `rng2`. If the ranges are defined
    over a stlmap or stlmmap elements of `rng1` have priority over the elments
    of `rng2`. Uses `rng1`'s key-less-than, value-less-than and value-equal
    functions.

<!-- -->
pure-stlmap's set functions do not necessarily produce the same results as
their Pure standard library counterparts. In particular, when applied to
multi-keyed contaners, [`stl::map_union`](#stl::map_union/stlmap) Produces the
multiset union of its arguments while (+) in the Pure standard library
produces the multiset sum. If you want the multiset sum of a stlmmap or
stlhmap, use [`stl::map_merge`](#stl::map_merge/stlmap). Also, in pure-stlmap,
as in the STL, the left hand map or set has priority of elements while in the
Pure standard library the right hand set has priority of elements. This can
make a difference when applying set operations to a pair of stlmaps or
stlmmaps. E.g.,

    > let smm1 = stlmmap ["a"=>1,"b"=>2,"c"=>31,"c"=>32];
    > let smm2 = stlmmap ["c"=>32,"c"=>32,"c"=>33,"d"=>4,"e"=>5];

    > members $ map_merge smm1 smm2; // three "c"=>32
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"c"=>32,"c"=>32,"c"=>33,"d"=>4,"e"=>5]

    > members $ map_union smm1 smm2;  // two "c"=>32
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"c"=>32,"c"=>33,"d"=>4,"e"=>5]

    > let sm1 = stlmap ["a"=>1,"b"=>2,"c"=>31];
    > let sm2 = stlmap ["c"=>32,"d"=>4,"e"=>5];

    > members $ map_union sm1 sm2; // "c"=>31 from sm1, not "c"=>32 from sm2
    ["a"=>1,"b"=>2,"c"=>31,"d"=>4,"e"=>5]

    > members $ map_intersection sm1 sm2; // "c"=>31 from sm1
    ["c"=>31]

### Direct C Calls

It is common to encounter code that (a) tests if a key is stored in a
container using [`member`](#member/stlmap) and (b) in the case of maps,
retreives the value or values associated with the key using (!) and/or (c)
changes the value or values using [`replace`](#replace/stlmap). Depending on
what modules have been loaded, these functions may be heavily overloaded which
can cause a small delay when the functions are called. To avoid this,
pure-stlmap exposes the corresponding C functions so that they can be called
directly. The C functions have the same name as the overloaded functions
except for a prefix. E.g.,

<a name="stl::sm_member/stlmap"></a>`stl::sm_member sm key`, <a name="stl::sm_get/stlmap"></a>`stl::sm_get sm key`, <a name="stl::sm_put/stlmap"></a>`stl::sm_put sm key val`
:   The first two functions are the direct C call equivalents of
    ([`::member`](#member/stlmap) `sm` `key`) and (`sm!key`). The third is
    like ([`::replace`](#replace/stlmap) `sm` `key` `val`) except that it will
    insert (key=&gt;val) if key is not already stored in `sm`. Here, `sm` is a
    stlmap or a stlset (except that sm\_put is not defined for stlsets).

<!-- -->

<a name="stl::shm_member/stlmap"></a>`stl::shm_member shm key`, <a name="stl::shm_get/stlmap"></a>`stl::shm_get shm key`, <a name="stl::shm_put/stlmap"></a>`stl::shm_put shm key val`
:   The first two functions are the direct C call equivalents of
    ([`::member`](#member/stlmap) `shm` `key`) and (`shm!key`). The third is
    like ([`::replace`](#replace/stlmap) `shm` `key` `val`) except that it
    will insert (key=&gt;val) if key is not already stored in `shm`. Here,
    `shm` is a stlhmap or a stlhset (except that shm\_put is not defined for
    stlhsets).

<!-- -->

<a name="stl::smm_member/stlmap"></a>`stl::smm_member smm key`, <a name="stl::smm_get/stlmap"></a>`stl::smm_get smm key`, <a name="stl::smm_put/stlmap"></a>`stl::smm_put smm key vals`
:   The first two functions are the direct C call equivalents of
    ([`::member`](#member/stlmap) `smm` `key`) and (`smm!key`). The third is
    like ([`::replace`](#replace/stlmap) `smm` `key` `val`) except that it
    will insert (key=&gt;val1, key=&gt;val2, ...) if key is not already stored
    in `smm`. Here, `smm` is a stlmmap or a stlmset (except that smm\_put is
    not defined for stlmsets).

<!-- -->
Iterators
---------

This section provides a quick overview of pure-stlmap's "iterator-based"
interface.

### Concepts

Given a valid iterator you can access, modify or erase the element it points
to.

    > let sm1 = stlmap sm; members sm1;  
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5];

    > let i = find sm1 "b";   // use find to get an iterator - like C++

    > get_elm i; 
    "b"=>2

    > get_val i; 
    2

    > put_val i 20;
    20

    > members sm1;
    ["a"=>1,"b"=>20,"c"=>3,"d"=>4,"e"=>5]

Please note that you can never modify an element's key, only its value. If you
want to change both key and value, you have to erase the element and insert a
new element.

    > erase (sm1,i) $$ insert sm1 ("b1"=>21);
    1

    > members sm1;
    ["a"=>1,"b1"=>21,"c"=>3,"d"=>4,"e"=>5]

Given two iterators, i and j, pointing into a ordered container oc, the range
(i,j), denotes oc's elements starting with "oc\[i\]", the element pointed to
by i, up to but not including oc\[j\]. In pure-stlmap, this range is denoted
by the tuple (i,j).

    > members sm;
    ["a"=>1,"b"=>2,"c"=>3,"d"=>4,"e"=>5]

    > let i = stl::find sm1 "b"; // get the iterator

    > let j = stl::find sm1 "e";

    > members (i,j);             // get the elements in the range
    ["b"=>2,"c"=>3,"d"=>4]

Perhaps it is worth mentioning that functions that act on ranges do not care
if the range is specified by a pair of iterators or by keys.

    > members ss;
    ["a","b","c","d","e"]

    > map (+21) (ss,"c",smend);
    ["x","y","z"]

    > let i = find ss "c";
    > let j = pastend ss;
    > map (+21) (i,j);
    ["x","y","z"]

### Exceptions

In pure-stlmap functions that accept iterators throw a
[`bad_argument`](#stl::bad_argument) exception if called with an invalid
iterator. An iterator remains valid until the element it was pointing to has
been erased. These functions also attempt to throw bad argument exceptions for
invalid usage that would otherwise result in undefined behavior. An example of
an invalid use would be a range specified by iterators from different
containers. Here are some examples of iterator errors.

    > let i,j = find sm "a", find sm "d";

    > get_elm i, get_elm j;
    "a"=>1,"d"=>4

    > members (i,j);
    ["a"=>1,"b"=>2,"c"=>3]

    > catch id $ members (j,i); // j and i transposed, C++ would segfault
    bad_argument

    > erase (sm,"b");  // erase "b"=>2, leaving i and j valid
    1

    > get_elm i; // still valid
    "a"=>1

    > erase (sm,"a");  // erase "a"=>1 - invalidating i
    1

    > catch id $ get_elm i; // bad iterator exception
    bad_argument

### Functions

In this section "acon" always denotes one of the containers that supports
interators (stlmap, stlset, stlmmap and stlmset).

<a name="stl::iterator/stlmap"></a>`stl::iterator i`
:   Returns a new iterator that points to the same element as `i`.

<a name="stl::begin/stlmap"></a>`stl::begin acon`, <a name="stl::pastend/stlmap"></a>`stl::pastend acon`
:   Returns `acon`'s begin or past-end iterator.

<a name="stl::find/stlmap"></a>`stl::find acon k`
:   Creates a new iterator that points to an element in `acon` with key
    equivalent to `k` (if any) or `acon`'s past-end iterator if no such
    element exists.

<a name="stl::find_with_default/stlmap"></a>`stl::find_with_default map k`
:   Returns an iterator pointing to the element in `map`, a stlmap, with key
    equivalent to `k`. If no such element existed before the call, one is
    created and inserted using `k` and `map`'s default value. This function is
    pure-stlmap's version of C++'s \[\] operator for associative containers.

<a name="stl::insert_elm/stlmap"></a>`stl::insert_elm acon elm`
:   Attempts to insert `elm` into `acon`. (If `acon` is a stlmap or stlmmap,
    then elm must be a key value pair, (k=&gt;v)). If acon is a stlmap or
    stlset (i.e., with unique keys) [`insert_elm`](#stl::insert_elm/stlmap)
    returns a pair, the first of which is an iterator pointing to the element
    with key k that was just inserted (or the pre-existing element that
    blocked the insertion). The second element in the pair is a boolean value
    that is true if a new element was inserted. In contrast, if `acon` is a
    multi-keyed container (stlmmap or stlmset) the insert will always be
    successful and [`insert_elm`](#stl::insert_elm/stlmap) returns an iterator
    pointing to the element with key k that was just inserted, instead of an
    (iterator, boolean) tuple.

<a name="stl::insert_elm/stlmap"></a>`stl::insert_elm acon (elm,i)`
:   This is the same as the previous function except that (a) `i` is passed in
    as a hint to where the new element should be inserted and (b) a single
    iterator is returned rather than a iterator,boolean pair. If the new
    element is inserted just after `i`, the insertion can have constant time
    complexity.

<a name="stl::l_bound/stlmap"></a>`stl::l_bound acon k`
:   Return a new iterator that points to the first element in `acon`, a
    stlmap, stlset, stlmmap or stlmset, that is not less than `k`, or `acon`'s
    past-end iterator if none exists.

<a name="stl::u_bound/stlmap"></a>`stl::u_bound acon k`
:   Return a new iterator that points to the first element in `acon`, a
    stlmap, stlset, stlmmap or stlmset, that is greater than `k`, or `acon`'s
    past-end iterator if none exists.

<a name="stl::lu_bounds/stlmap"></a>`stl::lu_bounds acon k`
:   Return the pair l\_bound `acon` `k`, u\_bound `acon` `k`.

<!-- -->
E.g.,

    > let ok, smx, f, l = stl::range_info (sm1,"b","e");

    > ok, smx === sm1, stl::members (f,l);
    1,1,["b"=>2,"c"=>3,"d"=>4]

<a name="stl::inc/stlmap"></a>`stl::inc i`, <a name="stl::dec/stlmap"></a>`stl::dec i`, <a name="stl::move/stlmap"></a>`stl::move i n::int`
:   Move the iterator `i` forward one, back one or forward `n` elements
    respectively, where n can be negative. The iterator is mutated by these
    operations, provided the move is successful. An attempt to move to a
    position before the first element's position causes an
    [`out_of_bounds`](#stl::out_of_bounds) exception. Moves past the last
    element return the past-end iterator for the container that `i` is defined
    on.

<a name="stl::get_elm/stlmap"></a>`stl::get_elm i`, <a name="stl::get_key/stlmap"></a>`stl::get_key i`, <a name="stl::get_val/stlmap"></a>`stl::get_val i`
:   Return the element pointed to by the iterator `i`, or the element's key or
    value. For maps the element is returned as a key=&gt;value hash rocket
    pair. For sets, get\_elem, get\_key and get\_val all return the element
    (which is the same as its key).

<a name="stl::put_val"></a>`stl::put_val i newvalue`
:   Change the value of the element pointed to by the iterator `i` to
    `newvalue`. The element's key cannot be changed. The iterator must point
    into a stlmap or stlmmap.

<a name="stl::beginp/stlmap"></a>`stl::beginp i`, <a name="stl::pastendp/stlmap"></a>`stl::pastendp i`
:   Returns true if the iterator `i` is the begin iterator or pastend iterator
    of the container it is defined on.

<a name="stl::get_info/stlmap"></a>`stl::get_info i`
:   Returns a tuple (is\_valid,acon,key,val) where is\_valid is true if the
    iterator `i` is valid or false if not, acon is the container that i is
    defined on, and key, val are the key and value of the element `i` points
    to, if any. If `i` is the past-end iterator, key and val are set to
    [`stl::smend`](#stl::smend/stlmap) and `[]`, respectively.

<!-- -->

<a name="==/stlmap"></a>`i == j`
:   Returns true if the iterators `i` and `j` point to the same element.

<a name="erase/stlmap"></a>`erase (acon,i)`, <a name="erase/stlmap"></a>`erase (acon,i,j)`
:   Erases the element pointed to by `i` or the elements in the range (`i`,
    `j`). Both `i` and `j` must be iterators defined on `acon` (or a
    [`bad_argument`](#bad_argument) exception will be thrown).

<!-- -->
### Examples

Here are some examples using iterators.

    > let b,e = begin smm, pastend smm;

    > members (b,e);
    ["a"=>1,"b"=>2,"c"=>31,"c"=>32,"d"=>4,"e"=>5]

    > let i,j = lu_bounds smm "c";

    > members (b,i);
    ["a"=>1,"b"=>2]

    > members (i,j);
    ["c"=>31,"c"=>32]

    > members (j,e);
    ["d"=>4,"e"=>5]

    > get_elm i;
    "c"=>31

    > get_elm (inc i);
    "c"=>32

    > put_val i 132;
    132

    > map (\(k=>_)->k=>ord k) (b,i);
    ["a"=>97,"b"=>98,"c"=>99]

    > let is_set, smm1, k, v = get_info i; is_set, members smm1, k, v;
    1,["a"=>1,"b"=>2,"c"=>31,"c"=>132,"d"=>4,"e"=>5],"c",132

    > get_elm (dec j);
    "c"=>132

    > inc j $$ inc j $$ get_elm j;
    "e"=>5

    > inc j $$ endp j;
    1

Backward Compatibilty
---------------------

This section documents changes in pure-stlmap.

### pure-stlmap-0.2

Optimized common predicates, such as (&lt;) and (&gt;)

### pure-stlmap-0.3

Fixed (&gt;) comparisons on plain old data.