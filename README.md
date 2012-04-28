# node-supervisor

A little supervisor script for nodejs. It runs your program, and
watches for code changes, so you can have hot-code reloading-ish
behavior, without worrying about memory leaks and making sure you
clean up all the inter-module references, and without a whole new
`require` system.

# This Fork
This is node-supervisor forked at 0.3.0 and merged with two great
pull requests which weren't accepted at the time of forking:
[debug flag for node](https://github.com/isaacs/node-supervisor/pull/50) and [ignore directory feature](https://github.com/isaacs/node-supervisor/pull/41).


## node-supervisor -?


    Node Supervisor is used to restart programs when they crash.
    It can also be used to restart programs when a *.js file changes.

    Usage:
      supervisor [options] <program>
      supervisor [options] -- <program> [args ...]

    Required:
      <program>
        The program to run.

    Options:
      -w|--watch <watchItems>
        A comma-delimited list of folders or js files to watch for changes.
        When a change to a js file occurs, reload the program
        Default is '.'

      -i|--ignore <ignoreDirectory>
        Ignore changes within a particular directory

      -e|--extensions <extensions>
        Specific file extensions to watch in addition to defaults.
        Used when --watch option includes folders
        Default is 'node|js'

      -x|--exec <executable>
        The executable that runs the specified program.
        Default is 'node'

      -n|--no-restart-on error|exit
        Don't automatically restart the supervised program if it ends.
        Supervisor will wait for a change in the source files.
        If "error", an exit code of 0 will still restart.
        If "exit", no restart regardless of exit code.
        
      -d|--debug
        Start node with --debug flag.
        
      -k|--debug-brk
        Start node with --debug-brk flag.

      -h|--help|-?
        Display these usage instructions.

      -q|--quiet
        Suppress DEBUG messages

    Examples:
      supervisor myapp.js
      supervisor myapp.coffee
      supervisor -w scripts -e myext -x myrunner myapp
      supervisor -w scripts -i views -e myext -x myrunner myapp
      supervisor -w lib,server.js,config.js server.js
      supervisor -- server.js -h host -p port
      supervisor --ignore views --no-restart-on error --debug --quiet app.js


## Simple Install

Install npm, and then do this:

    npm install supervisor -g

You don't even need to download or fork this repo at all.

## Fancy Install

Get this code, install npm, and then do this:

    npm link

## todo

1. Re-attach to a process by pid. If the supervisor is
backgrounded, and then disowned, the child will keep running. At
that point, the supervisor may be killed, but the child will keep
on running. It'd be nice to have two supervisors that kept each
other up, and could also perhaps run a child program.
2. Run more types of programs than just "node blargh.js".
3. Be able to run more than one program, so that you can have two
supervisors supervise each other, and then also keep some child
server up.
4. When watching, it'd be good to perhaps bring up a new child
and then kill the old one gently, rather than just crashing the
child abruptly.
5. Keep the pid in a safe place, so another supervisor can pull
it out if told to supervise the same program.
6. It'd be pretty cool if this program could be run just like
doing `node blah.js`, but could somehow "know" which files had
been loaded, and restart whenever a touched file changes.
