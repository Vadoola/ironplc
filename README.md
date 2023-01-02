# IronPLC

IronPLC aims to be a SoftPLC written entirely in "safe" Rust for embedded
devices running programs written in the IEC 61131-3 language.

SoftPLCs enable embedded other computers to operate as programmable logic 
controllers (PLCs) to execute all sorts of processes from home automation
and factories to industrial process automation and the electrical power grid.
They do this by implementing control algorithms that connect to sensors,
transducers and actuators using analog/digital IO, industrial protocols such as
I²C and Modbus, or even common internet protocol such as HTTP.

It is now where near there yet - currently what exists is a prototype of
source-to-source compiler for IEC 61131 to Rust. The reason for
source-to-source rather than virtual machine is a belief that the virtual
machine approach would require "unsafe" Rust. I've not tested this believe
add don't know whether the belief is true. The source-to-source compiler does
not yet have code generation.

## Usage

The current state of the project is it checks an IEC 61131-3 library for
syntactic and semantic correctness. The result is almost guaranteed to be
incorrect except for the most basic of libraries.

To run the checker, you need to install git, Rust and Cargo. Once you have
those, follow the steps below to check a library for correctness.

Get the code:

```sh
git clone https://github.com/garretfick/ironplc.git
cd ironplc
```

Build the application:

```sh
cargo build
```

Run the IEC 61131-3 checker:

```sh
.\target\debug\ironplc-plc2x.exe plc2x\resources\test\first_steps.st
```

## Developing

The current state of the project is it parses a small program
generated from [Beremiz](https://beremiz.org/). The only thing that you can do
is run unit and integration tests that try to parse correct and incorrect
programs.

```sh
cargo test
```

## How It Works

The project is split into 3 members:

* `dsl` defines relevant domain objects from the IEC 61131-3 language; it is
   the intermediate set of objects from parsing and contains an abstract syntax
   tree as one component (among many)
* `parser` is tokenizes and parses an IEC 61131-3 text file into the `dsl`
   objects
* `plc2x` is the front-end for a source-to-source compiler; it assembles all
   the pieces

There is no strict definition of what goes where. Better rules would be nice.
