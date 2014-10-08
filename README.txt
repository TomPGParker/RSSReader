running ./test.sh will compile and run a suite of tests on all the files.

They can however, be compiled and run independantly if that's what you wish and can be done so from seperate folders.

Format for running is:
java AtomServer <portnumber>
java GETClient <URL>
java PUTClient <URL> <INPUT_FILE_PATH>

The test document is test.sh, which this document includes more information about the system.

LAMPORT CLOCK NOTE: If you wish to have the programs output their lamport clock value change the boolean class variable LAMPORT to true. This will however, cause the majority of my tests to give fail results as the output gets mixed with the other information being outputted so run the test script before you do this.


What's new in Assignment 2:
-I've removed the updating loop from AtomServer. Previously when it was server a put it would make everything else wait until the put was complete before even looking at the get. Now it uses lamport clocks to store the items into a queue in the order they come in. The downside is that now it only ever executes one call at a time however this is a cleaner way and it also using lamport clocks to maintain ordering.
-When a put request is recieved by the server, the server takes the xml file and puts it in the storedData folder and access it later to merge the files
-The feeds now merge into one feed rather than each replacing the last. The feed meta data comes from the latest feed and the entries are the 25 most recent from across all the feeds.
-The test suite has been greatly improved. It now outputs the number of tests it fails and is much easier to understand.
-The PUT client can now be set to send a heartbeat to the server by using the -p option when calling it e.g., java PUTClient <address> <filepath> -p
-The PUT client is now that one which parses the xml instead of the server as it mistakenly did in assignment 1. Changing it over was surprisingly simple.
-The test script no longer supresses the AtomServer's output. I thought this would make things more clear than they were in assignment 1.

Decisions:
-Although we've been told to refer to the out PUTClient as a Content Server nowhere did it say to actually change the name, so I decided to keep it how it was.
-The makeFeed function is called more often than it needs to be, but that is so I can check what the AtomServer is doing for testing. To be more efficient you can turn the boolean class variable TESTING to false.
-There is a sleep after each put request so that the server has time to process if before the next command that. Although the lamport clock ensures order when sending multiple requests, it does not ensure that a System call won't interrupt processing.
-When the server restarts it sets all the feeds times back to zero. Although this may cause a feed to stay longer than it is usually meant to there's also the posability that it had been getting requests while it was down so consider this an apologetic compramise on the Atom Server's behalf.

Observations:
-It's slightly more difficult to test this one as there is a time factor. At one point I was failing a test because there's was too long between the put and the get (this was on the puts without heartbeats)