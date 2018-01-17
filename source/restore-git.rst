Restoring Git repositories
==========================

How a bare repository looks like
--------------------------------

It contains a few folders (``objects``, ``refs``...) and some files.


How to restore
--------------

Clone the bare repository into a "regular" one::

    git clone bare-repo working-repo

``working-repo`` will have a working directory.
