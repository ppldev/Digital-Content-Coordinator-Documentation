Create XML from metadata csv file.
----------------------------------
You can use the XML batch tool. It’s accessible through MAMP at localhost:8888/xml-tool (start MAMP before navigating to the URL).

### Select your CSV file
1. Select the type of batch you want to prepare.
..* For generating XML files (replacing existing metadata or just generating XML) select basic.
..* For compound (more than one object for metadata file) select compound and enter the number of children (2+)
..* For books select Book Batch and then enter the Book Title and the number of pages in the fieldset below. Ignore the number of child objects option.
__If you want to prepare a directory for batch upload make sure your images are added to the images directory in the xml-tool folder (xml-tool -> images).__

#### File naming conventions for image files: 
..* Compound: object-identifier_[number].jpg -number represents the [number] in sequence of the child object.  
	__Ex: objects with front and back - object-identifier_1.jpg (front) object-identifier_2.jpg (back)__.
..* Book: page_number.jpg - where [number] is the page number in the sequence of the book.

Assuming images are named correctly the image will be moved into the batch directory that was created when XML was generated and renamed according to the the naming convention required for the batch ingest operation you’re looking to use.

### Pull up your islandora docker instance
1. On the command line navigate to /Users/[username]/Documents/ISLE
..* Run the following command `‘docker-compose up -d’`
..* The docker containers for the Islandora installation will launch. You should see a series of messages telling you the docker containers have successfully launched.
2. Once the containers have launched you will need to move the directory containing the XML and image files into the apache docker container (I would zip the container). 
..* Run the following command `‘docker cp [source_path/to/directory] isle-apache-ld:/var/www/html’`
..* That will move the file into the docker container running drupal and place the ingest directory in the drupal root.
3. ssh into the docker apache container
..* Run the following command `‘docker exec -it isle-apache-ld bash’`
4. Navigate to the drupal rood `cd /var/www/html`
5. Run the drush command for the batch ingest process
..* You can find instructions on batch ingesting Book objects here <https://wiki.duraspace.org/display/ISLANDORA/Islandora+Book+Batch>
..* Instructions for ingesting compound objects here <https://github.com/MarcusBarnes/islandora_compound_batch>
..* General batch instructions / a nice "cookbook" reference guide for islandora batch options <https://github.com/MarcusBarnes/mik/wiki/Cookbook:-Importing-your-packages-into-Islandora>

#### Notes
1. The islandora instance on the scanning Mac can be viewed at <https://isle.localdomain>. Ignore any https warnings. ISLE is configured to load under https and comes with a provisional SSL cert. That cert is intended to be replaced and tends to cause browser warnings.
2. The user name and pass for the default admin in ISLE is un: isle pw: isle. 
3. All batch drush commands, at some point, will need the PID for the collection you are adding (an) object(s) to. I set up a collection in the ISLE instance on your MAC with the PID islandora:whaling-logs.
4. I would test this process with a small batch first (a few objects and xml or, in the case of a book, a few pages packaged according to the book batch specifications).
5. The xml-tool I built currently only moves/renames .jpg files. TIFF files or other file formats will be ignored.