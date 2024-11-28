## How to avoid dropped connections

We are using saga. Once we run a program on saga (or any remote computer), 
that program will only keep running a long as the terminal is connected to 
it, that is, as long as you have the computer and the ssh connection up. 
This is a bit inconvenient since we're oftne working on laptops and might 
want to pack that one down without interrupting the connection. There is 
a program on unix computers that is called `screen` which will let us do that.

### How to start

1. Log onto saga
2. Type in `screen`. You will get a new prompt, but that's it
3. Start whatever program that yo want to have running.
4. Depress `Control-a`, followed by `d`. Your screen will now say something
about a screen being `detached`.

You can now end your ssh login session, and your program will keep running.

### How to see and resume your sessions

You can see a list of screens (you can have multiple), type in `screen -ls`.
You will see something along the lines of `23921.pts-123.login-0-1`. The number
before `pts` is what I refer to below.
To connect to a specific screen, type in `screen -DR <number>`.

Note, there are multiple login nodes on saga, that is, when you log in you
might not end up on the same computer that you last were on. Thus, if you 
use screens, you might want to check which one you're on when you start your 
screen (use the command `hostname`). Then, when you log in again, you can 
check again which one you're on. If you're on the wrong one, use ssh to log 
into the other one.

For more, [check out this link](https://www.tecmint.com/screen-command-examples-to-manage-linux-terminals/)
