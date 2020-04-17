imd
===

imd lets you browse and edit interactive Markdown files. The links are followable and the forms fillable.


Planned features
----------------

### Links and buttons
Links work the same as in regular Markdown. Navigate the links with up and down arrows and follow with Enter key.

    [ link name ](http://link.com)


Buttons are special links that point to pages on the same site. For example, the below "About Us" button automatically
points to an /about-us/ child page of the current page. If there's no /about-us/ child page, the parent directories are
searched.

    [ About Us ]


### Forms
There are no special submit buttons with any forms. When imd browser lets you fill out a form, it actually modifies
your copy of the .imd file. Submission happens via a commit-push of the file via the DCAF. You could achieve the exact
same effect by editing the .imd file in Vim and then performing a "git commit" and a "git push" on the command line.


Single-line forms:

    First name: __________________


Multi-line forms:


      - Enter your message ------
     |~                          |
     |~                          |
     |~                          |
      ---------------------------


Checkboxes and radio buttons:

    [ ] multi         (x) exclusive
    [x] select        ( ) select


Philosophy
==========

1.  Data in storage, in transit, and on display should always be in one and the same format.
    SQL, HTML, JavaScript, HTTP headers, JSON, and rendered pages must all be replaced with human-readable plaintext.
2.  Data storage and transit should happen via a distributed content-addressable filesystem (DCAF)
    [like Git](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain).
    Communication should be thought of as collaboration on the same set of data rather than message passing.
    Using a DCAF will eliminate data loss and inconsistencies, and provide a perfect audit trail and backups.
        - A file is an atom of modification. Any time a change is made to a file, no matter how small, the whole file
          is modified. When two users modify the same file, there is no automatic way to resolve the conflict.
          If there was such a way, then the file is not truly an atom of modification, and should be broken down into
          multiple files to allow for easier collaboration. Two separate files, on the other hand, are always
          considered independent, and changes to separate files can be resolved automatically.
          ** If two people can't modify the same object without causing logical conflicts, the object is a file. **
        - A directory is an atom of revision. It is the smallest object which can have its own history.
          Imagine a Git repository that's only allowed to have one file in it - a file-level repository. Even if we
          could easily have a repository for every file, those individual histories wouldn't be very useful to us.
          We use version control to create snapshots of how multiple files interact with each other. A directory is
          a grouping of files for which we would be interested in capturing snapshots of their relationships and
          interactions.
          ** If two people can modify the same object without causing logical conflicts, the object is a directory. **
          ** If an object's history is interesting, the object is a directory or a lone file in a directory. **
        - Webpages and application states can have multiple people collaborating on one thing (e.g. chat application),
          and can also have interesting histories in and of themselves, so the underlying objects must be directories.
          Therefore, imd must operate on entire directories rather than files.
