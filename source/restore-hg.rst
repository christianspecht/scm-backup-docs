Restoring Mercurial repositories
================================

How a bare repository looks like
--------------------------------

It contains a single folder named ``.hg``:

.. image:: restore-hg-folder.png


How to restore
--------------

Clone
+++++

Clone the bare repository into a “regular” one::

    hg clone bare-repo working-repo

``working-repo`` will have a working directory.


Update
++++++

Updating a bare repo to any revision will create a working directory in that repository.

To do this, ``hg update`` to any revision. For the newest revision, update to ``tip`` *(a tag which points to the latest commit)*::

    cd bare-repo
    hg update tip

In TortoiseHG, right-click on any revision ⇒ **Update**.

