# Islandora Ingest Process
---

## Batch Ingesting Content into Islandora

You can batch ingest objects into the repository in two ways.

**Using the Web Importer**
---
You can use the Batch Ingest web interface.  That can be found on any object that has the **Collection** content model.

  1. In the **Manage** admin area for the collection you want to add objects to click the **Collection** button.![images/collection_button.png](images/collection_button.png)
  2. On the **collection** admin page click the **Batch Import Objects** button.
  3. On the next screen, for **importer** select **ZIP File Importer**.
  4. On the next screen upload the zip file using the file upload form. Below that there will be a fieldset labeled **Contet Model**. Select the content model for the objects you are importing.  If you don't select the right content model the import will fail.
  5. Below that select the namespace.  We currently only use one namespace, **islandora** so you can leave this as the default.
  6. Click **Import** and wait for the batch importer to run.

**Notes**

* The web importer doesn't work well for really large batches or for batch ingests with objects that have a really large file size.
* If you close the browser window while the web importer is running the batch ingest will be stopped and won't be complete.

**Using Drush Commands**
---

I like ingesting using Drush. It's faster and it can run in the background while you do other things.

1. Move the directory containing the metadata and objects to the local or remote directory containing ISLE. I usually put it in the directory that is the symlink (bind-mount) for the Apache container. In the case of our remote ec2 instance I SFTP the files to `/opt/ISLE-Multi-Environment/prod/pld`

2. Either locally or on the remote site shell into the Apache Docker Container.

```sh
docker exex -it isle-apache-prod bash
```

2. In the Apache container navigate to the root directory of the Drupal installation

```sh
cd var/www/html
```

3. Run the drush command for the content model you need to ingest. For most objects that will be

```sh
drush -v -u 1 --uri=https://provlibdigital.org islandora_batch_scan_preprocess --content_models=[content model] --parent=islandora:[pid of collection you are ingesting into] --parent_relationship_pred=isMemberOfCollection --namespace=islandora --type=directory --scan_target=/var/www/html/[name of directory with files to ingest]
```

4. Let that run. It usually only takes a second.
5. Once that command has finished you are ready to process the batch. Run this command

```sh
drush -v -u 1 --uri=https://provlibdigital.org islandora_batch_ingest
```

6. This will start the batch process. When this runs it first creates the new islandora objects. You'll see a bunch of messages printed to the terminal that say something like `ingesting islandora:pid-number` for each object. Depending on what time of item you are ingesting a number of messages will print to the terminal indicating that the derivatives for the object are being created.

7. When the ingest has finished you can visit the [Islandora Batch Set Reports](https://provlibdigital.org/admin/reports/islandora_batch_sets) to check the status of the items that were ingested. This is where you would go to look for error messages.

**Variations on drush islandora batch commands**

Different content models have slightly different drush commands for batch ingest.

**Islandora Compound Object srush ingest command**

```sh
drush -v --user=isle islandora_compound_batch_preprocess --scan_target=/var/www/html/[folder or zip file] --namespace=islandora --parent=islandora:[collection identifier]
```

## Bulk Deleting Objects in islandora

:link: [Islandora Bulk Delete Module](https://github.com/mjordan/islandora_bulk_delete)

You can bulk delete objects from the repository using a drush command. The alternative is to do it manually via a Drupal admin. That can be slow and tedious...

To use the **iChainsaw** drush command you need to prepare a .txt file that contains all of the pids for objects in the repository you want to delete.

**Example structure of that file - mypidfile.txt**

```
islandora:001
islandora:002
islandora:003
etc...

```

Move that .txt file to the root directory of the Drupal installation and then issue this drush command

```sh
drush iChainsaw --user=isle --pid_file=/var/www/html/mypidfile.txt
```
