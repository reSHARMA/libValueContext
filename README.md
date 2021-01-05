<h1 align="center">
  <br>
  <a href="#"><img src="https://i.ibb.co/Jj87195/6ba12165-5f81-4eb9-bb0a-1d7f7a54a5b7-200x200.png" alt="libValueContext" width="200"></a>
  <br>
  libValueContext
  <br>
</h1>

<h4 align="center">Add context-sensitivity to your analysis using value-context</h4>


## Table of Contents

- [Getting Started](#getting-started)
  - [Building from source](#build-from-source)
  - [Using with opt](#using-with-opt)
- [Usage](#usage)
  - [Initialize a value context object](#initialize-a-value-context-object)
  - [Initialize a context](#initialize-a-context)
  - [Manipulate dataflow values](#manipulate-dataflow-values)
  - [Update the context graph](#update-the-context-graph)
  - [Retrieve the saved contexts](#retrieve-the-saved-contexts)
  - [Iterate through the context graph](#iterate-through-the-context-graph)

## Getting Started

### Building from source
```sh
$ git clone this_repository.git
$ cd this_repository
$ mkdir build; cd build
$ cmake .. && make
$ make install
```

### Using with opt
* Path to the headers should be searchable by the compiler
* In your LLVM IR pass:
  * Include the header file, ```ValueContext/ValueContext.h```
  * Use namespace ```ValueContextUtil```

## Usage

### Initialize a value context object
Initialize the ```ValueContext``` object with the datatype of your dataflow value  
```cpp
ValueContext<AliasMap> VC(BI, Top);
```

### Initialize a context
Initailize a context for a given ```llvm::Function F``` and initial dataflow value ```Initial``` as follows:  
```cpp
Context C = VC.initializeContext(F, Initial);
```

### Manipulate dataflow values
Manipulate data structures storing dataflow values directly through ```getDataFlowIn``` and ```getDataFlowOut```  
```cpp
auto DataflowValue = VC.getDataFlowIn[Context][Instruction];
```

### Update the context graph
You may want to update the context graph after initializing a new context.   
```cpp
VC.updateContextGraph(InitialContext, NewContext, CallSite);
```

### Retrieve the saved contexts
To get the previously saved context for a given ```llvm::Function F``` and initial dataflow value ```Initial``` do as follow:  
```cpp
auto SavedContext = VC.getSavedContext(F, Initial);
```
if the value of ```SavedContext``` is less than 0, implies that this context was not saved previously.   

### Set result for a saved context
When you reach the boundary instruction, you may want to update the result for the context ```C```
```cpp
VC.setResult(C, DataflowResutant);
```

### Iterate through the context graph 
To iterate over all the contexts which invoked this context using a function call
```cpp
for (auto T : VC.getContextChild(C)) {
    // For a context Child which called the context C though callsite Inst
    // T is the pair of Child and Inst
}
```

