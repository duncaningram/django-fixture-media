# django-fixture-media #

`django-fixture-media` is a Django app that lets you have media files in your model fixtures. It upgrades Django's `loaddata` command to copy media files in your fixture paths to your project's `MEDIA_ROOT` directory. Its `dumpdata` command also copies out references media files alongside the dumped fixture.

### Installing ###

Install `django-fixture-media` as any other Python package to your Django project's Python environment. Then add `fixturemedia` to your `INSTALLED_APPS` setting:

    INSTALLED_APPS = (
        #'django.contrib.admin',
        # ...
        'fixturemedia',
        #'yourapp',
    )

The management commands are named `loaddata` and `dumpdata`, overriding Django's built-in commands of the same names.


### Dumping fixtures ###

When you use `dumpdata` to dump a fixture with media files, you necessarily need to write the fixture to disk, so that media files can be written with it. The `dumpdata` command has a required `--outfile` argument for specifying where on disk to write the file to.

    $ ./manage.py dumpdata --outfile myapp/fixtures/archive.json

Any media files referenced by `FileField` fields (including subclasses like `ImageField`) in the dumped models will be written to a neighboring `media/` directory.

    $ ls -R myapp/fixtures/media | head -7
    artwork

    myapp/fixtures/media/artwork:
    1067342064.jpeg
    1266612525.jpg
    1284255080.jpg
    1420544354.jpg


### Loading fixtures ###

To load a fixture, use `loaddata` as you normally would.

    $ ./manage.py loaddata myapp/fixtures/archive.json

Media files referenced by file fields in the loaded models are sought in `media/` subdirectories inside the project's fixture paths. Found media files are copied from there to your project's `MEDIA_ROOT`.

    $ ls -R mysite/media | head -7
    artwork

    mysite/media/artwork:
    1067342064.jpeg
    1266612525.jpg
    1284255080.jpg
    1420544354.jpg
