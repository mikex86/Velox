![Velox Server](https://github.com/mikex86/Velox/blob/master/images/velox_logo.svg)

# Velox Minecraft Server
![Java CI with Gradle](https://github.com/mikex86/Velox/workflows/Java%20CI%20with%20Gradle/badge.svg)
## About

Velox primary goal is to improve performance of the Vanilla server.
Minor behaviour changes are not specifically aimed to be avoided.
Compatibility with the Bukkit API is intentionally dropped to avoid running
into limitations introduced by an API that was designed around the single threaded architecture
of Minecraft.


## Velox Plugin API

Velox implements a (at the moment very incomplete) Plugin API.
To see an example plugin, visit: https://github.com/mikex86/VeloxExamplePlugin

The project is in early development, but feel free to test the potentially unstable 
builds.


## Multithreading
Velox implements region based entity and block ticking. This means entities and blocks in chunks that do not have
a loaded trail of chunks between them, will be ticked concurrently.
Dimensions are also ticked concurrently.
This is not a full async tick, heavily performance demanding regions, even in different dimensions, may still impact
server performance for other players.

### Is this safe?
Yesn't...<br>
The region model makes the assumption, that entities/blocks in different regions do not interact with one
another. In vanilla Minecraft this should never happen. If you are aware of any edge case
where this might occur, please do not hesitate to create an issue.

Maybe an even bigger problem are random elements of surprise in how Minecraft is implemented.
No all code in Minecraft follows the principle of least astonishment and can therefore cause problems 
nobody could expect till they arise. So, in a way, making Velox safer is a constant bug squashing battle.

## Why drop support for the Bukkit API?

As the Bukkit API was not created with this kind of ticking model in mind,
it is likely that plugins might perform unsafe interactions, leaving no other option but to "hope for the best".
Perhaps an even greater problem would be the thread safety measures for the plugins themselves.
Events like PlayerMoveEvent, which are invoked many times per second, might cause the server
to synchronize for long enough to eliminate the gains of concurrency.


## How to build
Clone the repository and execute the command

```./gradlew buildFinalJar```

The resulting jar file will be placed under build/libs
