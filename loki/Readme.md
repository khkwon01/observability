## LogQL 

### 1. syntax

| selection | syntax | operations | scope |
|:---:|---|---|---|
|stream selector| { label="value", foo!="bar" } | =, <br> !=, <br> =~, <br> !\~ | select log streams to retrieve |
|line filter| \|= \`error\` | \|=, <br> != <br> \|~, <br> !\~ | filter to matching log lines |
|parser| \| json | json, logfmt, <br> pattern, <br> regexp, unpack | parse and extract labels from the log content |
|label filter| \| label="value" | =, !=, =~, <br> !~, <, <=, <br> >, >= | filter log lines |
|line format| \| line_format "{{.lable}}" | | rewrite the log line content |
|label format| \| new_label="{{.label}}" | | rename, modify or add labels |

- ip address matching
  - single ip : ip("192.168.1.11") and ip("::1")
  - range ip : ip("192.168.1.1-192.168.10.10") and ip("1001:cb2::1-1001:eg2::1")
  - cidr ip : ip("192.168.1.0/24") and ip("1001:cb2::/32")

- decolorize
  - {name="test"} | decolorize
 
- parsers
  
