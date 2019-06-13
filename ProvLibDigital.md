## Islandora / ISLE / DOCKER / ProvLibDigital Information

This directory contains the local version of our digital repository [ProvLibDigital](https://provlibdigital.org).

ProvLibDigital (**PLD**) is built on the Islandora Respository framework and uses [Docker](https://www.docker.com/) as an application deployment tool.

Docker is great and well worth learning about. There's lots to say about Docker but most important to you are the specific docker commands you'll need to use to interact with our Islandora instance both locally and on our remote server (hosted on Amazon EC2).

### Docker Commands You'll Need to Know ###

Docker containers can be spun up, interacted with (ex: ssh into a container), brought down, etc..., through the use of some Docker specific commands used on the command line.

`docker-compose up -d` - Run this command in the root directory _(this directory)_ to launch docker and spin up all containers associated with our Islandora instance. The file `docker-compose.yml` is worth reading through, if only to see what containers are being started and running when you run this command.
the `-d` flag indicates that you want to spin up docker in a detached state, so docker will run in the background.

`docker ps a` - run this command to see what containers are up and running. You can also get the container name from running this command. Knowing the container name is helpful if you want to run a command to interact with a specific container.

`docker exec -it [container-name] bash` - run this command to start a bash session in `[container-name]`. I mostly use this to run drush commands in the Apache container both locally and on our remote installation. The batch ingest process I use utilizes a few different drush commands. You can also use this command to rebuild the solr index inside the solr container and rebuild the fedora repository in the fedora container.

`docker-compose down` - bring down the docker containers currently runnging. You'll mostly do this locally when you're done local development work for PLD. You should run this command on the remote if you want to update the docker containers (pull the latest versions then restart).

## IMPORTANT NOTE ##
`docker compose down -v` - This command will pull down any running docker containers and erase any persisting data in those containers. Basically this will erase all data saved in the Apache, Fedora, and Solr containers and restore them to a fresh install of ISLE. We have backups of our EC2 instance you can use in the event that the docker containers get wiped. I made a backup of the EC2 instance anytime I ingested new content or modified the theme or modules in Drupal.

## Drupal Specific Information ##
#### :file_folder: PLD Site theme

**Theme Directory:** `pld/sites/all/themes/pld`

The `pld` directory is a [bind mount](https://docs.docker.com/storage/bind-mounts/) to the `var/www/html` directory in the docker container for Apache. You can move files into this folder, modify files, interact with it and it will sync to the directory in the docker container.

The theme `pld` is based on a bare bones Drupal starter theme called [Basic](https://www.drupal.org/project/basic/releases/7.x-4.3).

## :file_folder: Islandora Overrides

**Template Directory Location:** `pld/sites/all/pld/templates/`

There are a number of files in this directory named `islandora-[some-template-name].tpl.php`. These are the specific files I used to override the presentation of specific parts of islandora and, in some cases, the data being passes to those templates. The biggest modification is the use of larger image datastreams for collection template files and search template files. In most cases I'm calling the **MEDIUM_SIZE** datastream, which is generated on ingest for Basic Image content model and Compound Content model (parent object only). For the Book Content Model I'm using the **JP2** datastream.
I did this to provide larger image thumbnails in grids since the default Islandora thumbnail size is pretty tiny. There may be a better way to do this such as modifying the Islandora module to adjust the size thumbnail datastream produced.

#### Islandora Templates and What They Do ####

##### islandora-basic-collection-grid.tpl.php #####

This file overrides the default collection grid view. Our theme currently uses SOLR to generate collection grids as well as grids of returned search results.  Please see **islandora-solr-grid.tpl.php**.

##### islandora-basic-collection-wrapper.tpl.php #####

File that generates the structure of a view for any object with the basic collection content model (all collections, root collection).

###### Modifications Made To This Template: ######

_(lines 24 - 29)_ -

```php
<?php
    if(!empty($dc_array['dc:title']['value'])): ?>

      <?php if($dc_array['dc:title']['value'] == 'Top-level Collection'): ?>

        <h2>Browse Collections</h2>

      <?php else: ?>

       <h2><?php print $dc_array['dc:title']['value']; ?></h2>

    <?php endif; ?>
```

I added a conditional to check if title of the current collection is "Top-Level Collection". If so display the `<h2>Browse Collections</h2>` instead.

_(lines 34 - 37)_ -


```php
<?php  if(!$display_metadata && !empty($dc_array['dc:description']['value'])): ?>
    <p><?php print nl2br($dc_array['dc:description']['value']); ?></p>
<?php endif; ?>
```
I added a check to see if the variable `$display_metadata` is not present and if the variable `$dc_array['dc:description']['value']` is not empty. If so, print the description.

##### islandora-basic-collection.tpl.php #####

This isn't currently being used on our theme. No need to spend a lot of time on this one...

##### islandora-basic-image.tpl.php #####

This outputs the display of any object with the basic image content model.

###### Modifications Made To This Template: ######

_(lines 14 - 18)_

```php
<div class="social-sharing-buttons">
      <div class="social-title">Share This Object:</div>
      <?php print $variables['facebook_button']; ?>
      <?php print $variables['twitter_button']; ?>
</div>
```
**Social Media Sharing** - These two variables pass dynamically generated links to share the content on facebook or twitter.

_(lines 25 - 36)_

```php
<?php if (isset($islandora_content) && $variables['under_copyright'] == NULL): ?>
    <div class="islandora-basic-image-content">
      <?php print $islandora_content; ?>
    </div>
<?php endif; ?>

<?php if($variables['under_copyright'] != NULL): ?>
      <div class="copyright-restriction__message">
        <?php print $variables['under_copyright']; ?>
      </div>
    <?php endif; ?>
```
This checks to make sure the variable `$islandora_content` is set and also checks to make sure the `COPYRIGHT` datastream is NULL. If so the `$islandora_content` variable is displayed.

_(`$islandora_content` = the MEDIUM_SIZE img datastream and the html wrapper)_

If the `COPYRIGHT` datastream is present display a message informing the end user that the image is under copyright restriction.

_(lines 42 - 66)_

```php
<?php if($variables['parent_collection'] != NULL): ?>
    <div class="parent-collection-info__wrapper">
      <h3>In Collection: <a href="<?php print $variables['parent_url']; ?>"><?php print $variables['parent_collection']; ?></a></h3>
      <?php if($variables['collection_finding_aid_button'] != NULL) {
        print $variables['collection_finding_aid_button'];
      }  ?>
    </div>
<?php endif; ?>

<div class="islandora-basic-image-download-btns__wrapper">
<?php if(isset($variables['img_btn']) && $variables['under_copyright'] == NULL): ?>
      <?php print $variables['img_btn']; ?>
<?php endif; ?>
<?php if(isset($variables['mods_btn'])): ?>
      <?php print $variables['mods_btn']; ?>
<?php endif; ?>

<?php if(isset($variables['dc_btn'])): ?>
    <?php print $variables['dc_btn']; ?>
<?php endif; ?>

<?php if(isset($variables['marcxml_btn'])): ?>
    <?php print $variables['marcxml_btn']; ?>
<?php endif; ?>
```





**Finding Aids**
We display links to Finding Aid datastreams on object display pages.

**Example:** [https://provlibdigital.org/islandora/object/islandora%3A10070](https://provlibdigital.org/islandora/object/islandora%3A10070)

You can expose a finding aid to an object by adding it to the parent collection as a datastream. To do this just add a new datastream to the parent object called **FINDING_AID**.  The file you upload should be the PDF of the collection finding aid.

**Example of a Parent Object** [https://provlibdigital.org/islandora/object/islandora%3A038-03](https://provlibdigital.org/islandora/object/islandora%3A038-03)

**Images Under Copyright Restriction**

I also use a datastream to check if an image in our repository is under copyright restriction and, if so, display a message stating that.

**Example** [Under Copyright Example](https://provlibdigital.org/islandora/object/islandora%3A26732?solr_nav%5Bid%5D=070bdc4b5f90ae1fc7ca&solr_nav%5Bpage%5D=1&solr_nav%5Boffset%5D=3)

To set an object to Under Copyright status add a datastream called **COPYRIGHT** to the object in question. The datastream can be anything (an empty text file, for example). I've opted to just make a really simple xml file.

`<copyright>Restricted</copyright>`

It really doesn't matter what's in the file. It could be a .jpg of a Garfield comic or a PDF copy of the Mueller Report. The only thing that matters is that the object has a datastream associated with it called **COPYRIGHT**. The presence of a COPYRIGHT datastream is what the object template is looking for. If that datastream file is present the template will display the copyright message.
