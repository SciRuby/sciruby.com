---
layout: post
title: "GSoC 2017 : Support to Import & Export of more formats"
date: 2017-08-29 17:16
comments: true
author: Athitya Kumar
categories: [GSOC 2017, GSOC, Daru, Daru-IO, Import, Export]
---

## Introduction

> "Hello friend. Hello friend? That's lame." - S01E01 (Pilot), Mr.Robot

My name is Athitya Kumar, and I'm a 4th year undergrad from IIT Kharagpur, India. I
was selected as a GSoC 2017 student developer by Ruby Science Foundation for project daru-io.

[Daru-IO](https://github.com/athityakumar/daru-io) is a plugin-gem to
[Daru](https://github.com/SciRuby/daru) gem, that extends support for many Import and Export
methods of `Daru::DataFrame`. This gem is intended to help Rubyists who are into Data Analysis
or Web Development, by serving as a general purpose conversion library.

Through this summer, I worked on adding support for various Importers and Exporters
while also porting some existing modules. Feel free to find a comprehensive set
of useful links in 
[Final Work Submission](https://athityakumar.github.io/blog/posts/GSoC_2017_-_Final_Work_Submission/)
and [README](https://github.com/athityakumar/daru-io/blob/master/README.md). Before proceeding any
further, you might also be interested in checking out a sample showcase of
[Rails example](https://daru-examples-io-view-rails.herokuapp.com/) and
[the code](https://github.com/Shekharrajak/daru_examples_io_view_rails) making it work.

## Mark Anthony's Speech (ft. daru)

> "Rubyists, Data Analysts and Web Developers, lend me your ears;

> I come to write about my GSoC project, not to earn praise for it."

For the uninitiated, Google Summer of Code (GSoC) 2017 is a 3-month program that
focuses on introducing selected students to open-source software development.
To know more about GSoC, feel free to click
[here](https://summerofcode.withgoogle.com/about/).

[daru](https://github.com/SciRuby/daru) is a Ruby gem that stands for Data Analysis in RUby. My
[initial proposal](https://drive.google.com/file/d/0B4shBmKvdinIZUNqcGwtWDZtb1NMMHg3SG51SFRqNlVqZkIw/)
was to make daru easier to integrate with Ruby web frameworks through better import-export features
([daru-io](https://github.com/athityakumar/daru-io)) and visualization methods
([daru-view](https://github.com/Shekharrajak/daru-view)). However, as both 
[Shekhar](https://github.com/Shekharrajak) and I
were selected for the same proposal, we split this amongst ourselves : daru-io was allocated to me
and daru-view was allocated to [Shekhar](https://github.com/Shekharrajak).

## 

> "The open-source contributions that people do, live after them;

> But their private contributions, are oft interred with their bones."

This is one of the reasons why I (and all open-source developers) are enthusiastic
about open-source. In open-source, one's work can be re-used in other projects
in accordance with the listed LICENSE and attribution, compared to the restrictions
and risk of Intellectual Property Right claims in private work.

## 

> "So be it. The noble Pythonistas and R developers;

> Might not have chosen to try daru yet."

It is quite understandable that Pythonistas and R developers feel that their corresponding
languages have sufficient tools for Data Analysis. So, why would they switch to Ruby and
start using daru?

## 

> "If it were so, it may have been a grievous fault;

> Give daru a try, with daru-io and daru-view."

First of all, I don't mean any offense when I say "grievous fault". But please, do give
Ruby and daru family a try, with an open mind.

Voila - the daru family has two new additions, namely
[daru-io](https://github.com/athityakumar/daru-io) and
[daru-view](https://github.com/Shekharrajak/daru-view). Ruby is a language
which is extensively used in Web Development with multiple frameworks such as Rails, Sinatra,
Nanoc, Jekyll, etc. With such a background, it only makes sense for daru to have daru-io and
daru-view as separate plugins, thus making the daru family easily integrable with Ruby web
frameworks.

## 

> "Here, for attention of Rubyists and the rest–

> For Pandas is an honourable library;

> So are they all, all honourable libraries and languages–

> Come I to speak about daru-io's inception."

Sure, the alternatives in other languages like Python, R and Hadoop are also good data analysis
tools. But, how readily can they be integrated into any web application? R & Hadoop don't have a
battle-tested web framework yet, and are usually pipelined into the back-end of any application
to perform any analysis. I'm no one to judge such pipelines, but I feel that pipelines are
hackish workarounds rather than being a clean way of integrating.

Meanwhile, though Python too has its own set of web frameworks (like Django, Flask and more),
Pandas doesn't quite work out-of-the-box with these frameworks and requires the web developer
to write lines and lines of code to integrate Pandas with parsing libraries and plotting
libraries.

## 

> "daru-io is a ruby gem, and open-sourced to all of us;

> But some might think it was an ambitious idea;

> And they are all honourable men."

As described above, daru-io is open-sourced under the MIT License with attribution to
myself and Ruby Science Foundation. Being a ruby gem, daru-io follows the best practices
mentioned in the Rubygems guides and is all geared up with a v0.1.0 release.

Disclaimer - By "men", I'm not stereotyping "them" to be all male, but I'm just merely
retaining the resemblence to the original speech of Mark Anthony.

## 

> "daru-io helps convert data in many formats to Daru::DataFrame;

> Whose methods can be used to analyze huge amounts of data.

> Does this in daru-io seem ambitious?"

[Daru](https://github.com/SciRuby/daru) has done a great job of encapsulating the two main
structures of Data Analysis - DataFrames and Vectors - with a ton of functionalities that are
growing day by day. But obviously, the huge amounts of data aren't going to be manually fed into
the DataFrames right?

One part of [daru-io](https://github.com/athityakumar/daru-io) is the battalion of Importers that
ship along with it. Importers are used to read from a file / Ruby instance, and create DataFrame(s).
These are the Importers being supported by v0.1.0 of daru-io : 

  - General file formats : CSV, Excel (xls and xlsx), HTML, JSON, Plaintext.
  - Special file formats : Avro, RData, RDS.
  - Database related : ActiveRecord, Mongo, Redis, SQLite, DBI.

For more specific information about the Importers, please have a look at the 
[README](https://github.com/athityakumar/daru-io/blob/master/README.md#table-of-contents)
and [YARD Docs](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Importers/).

Let's take a simple example of the JSON Importer, to import from GitHub's GraphQL API response. By
default, the API response is paginated and 30 repositories are listed in the url : 
`https://api.github.com/users/#{username}/repos`.

```ruby
require 'daru/io/importers/json'

dataframe = %w[athityakumar zverok v0dro lokeshh].map do |username|
  Daru::DataFrame.read_json(
    "https://api.github.com/users/#{username}/repos",
    RepositoryName: '$..full_name',
    Stars: '$..stargazers_count',
    Size: '$..size',
    Forks: '$..forks_count'
  )
end.reduce(:concat)

#=> #<Daru::DataFrame(120x4)>
#      Repository   Stars   Size   Forks
#   0  athityakum       0      6       0
#   1  athityakum       0    212       0
#   2  athityakum       0    112       0
#  ...    ...          ...   ...     ...
```

## 

> "When working with a team of Pythonistas and R developers;

> daru-io helps convert Daru::DataFrame to multiple formats.

> Does this in daru-io seem ambitious?

The second part of [daru-io](https://github.com/athityakumar/daru-io) is the collection of Exporters
that ship with it. Exporters are used to write the data in a DataFrame, to a file / database. These
are the Exporters being supported by v0.1.0 of daru-io :

  - General file formats : CSV, Excel (xls), JSON.
  - Special file formats : Avro, RData, RDS.
  - Database related : SQL.

For more specific information about the Exporters, please have a look at the 
[README](https://github.com/athityakumar/daru-io/blob/master/README.md#table-of-contents)
and [YARD Docs](http://www.rubydoc.info/github/athityakumar/daru-io/master/Daru/IO/Exporters/).

Let's take a simple example of the RDS Exporter. Say, your best friend is a R developer who'd like
to analyze a `Daru::DataFrame` that you have obtained, and perform further analysis. You don't want
to break your friendship, and your friend is skeptical of learning Ruby. No issues, simply use the RDS
Exporter to export your `Daru::DataFrame` into a .rds file, which can be easily loaded by your friend
in R.

```ruby
require 'daru/io/exporters/rds'

dataframe #! Say, the DataFrame is obtained from the above JSON Importer example

#=> #<Daru::DataFrame(120x4)>
#      Repository   Stars   Size   Forks
#   0  athityakum       0      6       0
#   1  athityakum       0    212       0
#   2  athityakum       0    112       0
#  ...    ...          ...   ...     ...

dataframe.write_rds('github_api.rds', 'github.api.dataframe')
```

## 

> "You all did see that in the repository's README;

> Codeclimate presented a 4.0 GPA;

> Code and tests were humbly cleaned;

> with help of rubocop, rspec, rubocop-rspec and saharspec.

> Ambition shouldn't have been made of humble stuff.

> Yet some might think it is an ambitious idea;

> And sure, they are all honourable men."

Thanks to guidance from my mentors
[Victor Shepelev](https://github.com/zverok), [Sameer Deshmukh](https://github.com/v0dro)
and [Lokesh Sharma](https://github.com/lokeshh), I've come to know about quite a lot
of Ruby tools that could be used to keep the codebase sane and clean.

- [rubocop](https://github.com/bbatsov/rubocop) : A Ruby static code analyzer, which enforces
  specified Ruby style guidelines.
- [rspec](https://github.com/rspec/rspec) : A unit-testing framework, which makes sure that codes
  of block are doing what they're logically supposed to do.
- [rubocop-rspec](https://github.com/backus/rubocop-rspec) : A plugin gem to rubocop, that extends
  rspec-related rules.
- [saharspec](https://github.com/zverok/saharspec) : A gem with a
  [punny name](https://github.com/zverok/saharspec/blob/master/README.md#saharspec-specs-dry-as-sahara), that extends a few features to rspec-its that are more readable. For example, `its_call`.

## 

> "I speak not to disapprove of what other libraries do;

> But here I am to speak what I do know.

> Give daru-io a try and y'all will love it, not without cause:

> Does anything withhold you then, from using daru-io?"

I really mean it, when I discretely specify "I speak not to disapprove of what other libraries do".
In the world of open-source, there should never be hate among developers regarding languages,
or libraries. Developers definitely have their (strong) opinions and preferences, and it's
understandable that difference in opinion do arise. But, as long as there's mutual respect for
each other's opinion and choice, all is well.

## 

> "O Ruby community! Thou should definitely try out daru-io,

> With daru and daru-view. Bear with me;

> My heart is thankful to the community of Ruby Science Foundation,

> And I must pause till I write another blog post."

If you've read all the way till down here, I feel that you'd be interested in trying out the
daru family, after having seen the impressive demonstration of Importers & Exporters above, and
the Rails example ([Website](https://daru-examples-io-view-rails.herokuapp.com/) |
[Code](https://github.com/Shekharrajak/daru_examples_io_view_rails)). I'm very thankful to mentors
[Victor Shepelev](https://github.com/zverok), [Sameer Deshmukh](https://github.com/v0dro)
and [Lokesh Sharma](https://github.com/lokeshh) for their timely Pull Request reviews and open
discussions regarding features. Daru-IO would not have been possible without them and the active
community of Ruby Science Foundation, who provided their useful feedback(s) whenever they could.
The community has been very supportive overall, and hence I'd definitely be interested to involve
with SciRuby via more open-source projects.