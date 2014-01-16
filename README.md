# elephantsearch_search

## What's it about?
Elephantsearch is principally a Ubuntu (Precise 64) based linux box running within Vagrant (http://www.vagrantup.com)
with Apache Stanbol on top.

It contains the following, additional software:
* ohai
* subversion
* ark
* java (openjdk)
* maven

## How to build it
First, get the repository:

    $ git clone https://github.com/sosterwalder/elephantsearch_search.git

After getting the repository, initialize and update the submodules:

    $ git submodule update --init

Then, start vagrant:

    $ vagrant up

The first time this might take a while, as it updates the system and installs the software mentioned above.

Log into the system, change to root:

    $ vagrant ssh
    vagrant@vagrant$ sudo su

Next, build and run Apache Stanbol:

    # Set memory related values for the build process.
    # Maybe you need to increase them.. I had not always luck 
    # building it.
    root@vagrant$ export MAVEN_OPTS="-Xmx1024M -XX:MaxPermSize=1024M"

    # Fetch the sources
    root@vagrant$ svn co http://svn.apache.org/repos/asf/stanbol/trunk stanbol

    # Build stanbol. This could take quite some time (30min. plus..)
    root@vagrant$ mvn clean install

## Running it
First, make sure you don't have anything running on port 8000 (on your host).

After building stanbol, you may run the stable launcher:

    root@vagrant$ java -Xmx1g -jar stable/target/org.apache.stanbol.launchers.stable-{snapshot-version}-SNAPSHOT.jar

Or, if you prefer, run the full version:

    root@vagrant$ java -Xmx1g -XX:MaxPermSize=1024m -jar full/target/org.apache.stanbol.launchers.full-{snapshot-version}-SNAPSHOT.jar

Now you should be able to access stanbol (within your host, not the vm running inside vagrant): http://localhost:8000


## What to do now?
Please check out the well documented http://stanbol.apache.org/docs/trunk/.
