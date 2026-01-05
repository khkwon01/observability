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
  - json
  - logfmt
  - pattern
  - regexp
  - unpack (promtail...)
 
- template functions
  - label as variables  {{ .component }}
  - access log line  {{ __line__ | lower }}
  - access timestamp  {{ __timestamp__ | date "~~~" }}

- log transforming
  - line format => line_format "{{ .label }}"
    {instance="test", service="test"} |= `sent` | json | regexp "(?P<method>sent) (?P<status>\\d+?) | line_format "{{.method}} {{.status}}"

- log aggregation
  - rate(range)
  - count_over_time(range)
  - bytes_rate(range)
  - bytes_over_time(range)
  - absent_over_time(range)
 
  ```
  count_over_time({service="test"}[$__auto])
  sum by (test) (rate({service=~".+test"} |= "error" [1m]))

  sum by (org_id) (
    sum_over_time(
      {service="test"}
         |= "test"
         | logfmt
         | unwrap bytes(bytes_processed) [1m])
    )
  )
  ```

- built-in aggregation
  - sum, avg, min, max, stddev, stdvar, count, topk, bottommk, sort, sort_desc
  ```
  topk(10, sum(rate({service="test"}[10m])) by (name))
  avg(rate(({service="test"} |= "GET" | json | path="/test")[10s])) by (region)
  ```

### 2. Architecture
<img width="427" height="333" alt="image" src="https://github.com/user-attachments/assets/9fd106bc-80f1-454c-bc56-13e96957fd42" />

  
    
  
