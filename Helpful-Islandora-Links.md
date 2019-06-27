# Helpful Islandora Links
---
:link: [Getting Started With Islandora](https://wiki.duraspace.org/display/ISLANDORA/Getting+Started+with+Islandora)

Wiki for Islandora project. This has some useful information but can be outdated...

:link: [Islandora Awesome](https://github.com/Islandora-Labs/islandora_awesome)

A list of helpful solution packs and modules for islandora.

:link: [Islandora Cookbook](https://github.com/manez/Islandora-Cookbook)

Modules and tools for Islandora 8. We're not on 8 currently, but eventually you will need to migrate up to 8. This should hopefully ease that process.

:link: [Islandora Compound Batch](https://github.com/MarcusBarnes/islandora_compound_batch)

Explains how to create a directory structure for batch ingesting compound objects into Islandora.

:link: [Islandora Datastream Crud](https://github.com/SFULibrary/islandora_datastream_crud)

We use this module to batch delete, reingest, or add datastreams to objects already in the repository. This module is really handy.  **NOTE** I modified the code of this repo to spit out the pid and local identifier of each object when running the drush command `islandora_datastream_crud_fetch_pids`. Having both is really helpful when revising XML files in batch and then using shell scripts to batch rename them to the islandora pid for pushing using this module.

:link: [Islandora Ingest Cookbook](https://github.com/MarcusBarnes/mik/wiki/Cookbook:-Importing-your-packages-into-Islandora)

This shows you how to prepare objects for batch ingest via either drush commands or web interface.  Covers a number of common content models.

:link: [Islandora Web Annotations](https://github.com/digitalutsc/islandora_web_annotations)

A module for adding web annotations to objects. I've tested this locally. Looks like a good candidate for building a web annotation feature on top of.

:link: [SparQL 101](https://github.com/whikloj/Sparql_101)

Helpful repo that covers how to query Fedora using SparQL queries.

:link: [Working with Fedora Objects Programmatically Via Tuque](https://github.com/Islandora/islandora/wiki/Working-With-Fedora-Objects-Programmatically-Via-Tuque)

Really useful cookbook that shows how to use the Tuque PHP library to get objects, their datastreams, add objects to a repository programmatically, delete objects, etc...

:link: [Islandora REST API](https://github.com/discoverygarden/islandora_rest)

An example of a RESTful API implementation for Islandora.

[Converting Spreadsheets into MODSXML using Open Refine](http://digitalscholarship.utsc.utoronto.ca//content/blogs/converting-spreadsheets-modsxml-using-open-refine)

An example of how to use Open Refine to generate MODS XML files.
