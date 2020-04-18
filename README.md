imd
===

`imd` is a Prolog library/commandline utility that converts its own flavor of Markdown to and from structured data.
The goal of `imd` is to enable human-readable text-based communication and data storage. The key differences between `imd`
and other Markdown implementations are:

1.  Interactivity. `imd`'s purpose is to find section breaks, links, buttons, forms, and other navigational/interactive
    features. `imd` doesn't care about generating HTML or compatibility with other Markdown implementations.
2.  Determinism. The structured format of `imd` allows a character-perfect recreation of the original Markdown file.
3.  Flexibility. `imd` aims to reliably work irrespective of the user's layout and spacing choices as much as possible.


Planned features
----------------

### Navigational components (links, buttons)

Links work the same as in regular Markdown, except now they work directly without any conversion to HTML:

    [link name](http://link.com/pagedir)


Buttons are special links that point to pages on the same site. For example, the below "About Us" button automatically
points to an /about-us/ child page of the current page. If there's no /about-us/ child page, the parent directories are
searched.

    [ About Us ]


### Communication components (form fields, checkboxes, radio buttons)

There are no special submit buttons with any forms. Instead, forms are filled by directly modifying the local copy of
the .imd file. Submission happens via something like a "git push" of the modified file to the server.


Single-line form fields:

    First name: __________________


Multi-line form fields:


      - Enter your message ------
     |~                          |
     |~                          |
     |~                          |
      ---------------------------


Checkboxes and radio buttons:

    [ ] multi         (x) exclusive
    [x] select        ( ) select



pe
==

pe stands for Pluggable Editor. It is an editor meant to be as simple (stupid) as possible, and rely heavily on
plugins like `imd` to actually do anything useful. Its original purpose is to serve as a way to interact with `imd`
pages - for instance, to jump between interactive components, follow links and buttons, edit form contents,
check checkboxes, and submit forms.

### Notes on existing editors

Vim is an editor centered around _movements_. At the core of Vim's philosophy is the recognition that one character
is not a useful unit of editing a file. Vim allows easy movements and operations on entire words, lines, and sections.
Vim is also modal, a feature loves by its expert users and hated by novices. 

Emacs is an editor centered around _commands_. At the core of Emacs' philosophy is the idea that a user pressing a
character key or key combination is executing a command, and that that command should be easily customizable.
Special commands are performed with combinations of Ctrl+Key combinations.


### Summing it up for pe

pe should allow easy movements, and use the structured information from a program like `imd` to determine what
'words', 'lines', and 'sections' are in the given context.

pe should not be modal by default, but allow a user to turn on modality if they so desire. The insert and normal modes
should be strikingly visually different (different or even inverted color schemes) to help prevent mode errors.

The modal key combinations should be exactly the same as the Ctrl+Key combinations in insert mode. For example, if
'dw' deletes a word in normal mode, the same should be accomplished as Ctrl+d Ctrl+w in insert mode. Ctrl+Key
combinations should be completely ignored in normal mode. In essence, the only reason to use normal mode should be 
not wanting to hold down Ctrl for long sequences of commands.

Like Emacs, pe should allow easy customizations of its commands (in Prolog?).



General Philosophy
==================

1.  Data in storage, in transit, and on display should always be in one and the same format. Rendered web pages, HTML,
    JavaScript, HTTP headers, JSON, and database storage formats must all be replaced with human-readable plaintext.
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
      Therefore, `imd` must operate on entire directories rather than files.
3.  User viewing of text should allow for quick semantic movement within the text's structure. User editing of text
    should allow for both quick movements and customizable keypress commands. Some additional UX constraints from
    "Design Rules Based on Analyses of Human Error" (Norman 1983):
    - *Feedback*: The state (mode) of the system should be clearly available to the user, ideally in a form that's
      unambiguous and that makes the set of options readily available.
    - *Similarity of response sequences*: Different classes of actions should have quite dissimilar command sequences.
    - *Actions should be reversible* (as much as possible) and where both irreversible and of relatively high
      consequence, they should be difficult to do, thereby preventing unintentional performance.
    - *Consistency of the system*: The system should be consistent in its structure and design of command so as to
      minimize memory problems in retrieving the operations.
