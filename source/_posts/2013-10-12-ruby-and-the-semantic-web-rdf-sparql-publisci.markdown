---
layout: post
title: "Ruby and the Semantic Web, RDF, and SPARQL: PubliSci"
date: 2013-10-12 16:36
comments: true
categories:
author: Will Strinz
categories: [GSOC2013,GSOC,Data mining,PubliSci,RDF,SPARQL,Semantic Web]
---
<p class="note"><strong>Editor's Note:</strong> This is the second of four blog posts detailing our Google Summer of
Code 2013 students' work. I edited it to include a very incomplete list of public RDF repositories. &mdash;John Woods</p>

Introduction
------------
Across all fields and disciplines, contemporary scientists are faced with a massive and growing volume of data. It often
won't fit in a lab notebook, and there is a pressing need to share it more quickly and widely than publication in a
journal article would allow for. Database software is one great solution for storage of such data, but relational
databases become brittle in the face of changes or new information, do not play nicely with other databases or data
derived from such databases, and may not be fully machine (or human) readable without pre-existing knowledge.

Meanwhile, the Internet is an extremely useful place to discover and share useful information, but it is essentially
built around linked documents, rather than pure data, and so our primary mechanism for sharing data is as HTML or text.

RDF and related technologies propose to provide the means to move beyond a web of documents to a web of data. Along
the way, these technologies may address many of the problems with conventional relational databases (e.g., SQL). At its
core, RDF defines an extremely flexible method for representing data on the web &mdash; which is nonetheless
unambiguously defined, without any external context, and can be linked to other data as web documents link to each
other. Because RDF data can be understood as either a set of subject&ndash;predicate&ndash;object statements or a
directed graph with labeled edges, a number of supporting standards and tools that have grown up around it to provide
powerful storage and access methods that are often easier to implement and use than those associated with relational
databases and the document-based web.

Enter PubliSci
--------------

This summer I created a Ruby gem, [PubliSci](http://github.com/sciruby/publisci), to facilitate data publication and interaction using
the [Semantic Web](https://en.wikipedia.org/wiki/Semantic_web). The format offers a unified way to share and combine
information from multiple sources, support for machine learning tools,
a [flexible query language](https://en.wikipedia.org/wiki/SPARQL) that makes application integration easy, and the
backing of the World Wide Web Consortium (W3C) and other standards-setting bodies.

The PubliSci gem comprises a set of parsers for converting various input formats using
the [RDF Data Cube vocabulary](http://semanticweb.com/w3c-publishes-last-call-working-drafts-for-rdf-data-cube-dcat_b35950), and
a Ruby interface for defining new ones. Since the relationship between external datasets and semantic web formats is
sometimes up to interpretation, a domain-specific language is included to allow end users to resolve ambiguities and
provide additional metadata.

Along with the conversion tool, a standalone server is available as an extension to the gem that simplifies setting up and interacting
with RDF data stores. The server allows import, export, querying, and management of external [triplestores](https://en.wikipedia.org/wiki/Triplestore) such as
[4store](http://4store.org/), and supports both cross-domain access and content negotiation so the gem can be accessed
using Javascript or other applications.

<p class="note"><strong>Triplestores</strong> are databases for the storage and retrieval of <strong>triples</strong>,
which are typically subject&ndash;object&ndash;predicate relationships (e.g., <i>Bob knows Fred</i>).</p>

If you'd like to contribute, the [source code is available on Github](https://github.com/sciruby/publisci), and [a broad
outline of the to do list can be had as well](http://gsocsemantic.wordpress.com/2013/08/05/bio-publisci/).

Usage
-----

Once you've done `gem install publisci`, you can require the gem in the normal way (`require 'publisci'`). To invoke the
domain-specific language, you'll also want to include the `DSL` module:

    require 'publisci'
    include PubliSci::DSL

Input data can be specified like so:

    # Specify input data
    data do
      # Use local or remote paths to point to the data file you want to load:
      source 'https://github.com/wstrinz/publisci/raw/master/spec/csv/bacon.csv'

      # Specify datacube properties.
      dimension 'producer', 'pricerange'
      measure 'chunkiness'

      # Set parser-specific options.
      option 'label_column', 'producer'
    end

You can provide meta-data on your dataset as well.

    metadata do
      dataset 'bacon'
      title 'Bacon dataset'
      creator 'Will Strinz'
      description 'some data about bacon'
      date '1-10-2010'
    end

Sending the data to a repository is simple.

    # Send output to an RDF::Repository
    #  can also use 'generate_n3' to output a turtle string
    repo = to_repository

SPARQL queries can be run on the dataset using the `QueryHelper` module.

    # run SPARQL queries on the dataset
    PubliSci::QueryHelper.execute('select * where {?s ?p ?o} limit 5', repo)

Finally, data can be exported in other formats, such as ARFF:

    # export in other formats
    PubliSci::Writers::ARFF.new.from_store(repo)

Some places to look for RDF repositories
----------------------------------------
* Social sciences and economics: [PlanetData wiki: Datasets](http://wiki.planet-data.eu/web/Datasets), [270a Linked Dataspaces](http://270a.info/)
* Chemistry: [Resource description framework technologies in chemistry](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3118380/)
* Molecular biology: [EMBL-EBI: Current RDF Resources](http://www.ebi.ac.uk/rdf/), [Bio2RDF](http://bio2rdf.org/),
  [Gene Ontology](http://www.geneontology.org/GO.format.rdfxml.shtml)
* Ecology: [BBC Wildlife Finder](http://www.bbc.co.uk/nature/feedsanddata)
* General: [Data Incubator: Growing the Web of Data](http://dataincubator.org/)




