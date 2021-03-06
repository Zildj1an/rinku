Rinku does linking
==================

Rinku is a Ruby library that does autolinking.
It parses text and turns anything that remotely resembles a link into an HTML link,
just like the Ruby on Rails `auto_link` method -- but it's about 20 times faster,
because it's written in C, and it's about 20 times smarter when linking,
because it does actual parsing instead of RegEx replacements.

Rinku is a Ruby Gem 
-------------------

Rinku is available as a Ruby gem:

    $ [sudo] gem install rinku

The Rinku source is available at GitHub:

    $ git clone git://github.com/tanoku/rinku.git

Rinku is a standalone library
-----------------------------

It exports a single method called `Rinku.auto_link`.

~~~~ruby
require 'rinku'

auto_link(text, mode=:all, link_attr=nil)
auto_link(text, mode=:all, link_attr=nil) { |link_text| ... }
~~~~~~~~~

Parses a block of text looking for "safe" urls or email addresses,
and turns them into HTML links with the given attributes.

NOTE: Currently the follow protocols are considered safe and are the
only ones that will be autolinked.

	http:// https:// ftp:// mailto://

Email addresses are also autolinked by default. URLs without a protocol
specifier but starting with `www.` will also be autolinked, defaulting to
the `http://` protocol.

- `text` is a string in plain text or HTML markup. If the string is formatted in
HTML, Rinku is smart enough to skip the links that are already enclosed in <a>
tags.

- `mode` is a symbol, either :all, :urls or :email_addresses, which specifies which
kind of links will be auto-linked.

- `link_attr` is a string containing the link attributes for each link that
will be generated. These attributes are not sanitized and will be include as-is
in each generated link, e.g.

	~~~~ruby
	auto_link('http://www.pokemon.com', :all, 'target="_blank"')
	# => '<a href="http://www.pokemon.com" target="_blank">http://www.pokemon.com</a>'
	~~~~

	This string can be autogenerated from a hash using the Rails `tag_options` helper.

- The method takes an optional block argument. If a block is passed, it will
be yielded for each found link in the text, and its return value will be used instead
of the name of the link. E.g.
 
	~~~~ruby
 	auto_link('Check it out at http://www.pokemon.com') do |url|
 		"THE POKEMAN WEBSITEZ"
 	end
 	# => 'Check it out at <a href="http://www.pokemon.com">THE POKEMAN WEBSITEZ</a>'
	~~~~

Rinku is a drop-in replacement for Rails 3.1 `auto_link`
----------------------------------------------------

Auto-linking functionality has been removed from Rails 3.1,
and is instead offered as a standalone gem, `rails_autolink`. You can
choose to use Rinku instead.

~~~~ruby
require 'rails_rinku'
~~~~

The `rails_rinku` package monkeypatches Rails with an `auto_link` method that
mimics 100% the original one, parameter per parameter. It's just faster.

Rinku is written by me
----------------------

I am Vicent Marti, and I wrote Rinku.
While Rinku is busy doing autolinks, you should be busy following me on twitter. `@tanoku`. Do it.

Rinku has an awesome license
----------------------------

Permission to use, copy, modify, and/or distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

