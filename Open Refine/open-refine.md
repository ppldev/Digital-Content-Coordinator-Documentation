# Creating MODS XML files in Open Refine

#### Links
[Converting Spreadsheets into MODS XML using Open Refine](http://digitalscholarship.utsc.utoronto.ca/content/blogs/converting-spreadsheets-modsxml-using-open-refine)

[Base MODS XML Open Refine Template](https://gist.github.com/sallain/7604ffb0c155294fcfaf)

[XML Split - a tool for splitting a large XML file into several smaller one](https://metacpan.org/pod/distribution/XML-Twig/tools/xml_split/xml_split)

I haven't used Open Refine to generate XML metadata files, however it is commonly used by other institutions for normalizing metadata and generating XML files for ingests.

The first link is a blog post covering how to use Open Refine to generate XML.

The second is a base MODS Open Refine Template. I have included a mods template in this directory named `pld-mods_template-for-open-refine.xml`. This contains all of the current metadata headers found in our csv file and the mods nodes they correspond to.

The biggest downside to Open Refine is that it doesn't automatically generate individual XML files for each row of a CSV. It generates one large XML file. If you use Open Refine you would need to build or find a tool to split the large XML file into individual files for each row of metadata. The last link is a tool that might be capable of doing that. 
