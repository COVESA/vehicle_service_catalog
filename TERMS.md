- **Service**
	- (General) A service is a software functionality or set of functionalities that can be activated by a client. A key characteristic of a service in support of SOA is that it has low coupling to other services and strives to provide a useful function also if used independently from other services.
	- (VSC) A VSC **Service** contains one or more **Interface**s, which are defined or referenced within the service definition.  The service definition may within its namespace also define **Type**s, constants and other definitions required to make use of the service.
	- A Service name is not in itself a Namespace, but its contents must be defined within *at least one* **Namespace**.  The Namespace in question is referenced or defined from within the Service definition.
	- A Service must have its fundamental definition in **one** single .yaml formatted file, although this file might use references to other parts.  In other words, all Interfaces that make up a service must be mentioned within this file*.
	- A file can contain a maximum of one Service.

_>	(To be further determined - how complete is our include/reference concept?)_

- **Namespace**
	- A namespace is a way to separate definitions in named groups.
	- Namespaces are used to ensure that local names will not clash with identically named items in other namespace.
	- Namespaces usually also separate what objects can be *seen* or *reached* from within a part of a program.
	- Namespaces can contain other Namespaces, creating a hierarchy.
	- How visibility/reachability is handled *might* be specific to the target language or environment, but VSC generally assumes that items within different (non-parent) namespaces are isolated from each other unless otherwise specified, and that common hierarchy rules apply (i.e. everything within a child namespace can see and reach items in any of its parent namespaces without a specific required statement of reference or inclusion).
- **Interface**
	- (General)
		- very broad term...
	- (VSC)
	- A VSC **Interface** is *primarily* defined by a collection of **Methods**.
	- An Interface name is not itself a Namespace but must be defined within a Namespace.
	- All Methods defined or referred to by an Interface are required to be within the same Namespace, i.e. it is not possible to build an Interface by referring to Methods that have been defined in different Namespaces.
	- The VSC interface definition language strives to usable in all areas of a computing system.   The VSC Interface object is therefore a pure software function focused definition, which does not define details around communication hardware, target language, communication protocols etc.  Such information is added through supplementary information later.
- **Method**
	- A method is a function with a (mandatory) name, and a set of **input parameters**, **output parameters**, and **error conditions**, all of which are optional and may be empty/undefined.
	- Like most other objects, a Method is always defined in the context of a Namespace, but this is most often implicitly determined because the Method is defined within the context of an Interface/Service
- **Error**
	- A VSC Error definition is an error condition that can appear while (attempting to) execute a given Method.
	- An error is defined by referencing one or more **Type**s that define the data that shall be communicated when an error occurs.  Enumeration types are common, but it could be any defined type.
	- There can be more than one error type defined for a method
	- It is not specified by VSC how errors are propagated - this translation is target language/environment specific.
		- It may be added as return/output parameters in one programming language and translated to **Exception**-handling in another.  Communication protocols may implement their own error-condition channels and approaches.


----

NEW 2022-08-08:

----

## Properties

(N.B. In Franca IDL these are called Attributes)

- A **Property** is an observable data item.  It belongs in a **Namespace**, has a canonical name in addition to possible **aliases** (i.e. alternative names), and a data type.
- A Property is defined as a single item but the data type could be an arbitrary data type, including composite data types.
- The singular nature of a property suggests that its value shall updated and communicated as an atomic operation, i.e. it is either fully updated or not at all.

- The method of update and communication is typically target-environment specific, but the most common expectation is that a Property can be updated and _observed_, meaning that a client can call a method to get or set the current value of the property (with appropriate access control rules) and often also _subscribe_ to be notified of when the value of a property was changed for any reason.
- In some other language definitions this type of item is called an **Attribute** or **Field**.
- A property can also be seen as analogous (and bi-directionally translatable, if datatypes allow it) to a Sensor Signal in the Vehicle Signal Specification (VSS).

**Side proposal**: Consider renaming Property to Attribute to match Franca IDL, and because properties have another use in OpenAPI and AsyncAPI, and it seems other HTTP-related communication as well.


## Return values

- A Return Value specifies what is returned from a Method execution.  Although "out parameters" could serve a similar purpose, the Return Value has been given special status.

- There is a single return value type, but it can use any data type, including composite types.

- In computing systems there are two main paradigms regarding invocation of a procedure/function: Blocking/synchronous vs. Non-blocking/asynchronous.

  - In a blocking/synchronous environment, the client that invokes an operations (calls a function) will stop until the operation is completed and "returns".  Upon return, a return-value (result) is communicated.
  - In an asynchronous setup, a client can trigger the start of an operation and then continue doing other work while it executes in parallel.  In this case, the executing function will report back when the operation is finished, but it could also give continuous reports as to the progress of the function ("streaming updates").

- In the VSC-language, methods can be defined without defining their invocation strategy (synchronous/asynchronous), which is instead often defined at a later time, and may be constrained by the target environment capability.

- In all cases, a Method in VSC-lang (optionally) defines a Return Value type.  The type could frequently be an enumeration type to communicate one of a predefined set of results, but it is free to be any datatype.

- Although details are target-dependent, this is the expected behavior of the return type:
  - In the case of a synchronous/blocking call, this value type is the return value of the function that is communicated (once) after the operation completes.
  - In the case of an asynchronous environment, the return value type is also used, with the difference that it can be returned _multiple times_.
  - (After an operation has been completed and its equivalent of "DONE" has been communicated, the function is naturally expected to seize any further communication of the return type)

- In this manner, the same concept (return value) can be defined, without knowing the manner that the function will be invoked(\*).

- Only valid values of the return value type can be returned.  (Additional mechanisms could of course be designed as a side-channel defined in another part of the system definition, but its definition would not be tied to the Method definition).

- (\*) Thus, if async operation is expected ("streaming updates") then it is still a good idea to design the return value for this.  It is likely to include something to indicate that processing is under way.  For example the streaming returns during an operation might be:
`Started, Processing..., Processing..., Processing..., Done!"`

- A return value can also indicate that the operation did NOT complete successfully, but the more capable Errors concept should not be overlooked.


## Event

- An Event is a time-sensitive communication with _fire-and-forget_ behavior, sent between parts in the distributed system.
- An Event can carry multiple pieces of data with it, each of which can use an arbitrary data type.
- An Event is defined similar to a Method but with some differences in definition and behavior:
  - An Event has a name, and _optionally_ a set of parameters that are distributed with the event, each having a name and a datatype.  This somewhat mimics a Method signature.
  - An Event can however **not** have a Return type and not any out-parameters.
- An Event can be sent from client to server or from server to client(\*).
- _Fire-and-forget_ expected behavior means that it depends on the target environment whether an Event can be guaranteed to be received or if it is non-reliable.  In either case, there is **no reply** defined for an Event.  Compare an an operation (Method) where reliability is assumed, and a return value would indicate the request was received and completed.

(\*) Shall the direction of an event be defined somehow?

