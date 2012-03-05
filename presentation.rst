Introduction to PDB
===================

:Authors: Chris McDonough, Agendaless Consulting
:Date: 3/9/2012 (PyCon 2012)

..  footer:: Chris McDonough, Agendaless Consulting

What is PDB
-----------

- Interactive shell.

- Gives you a glimpse into the execution paths of your code.

- Allows you to see the state of variables during execution.

- Allows you to execute expressions in the context of a particular stack
  frame.

When Should I Use It
--------------------

- "I don't use a debugger"

- When is it really not reasonable?

Script Mode
--------------------------------

``python -m pdb buggy.py``

  steps through the entire script in pdb.

Enhanced Script Mode (Py 3.2+)
-----------------------------------

Has ``-c``.

``python -m pdb -c continue buggy.py``

  lets the script run until an exception occurs, then presents prompt.

``python -m pdb -c 'until 2' buggy.py``

  lets the script run until it reaches line 2, then presents prompt.

These are actually ``.pdbrc`` commands on the command line (they run
after the commands in any ``.pdbrc`` file).

PDB Aliases
-----------

``alias ph !help(%1)``

  ``ph <someobject>`` prints help about the object

``alias pl pp list(locals().keys())``

   ``pl`` pretty-prints a list of the names local to the frame.

``%`` args are positional starting at 1; to consume all use ``%*``.

.pdbrc File
-----------

- Can live in the current directory or in your home directory.  If both
  exist, home directory directory values are executed, then current directory
  values.

- Almost any pdb command can be executed, but .pdbrc is most useful to define
  aliases.

Postmortem Mode
---------------

``pdb.pm()``

  Can be used after an uncaught exception has been raised.

Script mode is postmortem mode too.

Run Mode
--------

``pdb.run('some.expression()')``.

  Executes ``some.expression()`` under pdb control.  Uses current locals
  and globals to interpret the expression.

Trace Mode
----------

``pdb.set_trace()``

  Convenient to stick in development and test code.

Just another Python expression so you can conditionalize its execution.

Workhorse Commands
------------------

- list, print, pretty-print, next, continue, step, return, until, where, up,
  down.

- Aliases can be used.

- Blank line repeats the last command issued.

- ``readline`` is used in the debugger so arrow keys and other readline
  keybindings work (ctrl-a, ctrl-e, etc in emacs mode).

Where Am I?
-----------

- ``list``: displaying code in your current execution context

- ``p`` and ``pp``: displaying objects

- ``where``: showing the current location in the frame stack.

Execution Control
-----------------

- ``continue``, ``step``, ``return``, ``next``, ``return``, ``until``:
  execution control

Frame Stack Control
-------------------

- ``up``, ``down``: navigating the frame stack

Aspects
-------

- Exceptions generated when a pdb command generates an exception don't impact
  current program state.

- Shortcut aliases (``c`` vs. ``continue``)

- Getting help

Breakpoints
-----------

- ``break``, ``tbreak``, ``ignore``, ``enable``, ``disable``, and
  ``clear``: Managing breakpoints

Debug
-----

- ``debug``: recursive debugging

Esoteric Commands
-----------------

- ``!``-prefixing: modifying variables

- ``args``: printing args to the current function

- ``commands``: scripting pdb

PDB and Threads
---------------

- Set thread count to 1

Purple Bags
-----------

- Enhanced shells: ipdb, pudb, winpdb

  - IPython:  ipdb, ipdb.set_trace()

- In-editor debugger integration (Wing, Eclipse PyDev, PyCharm, etc)

- pdb-pm-like environments (Django/Flask/Pyramid debug toolbar)

