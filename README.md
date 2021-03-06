## popx

### A Reactive Dataflow Framework (JS for now)

This is a placeholder project for a work-in-progress.  Post any questions on the [github repo](https://github.com/mark-hahn/popx).

### Random Notes
- app consists of hierarchical levels
  - module nesting creates tree structure
  - popx modules
    - can be run like code module or script
    - non-leaf modules
    - strictly declarative
    - creates dataflow graphs
    - sets up code module instances and runs them
    - simple custom file format, similar to YAML
  - code modules
    - leaf modules
    - standard code, JS for now
    
    
- popx connections, aka pipes, values, wires
  - multiple readers and writers to single connection
  - any JS value (must be serializable with JSON)
  - change in value can trigger reaction (data-flow)
  - Optionally trigger reaction when writing same value (event)
  - works across processes and networks
  - connections are totally asynchronous
    - no callbacks
  - traffic rate is not meant to be high-speed
    - millisecond rates not microsecond
    - data may be serialized
    - data may cross process/network connections
    - performance achieved by appropriate module arhictecture
    - example is processing web pages as requests come in

      
- popx app
  - App is tree of popx files and inluded code modules
  - root of app tree is always a popx file
  - popx file may include other popx files and code modules
  - code modules can only include other code modules


- popx ide
  - graphical display of modules
    - shows modules as boxes with "pins and wiring"
  - single click pops open popx or code module
    - popx module shown as nested graphics
    - code module shown with normal text-based code editor
    - popx name comes from popping open module
  - runs in browser or Atom desktop editor
  - single click on connection wire shows current value
  - May single step at connection value level
  - Breaks on specific values or change/events on wires
  - May slow down execution to watch values as they change


- popx code modules
  - code modules have interaction rules
    - May call other nested code modules as usual
    - May not call other popx code modules
    - May communicate through inter-module popx connections
    - May use popx global namspaced connections
  - JS version requires ES6 (babel)
    
    
- popx npm module provides code module access to popx connections
  - single popx object, usually in variable named `$`
  - assignment to `$.varname` triggers write/event to connection `varname`
  - referencing `$.varname` reads value from connection
  - function `$.react` is called on `$.varname` input connection value change
    - function can react to one or more connections
    
    
- code module execution
  - top level runs under various specifiable conditions
    - at start of app
    - when any input connection has value
    - when all input connections have value
  - react functions
    - all execution after first run is by reaction only
  - code module execution is normal usually imperative code
    - first implementation only supports ES6 JS


- Parallel processing
  - supports code modules in different processes/cores/servers
  - isomorphic, code can be trivially moved between locations
  - Avoids nodejs single process performance limitation


- Uses
  - apps with circle of human input to processing to output
    - web apps with browser actions to logic to DOM display
    - server is just one component in circle
  - dataflow control apps from sensors to actions


- similar paradigm to reactiveX
  - both use reactive, event-driven pattern
  - both designed to work with any language
  - reactiveX
    - api of functions
    - pure functional
  - popx 
    - much simpler with imperative code modules
    - declarative data-flow connections
    - JS implementation only for now

### Heredoc strings
  - start and end with `"""` or `""""`
  - indent significant
  - indent whitespace not included
  - Triple quotes do not include line endings
  - Quad quotes do include line endings

### Popx Files
  - can include shebang `#!usr/bin/env popx`
  - purely declaritive
  - can be edited as pure text or graphic
  - often created/edited by the popx graphic ide
    - when opened by ide then popx file includes layout info
    - includes modules (boxes), connections (lines), labels, comments

### Popx file format
  - case sensitive
  - single-line comments are #
  - multi-line comments are /* */
  - supports markdown in comments
  - may include only modules (boxes), wires (lines), and constants
  
  - modules
    - module key is directory or file name without `.js` or `.popx`
    - properties
      - key is name of IO pin
        - pin name must match pin defined in module source
      - value
        - name of wire or `<` inline constant
        - missing value means wire name is same as pin name
      - reserved keys
        - *instance:* Name of module instance
          - if no instance key then instance name is module name
          
  - constants
    - key is wire name
    - inline constant connected to single pin needs no wire name


    - simple strings
        - do not require quotes
        - only chars allowed are `<space> a-z A-Z 0-9 $ _`
        - may not include reserved words such as undefined, null, true, false
        
        
    - simple lists
      - do not require `[ ]`
      - indicated by presence of commas
        - single value followed by comma is one-element list

  - wires
    - connections between modules (and constants)
    - each wire always has a single JS value
      - at start, wire value is undefined
      - takes on value when sent from module pin
      - pin may send same value (events)
    - each may have multiple senders and receivers
      - one module pin may both send and recieve
      - value is always last written, not deterministic
    - wires may cross process and cpus
      - value is serialized with JSON
      - wire is masterless
        - server is same as client

  - conventions
    - CSS and HTML for app stored in popx file constants
    - when single-driver, wire names match module pin names
          
          
    
    
### Status
  - Beta
  - used in production
    - 7K-line multi-zone HVAC app
  
### MIT license

