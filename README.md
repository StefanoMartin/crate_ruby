[![Gem Version](https://badge.fury.io/rb/crate_ruby.svg)](http://badge.fury.io/rb/crate_ruby)
[![Build Status](https://travis-ci.org/crate/crate_ruby.svg?branch=master)](https://travis-ci.org/crate/crate_ruby)
[![Code Climate](https://codeclimate.com/github/crate/crate_ruby.png)](https://codeclimate.com/github/crate/crate_ruby)


# CrateRuby

Official Ruby library to access a [Crate](http://crate.io) database.

## Installation

Add this line to your application's Gemfile:

    gem 'crate_ruby'

Or install it yourself as:

    $ gem install crate_ruby

## Usage

### Issueing SQL statements
    require 'crate_ruby'

    client = CrateRuby::Client.new

    result = client.execute("Select * from posts")
     => #<CrateRuby::ResultSet:0x00000002a9c5e8 @rowcount=1, @duration=5>

    result.each do |row|
      puts row.inspect
    end
     => [1, "test", 5]

    result.cols
     => ["id", "my_column", "my_integer_col"]
     
     
#### Using parameter substitution
     
     client.execute("INSERT INTO posts (id, title, tags) VALUES (\$1, \$2, \$3)",
                     [1, "My life with crate", ['awesome', 'freaky']])

### Up/Downloading data
    digest = Digest::SHA1.file(file_path).hexdigest

    #upload
    f = File.read(file_path)
    client.blob_put(table_name, digest, f)

    #download
    data = client.blob_get(table_name, digest)
    open(file_path, "wb") do |file|
      file.write(data)
    end

    #deletion
    client.blob_delete(table_name, digest)

## Tests

To run the tests start up the crate server first

    ruby spec/test_server.rb

## Contributing

1. Fork it ( `http://github.com/crate/crate_ruby/fork` )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Add some tests
4. Commit your changes (`git commit -am 'Add some feature'`)
5. Push to the branch (`git push origin my-new-feature`)
6. Create new Pull Request

##Maintainer

* [Christoph Klocker](http://www.vedanova.com), [@corck](http://www.twitter.com/corck)

##License & Copyright
See LICENSE for details.

