## Prerequisites

Create a Python virtual environment and install the prerequisites from the
[requirements](requirements.txt) file:

    virtualenv /tmp/venv
    /tmp/venv/bin/pip install -r requirements.txt


Download OMERO.py from the [OMERO downloads](https://www.openmicroscopy.org/omero/downloads/) and extract it.

Activate the virtual environment, and check the `metadata` command is
registered by the command-line interface shipped with OMERO.py:

    /tmp/venv/bin/activate
    cd /path/to/OMERO.py
    bin/omero metadata -h

## Annotating images

The image annotation is done in three steps:

-   first an CSV file needs to be created containing the annotations.
-   then the CSV file is converted into an OMERO.table attached to the
    top-level Project. 
-   finally the OMERO.table can be converted into a series of map annotations
    for each image.

### Annotation file

This file can have as many columns as desired. For images in a Project/Dataset/Hierarchy, each row correspond to an image and two columns `Dataset Name` and `Image Name` are required to find the image.

### Bulk annotations

First create an OMERO.table (`bulk_annotations`) attached the top-level project from the [annotation.csv](annotation.csv) file:

    bin/omero metadata populate --file annotation.csv Project:<id>

With OMERO 5.4.8, each row of this table should be viewable in the Web client
by expanding the Tables tab on the right-hand panel of each image.

### Map annotations

This conversion is done by a [configuration file](bulkmap.yml) which describes which columns should be turned into key/value pairs and whether some columns should be grouped together using a namespace e.g. Organism.

Run the `metadata populate` command with the `bulkmap` context:

    bin/omero metadata populate --context bulkmap --cfg bulkmap.yml Project:<id>

This should create map annotations for each image that can be viewed in the
right-hand panel of the web client. Some of these annotations will have
namespaces allowing them to be searchable using the
[OMERO.mapr](https://github.com/ome/omero-mapr) web app.
