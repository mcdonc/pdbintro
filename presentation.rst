.. include:: <s5defs.txt>

Introduction to PDB
===================

:Authors: Chris McDonough, Agendaless Consulting
:Date: 3/9/2012 (PyCon 2012)

..  footer:: Chris McDonough, Agendaless Consulting

What is PDB
-----------

- Interactive shell.

- Gives you a glimpse into the execution path of your code.

- Allows you to display and modify the state of variables during execution.

- Allows you to execute expressions in the context of your code.

When Should I Use It
--------------------

- "I don't use a debugger"

- When is it really not reasonable?

buggy.py
--------

::

  def divide_one_by(divisor):
      return 1/divisor

  if __name__ == '__main__':
      divide_one_by(0)

Invoking PDB
------------

- Script mode

- Postmortem mode

- Run mode

- Trace mode

Script Mode
-----------

``python -m pdb buggy.py``

  steps through the entire script in pdb.

::

  [chrism@thinko pdb]$ ../env27/bin/python -m pdb buggy.py 
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(1)<module>()
  -> def divide_one_by(divisor):

Enhanced Script Mode (Py 3.2+)
-----------------------------------

``python -m pdb -c continue buggy.py``

  lets the script run until an exception occurs, then presents prompt.

::

 [chrism@thinko pdb]$ ../env32/bin/python -m pdb -c continue buggy.py 
 Traceback (most recent call last):
 ...
 ZeroDivisionError: division by zero
 ...
 > /home/chrism/projects/pycon/2012/pdb/buggy.py(2)divide_one_by()
 -> return 1/divisor
 (Pdb) 

Enhanced Script Mode (Py 3.2+)
------------------------------

``python -m pdb -c 'until 2' buggy.py``

  lets the script run until it reaches line 2, then presents prompt.

::

 [chrism@thinko pdb]$ ../env32/bin/python -m pdb -c 'until 2' buggy.py 
 > /home/chrism/projects/pycon/2012/pdb/buggy.py(4)<module>()
 -> if __name__ == '__main__':
 (Pdb)

``-c`` commands are actually ``.pdbrc`` commands on the command line (they
run after the commands in any ``.pdbrc`` file).

Postmortem Mode
---------------

``pdb.pm()``

  Used after an uncaught exception has been raised.

::

  [chrism@thinko pdb]$ python -i buggy.py
  Traceback (most recent call last):
    ...
  ZeroDivisionError: integer division or modulo by zero
  >>> import pdb; pdb.pm()
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(2)divide_one_by()
  -> return 1/divisor
  (Pdb)

Run Mode
--------

``pdb.run('some.expression()')``.

  Executes ``some.expression()`` under pdb control.  Uses current locals
  and globals to interpret the expression.

::

  >>> import buggy, pdb; pdb.run('buggy.divide_one_by(0)')
  > <string>(1)<module>()
  (Pdb) s

Trace Mode
----------

``pdb.set_trace()``

- Convenient to stick in development and test code.

- Just another Python expression so you can conditionalize its execution.

- I use this mode all the time.  We'll be using it from now on.

::

  def divide_one_by(divisor):
      import pdb; pdb.set_trace()
      return 1/divisor

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

::

  [chrism@thinko pdb]$ cat ~/.pdbrc 
  alias ph !help(%1)
  alias pl pp list(locals().keys())

Command Language
----------------

- ``readline`` is used in the debugger so arrow keys and other readline
  keybindings work (ctrl-a, ctrl-e, etc in emacs mode).

- Aliases can be used.

- Blank line repeats the last command issued (unless it was a ``list``
  operation).

- Full command name or its shortened version can be used at ``(Pdb)`` prompt.

Where Am I?
-----------

- ``list (l)``: displaying code in your current execution context

- ``where (w)``: showing the current location in the frame stack.

Command: list (l)
------------------

::

  [chrism@thinko pdb]$ ../env27/bin/python buggy.py
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(3)divide_one_by()
  -> return 1/divisor
  (Pdb) l
    1  	def divide_one_by(divisor):
    2  	    import pdb; pdb.set_trace()
    3  ->	    return 1/divisor
    4  	

Command: where (w)
------------------

::

  [chrism@thinko pdb]$ ../env27/bin/python buggy.py
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(3)divide_one_by()
  -> return 1/divisor
  (Pdb) w
    /home/chrism/projects/pycon/2012/pdb/buggy.py(6)<module>()
  -> divide_one_by(0)
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(3)divide_one_by()
  -> return 1/divisor
  (Pdb) 

Displaying Things
-----------------

- ``p`` and ``pp``: displaying objects.  Accepts expression.

- ``args``: print values of current function's arguments.

Command: print (p)
------------------

::

  [chrism@thinko pdb]$ ../env27/bin/python buggy.py
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(3)divide_one_by()
  -> return 1/divisor
  (Pdb) p divisor
  0

Note that ``print`` != ``p``.  ``print`` is passed through to the
interpreter; ``p`` is executed by pdb.

Command: pretty-print (pp)
---------------------------

Use when results of ``p`` are too ugly.

::

  (Pdb) pp ['a'] * 16
  ['a',
   'a',
   'a',
   ...
   'a',
   'a',
   'a']

Command:  args (a)
------------------

Prints argument values passed to the current function.

::

  def divide_one_by(divisor):
      import pdb; pdb.set_trace()
      return 1/divisor

::

  [chrism@thinko pdb]$ ../env27/bin/python buggy.py
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(3)divide_one_by()
  -> return 1/divisor
  (Pdb) a
  divisor = 0


commands
--------

- scripting pdb

Execution Control
-----------------

- ``continue``, ``step``, ``return``, ``next``, ``return``, ``until``:
  execution control

Frame Stack Control
-------------------

- ``up``, ``down``: navigating the frame stack

Command: up (u)
---------------

::

  (Pdb) l
    1  	def divide_one_by(divisor):
    2  	    import pdb; pdb.set_trace()
    3  ->	    return 1/divisor
    4  	
    5  	if __name__ == '__main__':
    6  	    divide_one_by(0)
    7  	    
  [EOF]
  (Pdb) u
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(6)<module>()
  -> divide_one_by(0)
  (Pdb) l
    1  	def divide_one_by(divisor):
    2  	    import pdb; pdb.set_trace()
    3  	    return 1/divisor
    4  	
    5  	if __name__ == '__main__':
    6  ->	    divide_one_by(0)
    7  	    

Command: down (d)
-----------------

::

  (Pdb) l
    1  	def divide_one_by(divisor):
    2  	    import pdb; pdb.set_trace()
    3  	    return 1/divisor
    4  	
    5  	if __name__ == '__main__':
    6  ->	    divide_one_by(0)
    7  	    
  [EOF]
  (Pdb) d
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(3)divide_one_by()
  -> return 1/divisor
  (Pdb) l
    1  	def divide_one_by(divisor):
    2  	    import pdb; pdb.set_trace()
    3  ->	    return 1/divisor
    4  	
    5  	if __name__ == '__main__':
    6  	    divide_one_by(0)
    7  	    
  [EOF]


Modifying Variables
-------------------

- ``!``-prefixing: pass directly to interpreter, used often to modify
  variables.

::

  [chrism@thinko pdb]$ ../env27/bin/python buggy.py
  > /home/chrism/projects/pycon/2012/pdb/buggy.py(3)divide_one_by()
  -> return 1/divisor
  (Pdb) p divisor
  0
  (Pdb) !divisor=1
  (Pdb) p divisor
  1
  (Pdb) n


Breakpoints
-----------

- ``break``, ``tbreak``, ``ignore``, ``enable``, ``disable``, and
  ``clear``: Managing breakpoints

Debug
-----

- ``debug``: recursive debugging

Aspects
-------

- Exceptions generated when a pdb command generates an exception don't impact
  current program state.

- Getting help



PDB and Threads
---------------

- Set thread count to 1

Purple Bags
-----------

- Enhanced shells: ipdb, pudb, winpdb

  - IPython:  ipdb, ipdb.set_trace()

- In-editor debugger integration (Wing, Eclipse PyDev, PyCharm, etc)

- pdb-pm-like environments (Django/Flask/Pyramid debug toolbar)

