bepasty
=======

bepasty is like a pastebin for all kinds of files (text, image, audio, video,
documents, ..., binary).

The documentation is there:
https://bepasty-server.readthedocs.org/en/latest/

Development quickstart
----------------------

From a source checkout, use ``uv`` to create/update the development
environment and start the built-in server:

::

  uv sync
  BEPASTY_CONFIG="$PWD/var/bepasty-dev.conf" uv run bepasty-server --debug

The referenced ``var/bepasty-dev.conf`` is a local development configuration
with full permissions for anonymous users. Do not use it for production.

Example of ``var/bepasty-dev.conf``

::

    # Local development configuration only.
    # Grants full permissions to anonymous users. Do not use in production.

    SITENAME = 'localhost'

    SECRET_KEY = 'YqUFP_-MTycFOH0KtpZKo8G3_mCG_uhT62UA8kWy_5Phkf4GJfdYGFn2pjVXAmn-'

    STORAGE = 'filesystem'
    STORAGE_FILESYSTEM_DIRECTORY = './var/bepasty-storage'

    # Allow the development server to work over plain HTTP.
    SESSION_COOKIE_SECURE = False

    # Full anonymous permissions: admin, list, upload/create, modify, read, delete.
    DEFAULT_PERMISSIONS = 'admin,list,create,modify,read,delete'

    # No login secrets are needed for this all-anonymous development setup.
    PERMISSIONS = {}

Features
--------

* Generic:

  - you can upload multiple files at once, simply by drag and drop
  - after upload, you get a unique link to a view of each file
  - on that view, we show actions you can do with the file, metadata of the
    file and, if possible, we also render the file contents
  - if you uploaded multiple files, you can create a pastebin with the list
    of all these files - with a single click!
  - Set an expiration date for your files

* Text files:

  - we highlight all text file types supported by pygments (a lot!)
  - we display line numbers
  - we link from line numbers to their anchors, so you can easily get a link
    to a specific line

* Image files:

  - we show the image (format support depends on browser)
  - for image list items, we can show a slide show ("carousel" view)
  - in the items list, a thumbnail of images is shown

* Audio and video files:

  - we show the html5 player for it (format support depends on browser)

* asciinema recordings:

  - we show the asciinema player for .cast files

* URLs:

  - we support linking to / redirecting to external URLs, you can use
    this as a link shortener (avoiding privacy / data protection issues
    that may exist with other link shorteners)

* PDFs:

  - we support rendering PDFs in your browser (if your browser is able to)

* Storage: we use a storage backend api, currently we have backends for:

  - filesystem storage (just use a filesystem directory to store
    <uuid>.meta and <uuid>.data files)
  - currently there are no other storage implementations in master branch
    and releases. The "ceph cluster" storage implementation has issues and
    currently lives in branch "ceph-storage" until these issues are fixed.

* Keeping some control:

  - flexible permissions: read, create, modify, delete, list, admin
  - assign permissions to users of login secrets
  - assign default permissions to not-logged-in users
  - you can purge files from storage by age, inactivity, size, type, ...
  - you can do consistency checks on the storage
