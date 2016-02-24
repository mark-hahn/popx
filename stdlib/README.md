
## Popx standard libray

Collection of standard JS code modules to be included in popx declarative modules.

### dom
  - **textinput**
    - attaches to text input element to provide value and emit events
    - pins
      - *selector* input: CSS selector for input tag
      - *value* output: Value of input contents
      - *changed* output: Event emitted on changed event with event object value

### utils
  - **log**
    - each pin input creates a timestamped line with pin name and value
    - pins
      - *pins** input: any number of input pins that create log lines
      - *console* boolean input: send log to stdout (default:true)
      - *path* output: optional file path 
      