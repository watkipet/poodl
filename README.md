# POODL
Parallel Online Omniverse Description Language

POODL is a software description language much like VHDL is a hardware description language.

- Parallel - POODL uses blocks that execute in parallel.
- Online - POODL blocks can be replaced while they execute without shutting down the rest of the system.
- Omniverse - POODL can grow to any size and interconnect any system.
- Description - POODL describes blocks and the connections between them.
- Language - POODL describes the blocks in a system but not in a linear fashion as do most langauges.

## The Language
POODL consists of blocks, connections, and metadata. Blocks perform processing and / or retain state. Connections link blocks together and transfer blocks from one block to another. Metadata describes blocks and connections.

### Examples
Writing POODL is interactive. Instead of coding a file, you issue command to a shell.

#### Block

Following are some commands that define a Car block.

```
> newBlock Car
> enterBlock Car
> addInput Gasoline fuel
> addInput Oxygen air
> addOutput Exhaust output
> copyBlock Engine engine
> copyBlock FuelTank fuelTank
> copyBlock ExhaustPipe exhaustPipe
> connect fuel fuelTank.fillerNeck
> connect air engine.airCleaner
> connect fuelTank.pumpOutput engine.fuelInlet
> connect engine.exhaustManifold exhaustPipe.inlet
> connect exhaustPipe.output output
```

You can define aliases for commands:

```
> alias enterBlock cd
> alias connect ln
> alias copyBlock cp
```

You can describe blocks:

```
> describe Car
(Gasoline fuel, Oxygen air) -> Car -> (Exhaust output)
{
    Engine engine;
    FuelTank fuelTank;
    ExhaustPipe exhaustPipe;

    fuel -> fuelTank.fillerNeck;
    air -> engine.airCleaner;
    fuelTank.pumpOutput -> engine.fuelInlet;
    engine.exhaustManifold -> exhaustPipe.inlet;
    exhaustPipe.output -> output;
}
metadata
{
    Stateful: *true
    Clocked: *false
}
```

You can make a description block and replay it into a new block:

```
> describe Car > CarDescription
> descriptionToBlock CarDescription Car2
```

A description block is a block with no body and a metadata field that contains the description text.

## Motivation
This section explains why we need yet another language to learn.

### The Way Things Were
Digital computing has changed significantly since its inception in the 20th century. However it still retains much of its original legacy.

#### Serial Processing
Computers originally excelled at tabulating serial streams of data--be those punch cards are paper tape. Most computer langauges today still operate as a serial list of instructions with control flow. However, modern computer hardware has become increasingly parallel.

#### Two Memory Types
Computer memory is still mostly classified into main memory (RAM) and secondary memory (disk drives, SSDs, etc.). Even though computers now have caches of different speeds and large programs store their data on separate systems, most langauges still preserve the concept of loading a program from sedondary memory into main memory and then executing it.

#### Memory and CPU Separation
Computer languages still retain the concept of storing data in memory and retrieving it for evaluation by a CPU. However, at a macro level many web sites distribute themselves accross multiple nodes where each node has its own CPU and memory. This distribution of a program need not apply only to web sites and their nodes.

#### Offline Editing
Most langauges still go through an offline editing phase where the designer updates part of it, compiles it, and then runs it. For large programs, this cycle can take a very long time. An exception to this is micro-services in back-end development. Those can be switched out as the system runs.

### Discrete Computers
Existing languages for most part assume your program will be executed on a single computer. You can communicate betwen different programs running on separate computers or fork processes, but most languages are not meant to run accross many distributed compute units.

### The POODL Way
POODL does away with past assumptions and embraces the new landscape of interconnected massively parallel hardware that we know today.

#### Parallel
POODL blocks execute in parallel. As soon as you connect the input of your new block to the output of another, it begins processing in parallel with all the others.

#### Any Memory Type
POODL blocks contain their own memory. The physics of the actual memory behind the block determines how robust it is, it's throughput, and its latency. Blocks expose this information in their metadata so other blocks can determine if they want to make a connection.

#### Online Editing
There is no such thing as a discerete POODL program. You just connect POODL blocks together and let them run. That said, you can disconnect a POODL block to stop it from executing and you can make copies of a disconnected POODL block. Writing POODL blocks is not like writing code on paper. You interactively connect blocks together and form larger blocks out of many smaller blocks.

#### Distributed
POODL blocks need not run on the same processor. They need not run on the same computer. They can span different types of networks and interconnects so long as the connection metadata accurately represents the characteristics of the connection (latency, throughput, etc.).
