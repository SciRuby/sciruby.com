---
layout: post
title: "GSoC 2017 : Adding support to more Import-Export modules"
date: 2017-08-29 17:16
comments: true
author: Athitya Kumar
categories: [GSOC 2017, GSOC, Daru, Daru-IO, Import, Export]
---

> "Rubyists, Data Analysts and Web Developers, lend me your ears.
> I come to document my GSoC 2017 project, not to earn praise for it.
> The open-source contributions that people do, lives after them.
> But their private repositories, are oft interred with their bones."

Ah, it was quite amazing to try translating Mark Antony's speech for OSS lovers. Now
that we've broken the ice, let's get started with my GSoC 2017 project :
[Daru-IO](https://github.com/athityakumar/daru-io)
is a plugin-gem to daru gem, that extends support for many Import and Export methods
of `Daru::DataFrame`. This gem is intended to help Rubyists who are into Data Analysis
or Web Development, by serving as a general purpose conversion library.

Through this summer, I worked on adding support for various Importers and Exporters
while also porting some existing modules. Please feel free to find a comprehensive set
of useful links in this [Final Work Submission](https://athityakumar.github.io/blog/posts/GSoC_2017_-_Final_Work_Submission/).

Before proceeding any further to showcase code-snippets of daru-io, I think you'd be more
interested in seeing [Rails example](https://daru-examples-io-view-rails.herokuapp.com/)
and [the code](https://github.com/Shekharrajak/daru_examples_io_view_rails)
making it work.

# Importers

The `Daru::IO` Importers are intended to return a `Daru::DataFrame` from the given arguments. Generally,
all Importers can be called in two ways - from `Daru::IO` or `Daru::DataFrame`.

```ruby
#! Partially requires Format Importer
require 'daru/io/importers/format'

#! Usage from Daru::IO
instance = Daru::IO::Importers::Format.new(opts)
df1 = instance.from(connection)
df2 = instance.read(path)

#! Usage from Daru::DataFrame
df1 = Daru::DataFrame.from_format(connection, opts)
df2 = Daru::DataFrame.read_format(path, opts)
```

*Note: Please have a look at the respective Importer documentation links below, for more information about arguments and examples.*

## ActiveRecord Importer

Imports a `Daru::DataFrame` from an **ActiveRecord** connection.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/ActiveRecord)
- **Gem Dependencies**: `activerecord` gem
- **Other Dependencies**: Install database server(s) such as SQL / Postgresql / etc.
- **Usage**

```ruby
#! Partially require just ActiveRecord Importer
require 'daru/io/importers/active_record'

#! Usage from Daru::IO
df = Daru::IO::Importers::ActiveRecord.new(:field_1, :field_2).from(activerecord_relation)

#! Usage from Daru::DataFrame
df = Daru::DataFrame.from_activerecord(activerecord_relation, :field_1, :field_2)
```

## Avro Importer

Imports a `Daru::DataFrame` from an **.avro** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/Avro)
- **Gem Dependencies**: `avro` and `snappy` gems
- **Usage**

```ruby
#! Partially require just Avro Importer
require 'daru/io/importers/avro'

#! Usage from Daru::IO
df = Daru::IO::Importers::Avro.new.read('path/to/file.avro')

#! Usage from Daru::DataFrame
df = Daru::DataFrame.from_avro('path/to/file.avro')
```

## CSV Importer

Imports a `Daru::DataFrame` from a **.csv** or **.csv.gz** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/CSV)
- **Usage**

```ruby
#! Partially require just CSV Importer
require 'daru/io/importers/csv'

#! Usage from Daru::IO
df1 = Daru::IO::Importers::CSV.new(skiprows: 10, col_sep: ' ').read('path/to/file.csv')
df2 = Daru::IO::Importers::CSV.new(skiprows: 10, compression: :gzip).read('path/to/file.csv.gz')

#! Usage from Daru::DataFrame
df1 = Daru::DataFrame.from_csv('path/to/file.csv', skiprows: 10, col_sep: ' ')
df2 = Daru::DataFrame.from_csv('path/to/file.csv.gz', skiprows: 10, compression: :gzip)
```

## Excel Importer

Imports a `Daru::DataFrame` from a **.xls** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/Excel)
- **Gem Dependencies**: `spreadsheet` gem
- **Usage**

```ruby
#! Partially require just Excel Importer
require 'daru/io/importers/excel'

#! Usage from Daru::IO
df = Daru::IO::Importers::Excel.new(worksheet_id: 1).read('path/to/file.xls')

#! Usage from Daru::DataFrame
df = Daru::DataFrame.from_excel('path/to/file.xls', worksheet_id: 1)
```

## Excelx Importer

Imports a `Daru::DataFrame` from a **.xlsx** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/Excelx)
- **Gem Dependencies**: `roo` gem
- **Usage**

```ruby
#! Partially require just Excel Importer
require 'daru/io/importers/excelx'

#! Usage from Daru::IO
df = Daru::IO::Importers::Excelx.new(sheet: 2, skiprows: 10, skipcols: 2).read('path/to/file.xlsx')

#! Usage from Daru::DataFrame
require 'daru/io/importers/excel'
df = Daru::DataFrame.from_excel('path/to/file.xlsx', sheet: 2, skiprows: 10, skipcols: 2)
```

## HTML Importer

**Note: This module works only for static tables on a HTML page, and won't work in cases where the table is being loaded into the HTML table by inline Javascript. This is how the Mechanize gem works, and the HTML Importer also follows suit.**

Imports an **Array** of `Daru::DataFrame`s from a **.html** file or website.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/HTML)
- **Gem Dependencies**: `mechanize` gem
- **Usage**

```ruby
#! Partially require just HTML Importer
require 'daru/io/importers/html'

#! Usage from Daru::IO
df1 = Daru::IO::Importers::HTML.new(match: 'market', name: 'Shares analysis').read('https://some/url/with/tables')
df2 = Daru::IO::Importers::HTML.new(match: 'market', name: 'Shares analysis').read('path/to/file.html')

#! Usage from Daru::DataFrame
df1 = Daru::DataFrame.from_html('https://some/url/with/tables', match: 'market', name: 'Shares analysis')
df2 = Daru::DataFrame.from_html('path/to/file.html', match: 'market', name: 'Shares analysis')
```

## JSON Importer

Imports a `Daru::DataFrame` from a **.json** file / response.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/JSON)
- **Gem Dependencies**: `jsonpath` gem
- **Usage**

```ruby
#! Partially require just JSON Importer
require 'daru/io/importers/json'

#! Usage from Daru::IO
df1 = Daru::IO::Importers::JSON.new(index: '$..time', col1: '$..name', col2: '$..age').read('https://path/to/json/response')
df2 = Daru::IO::Importers::JSON.new(index: '$..time', col1: '$..name', col2: '$..age').read('path/to/file.json')

#! Usage from Daru::DataFrame
df1 = Daru::DataFrame.from_json('https://path/to/json/response', index: '$..time', col1: '$..name', col2: '$..age')
df2 = Daru::DataFrame.from_json('path/to/file.json', index: '$..time', col1: '$..name', col2: '$..age')
```

## Mongo Importer

**Note: The Mongo gem faces Argument Error : expected Proc Argument issue due to the bug in MRI Ruby 2.4.0 mentioned [here](https://bugs.ruby-lang.org/issues/13107). This seems to have been fixed in Ruby 2.4.1 onwards. Hence, please avoid using this Mongo Importer in Ruby version 2.4.0.**

Imports a `Daru::DataFrame` from a Mongo collection.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/Mongo)
- **Gem Dependencies**: `jsonpath` and `mongo` gems
- **Other Dependencies**: Install MongoDB
- **Usage**

```ruby
#! Partially require just Mongo Importer
require 'daru/io/importers/mongo'

#! Usage from Daru::IO
df = Daru::IO::Importers::Mongo.new('cars').from('mongodb://127.0.0.1:27017/test')

#! Usage from Daru::DataFrame
df = Daru::DataFrame.from_mongo('mongodb://127.0.0.1:27017/test', 'cars')
```

## Plaintext Importer

Imports a `Daru::DataFrame` from a **.dat** plaintext file (space separated table of simple strings and numbers). For a sample format of the plaintext file, have a look at the example [bank2.dat](https://github.com/athityakumar/daru-io/blob/master/spec/fixtures/plaintext/bank2.dat) file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/Plaintext)
- **Usage**

```ruby
#! Partially require just Plaintext Importer
require 'daru/io/importers/plaintext'

#! Usage from Daru::IO
df = Daru::IO::Importers::Plaintext.new([:col1, :col2, :col3]).read('path/to/file.dat')

#! Usage from Daru::DataFrame
df = Daru::DataFrame.from_plaintext('path/to/file.dat', [:col1, :col2, :col3])
```

## RData Importer

Imports a `Daru::DataFrame` from a variable in **.rdata** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/RData)
- **Gem Dependencies**: `rsruby` gem
- **Other Dependencies**: Install R and set `R_HOME` variable as given in the [Contribution Guidelines](https://github.com/athityakumar/daru-io/blob/master/CONTRIBUTING.md)
- **Usage**

```ruby
#! Partially require just RData Importer
require 'daru/io/importers/r_data'

#! Usage from Daru::IO
df = Daru::IO::Importers::RData.new('ACS3').read('path/to/file.RData')

#! Usage from Daru::DataFrame
df = Daru::DataFrame.from_rdata('path/to/file.RData', 'ACS3')
```

## RDS Importer

Imports a `Daru::DataFrame` from a **.rds** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/RDS)
- **Gem Dependencies**: `rsruby` gem
- **Other Dependencies**: Install R and set `R_HOME` variable as given in the [Contribution Guidelines](https://github.com/athityakumar/daru-io/blob/master/CONTRIBUTING.md)
- **Usage**

```ruby
#! Partially require just RDS Importer
require 'daru/io/importers/rds'

#! Usage from Daru::IO
df = Daru::IO::Importers::RDS.new.read('path/to/file.rds')

#! Usage from Daru::DataFrame
df = Daru::DataFrame.from_rds('path/to/file.rds')
```

## Redis Importer

Imports a `Daru::DataFrame` from **Redis** key(s).

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/Redis)
- **Gem Dependencies**: `redis` gem
- **Other Dependencies**: Install Redis, and run an instance of `redis-server`
- **Usage**

```ruby
#! Partially require just Redis Importer
require 'daru/io/importers/redis'

#! Usage from Daru::IO
df = Daru::IO::Importers::Redis.new(match: 'time:1*', count: 1000).from({url: 'redis://:password@host:port/db'})

#! Usage from Daru::DataFrame
df = Daru::DataFrame.from_redis({url: 'redis://:password@host:port/db'}, match: 'time:1*', count: 1000)
```

## SQL Importer

Imports a `Daru::DataFrame` from a **sqlite.db** file / **DBI** connection.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/SQL)
- **Gem Dependencies**: `dbd-sqlite3`, `activerecord`, `dbi` and `sqlite3` gems
- **Usage**

```ruby
#! Partially require just SQL Importer
require 'daru/io/importers/sql'

#! Usage from Daru::IO
df1 = Daru::IO::Importers::SQL.new('SELECT * FROM test').read('path/to/file.sqlite')
df2 = Daru::IO::Importers::SQL.new('SELECT * FROM test').from(dbi_connection)

#! Usage from Daru::DataFrame
df1 = Daru::DataFrame.read_sql('path/to/file.sqlite', 'SELECT * FROM test')
df2 = Daru::DataFrame.from_sql(dbi_connection, 'SELECT * FROM test')
```

# Exporters

The `Daru::IO` Exporters are intended to 'migrate' a `Daru::DataFrame` into a file, or database. All Exporters can be called in two ways - from `Daru::IO` or `Daru::DataFrame`.

```ruby
#! Partially requires Format Exporter
require 'daru/io/exporters/format'

#! Usage from Daru::IO
instance = Daru::IO::Exporters::Format.new(df, opts)
instance.to_s #=> Provides a file-writable string, which can be used in web applications for file download purposes
instance.to #=> Provides a Format instance
instance.write(path) #=> Writes to the given path

#! Usage from Daru::DataFrame
string = df.to_format_string(opts) #=> Provides a file-writable string, which can be to write into a file later
instance = df.to_format(opts) #=> Provides a Format instance
df.write_format(path, opts) #=> Writes to the given path
```

*Note: Please have a look at the respective Exporter documentations links below, for more information about arguments and examples.*

## Avro Exporter

Exports a `Daru::DataFrame` into a **.avro** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Exporters/Avro)
- **Gem Dependencies**: `avro` gem
- **Usage**

```ruby
#! Partially require just Avro Exporter
require 'daru/io/exporters/avro'

avro_schema = {
  'type' => 'record',
  'name' => 'Example',
  'fields' => [
    {'name' => 'col_1', 'type' => 'string'},
    {'name' => 'col_2', 'type' => 'int'},
    {'name' => 'col_3', 'type'=> 'boolean'}
  ]
}

#! Usage from Daru::IO
string = Daru::IO::Exporters::Avro.new(df, avro_schema).to_s
Daru::IO::Exporters::Avro.new(df, avro_schema).write('path/to/file.avro')

#! Usage from Daru::DataFrame
string = df.to_avro_string(avro_schema)
df.write_avro('path/to/file.avro', avro_schema)
```

## CSV Exporter

Exports a `Daru::DataFrame` into a **.csv** or **.csv.gz** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Exporters/CSV)
- **Usage**

```ruby
#! Partially require just CSV Exporter
require 'daru/io/exporters/csv'

#! Usage from Daru::IO
csv_string = Daru::IO::Exporters::CSV.new(df, converters: :numeric, convert_comma: true).to_s
Daru::IO::Exporters::CSV.new(df, converters: :numeric, convert_comma: true).write('path/to/file.csv')
csv_gz_string = Daru::IO::Exporters::CSV.new(df, converters: :numeric, compression: :gzip, convert_comma: true).to_s
Daru::IO::Exporters::CSV.new(df, converters: :numeric, compression: :gzip, convert_comma: true).write('path/to/file.csv.gz')

#! Usage from Daru::DataFrame
csv_string = df.to_csv_string(converters: :numeric, convert_comma: true)
df.write_csv('path/to/file.csv', converters: :numeric, convert_comma: true)
csv_gz_string = df.to_csv_string(converters: :numeric, compression: :gzip, convert_comma: true)
df.write_csv('path/to/file.csv.gz', converters: :numeric, compression: :gzip, convert_comma: true)
```

## Excel Exporter

Exports a `Daru::DataFrame` into a **.xls** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Exporters/Excel)
- **Gem Dependencies**: `spreadsheet` gem
- **Usage**

```ruby
#! Partially require just Excel Exporter
require 'daru/io/exporters/excel'

#! Usage from Daru::IO
string = Daru::IO::Exporters::Excel.new(df, header: {color: :red, weight: :bold}, data: {color: :blue }, index: false).to_s
Daru::IO::Exporters::Excel.new(df, header: {color: :red, weight: :bold}, data: {color: :blue }, index: false).write('path/to/file.xls')

#! Usage from Daru::DataFrame
string = df.to_excel_string(header: {color: :red, weight: :bold}, data: {color: :blue }, index: false)
df.write_excel('path/to/file.xls', header: {color: :red, weight: :bold}, data: {color: :blue }, index: false)
```

## JSON Exporter

Exports a `Daru::DataFrame` into a **.json** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Exporters/JSON)
- **Gem Dependencies**: `jsonpath` gem
- **Usage**

```ruby
#! Partially require just JSON Exporter
require 'daru/io/exporters/json'

#! Usage from Daru::IO
hashes = Daru::IO::Exporters::JSON.new(df, orient: :records, pretty: true, name: '$.person.name', age: '$.person.age').to
string = Daru::IO::Exporters::JSON.new(df, 'path/to/file.json', orient: :records, pretty: true, name: '$.person.name', age: '$.person.age').to_s
Daru::IO::Exporters::JSON.new(df, orient: :records, pretty: true, name: '$.person.name', age: '$.person.age').write('path/to/file.json')

#! Usage from Daru::DataFrame
hashes = df.to_json('orient: :records, pretty: true, name: '$.person.name', age: '$.person.age')
string = df.to_json_string(orient: :records, pretty: true, name: '$.person.name', age: '$.person.age')
df.write_json('path/to/file.json', orient: :records, pretty: true, name: '$.person.name', age: '$.person.age')
```

## RData Exporter

Exports multiple `Daru::DataFrame`s into a **.rdata** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Exporters/RData)
- **Gem Dependencies**: `rsruby` gem
- **Other Dependencies**: Install R and set `R_HOME` variable as given in the [Contribution Guidelines](https://github.com/athityakumar/daru-io/blob/master/CONTRIBUTING.md)
- **Usage**

```ruby
#! Partially require just RData Exporter
require 'daru/io/exporters/r_data'

#! Usage from Daru::IO
string = Daru::IO::Exporters::RData.new('first.df': df1, 'second.df': df2).to_s
Daru::IO::Exporters::RData.new('first.df': df1, 'second.df': df2).write('path/to/file.RData')
```

## RDS Exporter

Exports a `Daru::DataFrame` into a **.rds** file.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Exporters/RDS)
- **Gem Dependencies**: `rsruby` gem
- **Other Dependencies**: Install R and set `R_HOME` variable as given in the [Contribution Guidelines](https://github.com/athityakumar/daru-io/blob/master/CONTRIBUTING.md)
- **Usage**

```ruby
#! Partially require just RDS Exporter
require 'daru/io/exporters/rds'

#! Usage from Daru::IO
string = Daru::IO::Exporters::RDS.new(df, 'sample.dataframe').to_s
Daru::IO::Exporters::RDS.new(df, 'sample.dataframe').write('path/to/file.rds')

#! Usage from Daru::DataFrame
string = df.to_rds_string('sample.dataframe')
df.write_rds('path/to/file.rds', 'sample.dataframe')
```

## SQL Exporter

Exports a `Daru::DataFrame` into a database (SQL) table through DBI connection.

- **Docs**: [rubydoc.info](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Exporters/SQL)
- **Gem Dependencies**: `dbd-sqlite3`, `dbi` and `sqlite3` gems
- **Other Dependencies**: Install SQL database server
- **Usage**

```ruby
#! Partially require just SQL Exporter
require 'daru/io/exporters/sql'

#! Usage from Daru::IO
Daru::IO::Exporters::SQL.new(df, DBI.connect('DBI:Mysql:database:localhost', 'user', 'password'), 'cars_table').to

#! Usage from Daru::DataFrame
df.to_sql(DBI.connect('DBI:Mysql:database:localhost', 'user', 'password'), 'cars_table')
```

## Creating a new Daru-IO module

**Daru-IO** currently supports various Import / Export methods, as it can be seen from the above list. But the list is NEVER complete - there may always be specific use-case format(s) that you need very badly, but might not fit the needs of majority of the community. In such a case, don't worry - you can always tweak (aka monkey-patch) daru-io in your application. The architecture of `daru-io` provides a neater way of monkey-patching into `Daru::DataFrame` to support your unique use-case.

 - **Adding a new Importer to Daru-IO**

```ruby
#! YAML Importer

module Daru
  module IO
    module Importers
      class YAML < Base
        Daru::DataFrame.register_io_module :from_yaml, self
        Daru::DataFrame.register_io_module :read_yaml, self

        def initialize(opts)
          optional_gem 'gem_dependency_to_parse_yaml'

          @opts = opts
        end

        def from(instance)
          #! Your code to create Daru::DataFrame
          #! from given YAML instance
        end

        def read(path)
          #! Your code to read the YAML file
          #! and create Daru::DataFrame
        end
      end
    end
  end
end

df = Daru::DataFrame.read_yaml('path/to/file.yaml', skip: 10)
# or,
df = Daru::IO::Importers::YAML.new(skip: 10).read('path/to/file.yaml')
```

 - **Adding a new Exporter to Daru-IO**

```ruby
#! YAML Exporter

module Daru
  module IO
    module Exporters
      class YAML < Base
        Daru::DataFrame.register_io_module :to_yaml, self
        Daru::DataFrame.register_io_module :to_yaml_string, self
        Daru::DataFrame.register_io_module :write_yaml, self

        def initialize(dataframe, opts)
          optional_gem 'gem_dependency_to_write_yaml'

          super(dataframe) #! Have a look at documentation of Daru::IO::Exporters::Base#initialize
          @opts = opts
        end

        def to
          #! Your code to return a YAML instance
        end

        def to_s
          super
          #! By default, Exporters::Base adds this to_s method to all Exporters,
          #! by making the write mthod to write to a tempfile, and then reading it.
        end

        def write(path)
          #! Your code to write the YAML file
          #! with the data in the @dataframe
        end
      end
    end
  end
end

df = Daru::DataFrame.new(x: [1,2], y: [3,4])

df.to_yaml(rows: 10..19) #! or, Daru::IO::Exporters::YAML.new(df, rows: 10..19).to
df.to_yaml_string(rows: 10..19) #! or, Daru::IO::Exporters::YAML.new(df, rows: 10..19).to_s
df.write_yaml('dataframe.yml', rows: 10..19) #! or, Daru::IO::Exporters::YAML.new(df, rows: 10..19).write('dataframe.yml')
```

## Acknowledgements

I'm very thankful to mentors [Victor Shepelev](https://github.com/zverok),
[Sameer Deshmukh](https://github.com/v0dro) and [Lokesh Sharma](https://github.com/lokeshh)
for their timely Pull Request reviews and open discussions regarding features. Daru-IO
wouldn't have been possible without them and the members of Ruby Science Foundation,
who provided their useful feedback(s) whenever they could. The community has been very
supportive overall, and I'd be interested to involve with SciRuby via more open-source
projects.