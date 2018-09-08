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

The image annotation is done in two steps:

-   first the [annotation.csv](annotation.csv) is turned into an OMERO.table
    attached to the top-level Project
-   then the OMERO.table is converted into MapAnnotations for each image using
    the [configuration file](bulkmap.yml)

### Bulk annotations

First create an OMERO.table (`bulk_annotations`) attached the top-level project from the [annotation.csv](annotation.csv) file:

    bin/omero metadata populate --file annotation.csv Project:<id>

With OMERO 5.4.8, opening the Tables tab on the right-hand panel of each image
should display the row associated with the Image ID.

### Map annotations

Run the `metadata populate` command with the `bulkmap` context:

    bin/omero metadata populate --context bulkmap --cfg bulkmap.yml Project:<id>

This should create map annotations for each image that can be viewed in the
right-hand panel of the web client. Some of these annotations will have
namespaces allowing them to be searchable using the
[OMERO.mapr](https://github.com/ome/omero-mapr) web app.
