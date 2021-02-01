================================================================================
SPACETEMPLATE
================================================================================
Spacetemplate lets you recursively apply a directory "template" to another
directory (on Haiku).

For example, let's say you are very particular in how your "Music" folder
windows look in Tracker― you need the "Music" folder to be a particular size, at
a particular spot, and you want all artists to be in another spot, and all
albums in another spot.

You could manually move and resize every folder in Music, but that's annoying.
Or you could write a script to apply a given template to all Music subdirectories.

… That's what this is. The template script.

========================================
USAGE
========================================

$ spacetemplate templatefolder folder

There are no arguments― it'll just apply the template folder to the folder.


========================================
TEMPLATES
========================================
A template folder hierarchy should look like the target folder's.
It'll copy the attributes from the template folder to the corresponding target,
for example:

	template/Apple/		--> target/Apple/
	template/Apple/Dad	--> target/Apple/Dad/
	template/Pear/Mom	--> target/Pear/Mom/

If a folder is in the target but doesn't have a match in the template, it'll
be ignored.

There is a fallback: _default_

_default_ will match any folder in matches to its parent folder, as long
as there isn't a direct match.

So:
	template/_default_/		--> target/Apple/
	template/_default_/Dad/		--> target/Apple/Dad/
	template/_default_/_default_/	--> target/Pear/Mom/

Keep in mind, though, that if there is a direct template for a folder, it won't
match a _default_ in the same parent directory. Default is for any folder there
is no direct match for.

	template/_default_/	--> target/Pear/
	template/Apple/		--> target/Apple/

Since there is a direct match to Apple, template/_default_/ won't be applied
to Apple. Only template/Apple/ will.

There is a wildcard, too: _all_

_all_ will match all (surprise) folders in its parent directory, even if they
have direct matches. It's basically _default_ applied to everything.

	template/_default_/	--> target/Pear/
	template/_all_/		--> target/Pear/
	template/Apple/		--> target/Apple/
	template/_all_/		--> target/Apple/

_default_ applies to Pear, but not Apple― _all_ applies to both Pear and Apple.

One more bit: _default_ and _all_ matches won't copy over the position of the
folder (trk/pinfo_le). Since if it did that, you'd end up with a bunch of
folders piled on top of each other. What a hassle.
Direct matches will copy location over, though.

The order of application is:
	# _all_
	# _default_
	# Direct

If there is a direct match, it will apply after _all_. If there is a _default_,
it will apply before _all_. 


========================================
EXAMPLE
========================================
Sooo back to the music organization problem…
Here's an example of how you'd solve that.

	template/_default_/			--> Music/*/
	template/_default_/_default_/		--> Music/*/*/

Kinda anti-climactic, for all that build-up…
Alright, let's make it more interesting.

"Rhapsody of Fire" is a band that's had several incarnations, so it would make
sense if you grouped all the Rhapsody derivatives into a single directory, as
an exception to the above rules.

	template/Rhapsody/_default_/		--> Music/Rhapsody/*/
	template/Rhapsody/_default_/_default_/	--> Music/Rhapsody/*/*/

Still not very interesting. Dang. Well, you get the point lol


========================================
BORING STUFF
========================================
MIT License
Jaidyn Ann, <jadedctrl@teknik.io>
