# Machine Language

Computers can be described concretely--in terms of their hardware architecture--and abstractly--in terms of the hardware's capabilities. Machine language describes the computer in terms of its capabilities. It is an agreement between hardware developers and software developers regarding how the computer's memory can be manipulated using a processor and a set of registers.

##### Memory

A hardware system's memory is the set of devices that can store data and instructions. From a programmer's perspective, the memory is an array of fixed-width registers called _words_ or _locations_. Each register can store some piece of data, and can be accessed via its address--a unique number specifying which cell holds its data. The memory devices include lookup logic that can find the data stored at a given location.

##### Processor

A processor, or central processing unit, is a device that is capable of performing a fixed set of elementary operations, like arithmetic, boolean logic, memory access, and branching. The processor can store inputs, outputs, and instructions either in memory, or in registers.

##### Registers

While hardware developers know that memory itself is composed of registers (atomic locations capable of holding data of a fixed width), the registers that a processor uses are located in close proximity to the processor itself. Since memory access is a relatively slow operation, the processor uses these local registers for high-speed local memory, in order to execute instructions more quickly.

##### Languages

Each hardware platform has its own formalism--its own machine language agreement describing how to perform elementary operations. In order to write in a given machine language, we have to understand the rules of the game: the hardware's instruction set.