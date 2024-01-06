# Tendermint Performance

```bash
{
  "__inputs": [
    {
      "name": "DS_PROMETHEUS",
      "label": "Prometheus",
      "description": "",
      "type": "datasource",
      "pluginId": "prometheus",
      "pluginName": "Prometheus"
    }
  ],
  "__elements": {},
  "__requires": [
    {
      "type": "panel",
      "id": "gauge",
      "name": "Gauge",
      "version": ""
    },
    {
      "type": "grafana",
      "id": "grafana",
      "name": "Grafana",
      "version": "9.2.6"
    },
    {
      "type": "panel",
      "id": "graph",
      "name": "Graph (old)",
      "version": ""
    },
    {
      "type": "datasource",
      "id": "prometheus",
      "name": "Prometheus",
      "version": "1.0.0"
    },
    {
      "type": "panel",
      "id": "stat",
      "name": "Stat",
      "version": ""
    },
    {
      "type": "panel",
      "id": "text",
      "name": "Text",
      "version": ""
    },
    {
      "type": "panel",
      "id": "timeseries",
      "name": "Time series",
      "version": ""
    }
  ],
  "annotations": {
    "list": [
      {
        "builtIn": 1,
        "datasource": {
          "type": "datasource",
          "uid": "grafana"
        },
        "enable": true,
        "hide": true,
        "iconColor": "rgba(0, 211, 255, 1)",
        "name": "Annotations & Alerts",
        "target": {
          "limit": 100,
          "matchAny": false,
          "tags": [],
          "type": "dashboard"
        },
        "type": "dashboard"
      }
    ]
  },
  "description": "A Grafana dashboard compatible with all the cosmos-sdk and tendermint based blockchains.",
  "editable": true,
  "fiscalYearStartMonth": 0,
  "gnetId": 11036,
  "graphTooltip": 0,
  "id": null,
  "links": [
    {
      "asDropdown": false,
      "icon": "external link",
      "includeVars": false,
      "keepTime": false,
      "tags": [],
      "targetBlank": false,
      "title": "Switch",
      "tooltip": "",
      "type": "dashboards",
      "url": ""
    }
  ],
  "liveNow": false,
  "panels": [
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "gridPos": {
        "h": 9,
        "w": 24,
        "x": 0,
        "y": 0
      },
      "id": 66,
      "options": {
        "code": {
          "language": "plaintext",
          "showLineNumbers": false,
          "showMiniMap": false
        },
        "content": "<center>\n<img src=\"https://raw.githubusercontent.com/luneareth/stake.roomit.xyz/main/img/services/logo.png\">\n</center>\n",
        "mode": "html"
      },
      "pluginVersion": "9.2.6",
      "title": "RoomIT",
      "type": "text"
    },
    {
      "collapsed": false,
      "datasource": {
        "type": "prometheus",
        "uid": "lYqnYuHVk"
      },
      "gridPos": {
        "h": 1,
        "w": 24,
        "x": 0,
        "y": 9
      },
      "id": 64,
      "panels": [],
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "lYqnYuHVk"
          },
          "refId": "A"
        }
      ],
      "title": "$chain_id overview",
      "type": "row"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "$DS"
      },
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "locale"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 6,
        "x": 0,
        "y": 10
      },
      "hideTimeOverride": false,
      "id": 4,
      "links": [],
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "textMode": "auto"
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "editorMode": "code",
          "expr": "tendermint_consensus_height{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "hide": false,
          "instant": true,
          "interval": "30s",
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "title": "Block Height",
      "type": "stat"
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "locale"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 6,
        "x": 6,
        "y": 10
      },
      "hideTimeOverride": false,
      "id": 40,
      "links": [],
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "textMode": "auto"
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_total_txs{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "interval": "30s",
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "title": "Total Transactions",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "$DS"
      },
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 2,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "s"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 6,
        "x": 12,
        "y": 10
      },
      "hideTimeOverride": true,
      "id": 39,
      "links": [],
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "area",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "mean"
          ],
          "fields": "",
          "values": false
        },
        "textMode": "auto"
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "editorMode": "code",
          "expr": "tendermint_consensus_height{chain_id=\"$chain_id\", instance=\"$instance\"} / tendermint_consensus_block_interval_seconds_count{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "interval": "30s",
          "intervalFactor": 1,
          "range": true,
          "refId": "A"
        }
      ],
      "timeFrom": "1h",
      "title": "Avg Block Time",
      "type": "stat"
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "short"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 6,
        "x": 18,
        "y": 10
      },
      "hideTimeOverride": false,
      "id": 47,
      "links": [],
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "area",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "textMode": "auto"
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_validators_power{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "instant": false,
          "interval": "30s",
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "title": "Bonded Tokens",
      "type": "stat"
    },
    {
      "aliasColors": {
        "Height for last 3 hours": "#447ebc",
        "Total Transactions for last 3 hours": "#ef843c"
      },
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": {
        "uid": "$DS"
      },
      "decimals": 0,
      "fieldConfig": {
        "defaults": {
          "links": []
        },
        "overrides": []
      },
      "fill": 3,
      "fillGradient": 0,
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 0,
        "y": 14
      },
      "hiddenSeries": false,
      "hideTimeOverride": false,
      "id": 15,
      "legend": {
        "alignAsTable": true,
        "avg": false,
        "current": true,
        "max": true,
        "min": true,
        "rightSide": false,
        "show": true,
        "sideWidth": 350,
        "total": false,
        "values": true
      },
      "lines": true,
      "linewidth": 1,
      "links": [],
      "nullPointMode": "null",
      "options": {
        "alertThreshold": true
      },
      "percentage": false,
      "pluginVersion": "9.2.6",
      "pointradius": 5,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_validators{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "instant": false,
          "interval": "",
          "intervalFactor": 1,
          "legendFormat": "Active",
          "refId": "A"
        },
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_missing_validators{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "interval": "",
          "intervalFactor": 1,
          "legendFormat": "Missing",
          "refId": "B"
        },
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_byzantine_validators{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "intervalFactor": 1,
          "legendFormat": "Byzantine",
          "refId": "C"
        }
      ],
      "thresholds": [],
      "timeRegions": [],
      "title": "Validators",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "mode": "time",
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "decimals": 0,
          "format": "locale",
          "label": "",
          "logBase": 1,
          "min": "0",
          "show": true
        },
        {
          "format": "none",
          "logBase": 1,
          "show": false
        }
      ],
      "yaxis": {
        "align": false
      }
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisCenteredZero": false,
            "axisColorMode": "text",
            "axisLabel": "",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "drawStyle": "line",
            "fillOpacity": 30,
            "gradientMode": "none",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "lineInterpolation": "linear",
            "lineWidth": 1,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "never",
            "spanNulls": false,
            "stacking": {
              "group": "A",
              "mode": "none"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "decimals": 0,
          "links": [],
          "mappings": [],
          "min": 0,
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "short"
        },
        "overrides": [
          {
            "matcher": {
              "id": "byName",
              "options": "Height for last 3 hours"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "#447ebc",
                  "mode": "fixed"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Total Transactions for last 3 hours"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "#ef843c",
                  "mode": "fixed"
                }
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 12,
        "y": 14
      },
      "hideTimeOverride": false,
      "id": 48,
      "links": [],
      "options": {
        "legend": {
          "calcs": [
            "lastNotNull",
            "max",
            "min"
          ],
          "displayMode": "table",
          "placement": "bottom",
          "showLegend": true,
          "width": 350
        },
        "tooltip": {
          "mode": "multi",
          "sort": "none"
        }
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_validators_power{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "instant": false,
          "interval": "",
          "intervalFactor": 1,
          "legendFormat": "Online",
          "refId": "A"
        },
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_missing_validators_power{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "interval": "",
          "intervalFactor": 1,
          "legendFormat": "Missing",
          "refId": "B"
        },
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_byzantine_validators_power{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "intervalFactor": 1,
          "legendFormat": "Byzantine",
          "refId": "C"
        }
      ],
      "title": "Voting Power",
      "type": "timeseries"
    },
    {
      "aliasColors": {
        "Height for last 3 hours": "#447ebc",
        "Total Transactions for last 3 hours": "#ef843c"
      },
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": {
        "uid": "$DS"
      },
      "fieldConfig": {
        "defaults": {
          "links": []
        },
        "overrides": []
      },
      "fill": 3,
      "fillGradient": 0,
      "gridPos": {
        "h": 5,
        "w": 12,
        "x": 0,
        "y": 23
      },
      "hiddenSeries": false,
      "hideTimeOverride": false,
      "id": 49,
      "legend": {
        "alignAsTable": false,
        "avg": true,
        "current": false,
        "max": true,
        "min": false,
        "rightSide": false,
        "show": true,
        "total": false,
        "values": true
      },
      "lines": true,
      "linewidth": 1,
      "links": [],
      "nullPointMode": "null",
      "options": {
        "alertThreshold": true
      },
      "percentage": false,
      "pluginVersion": "9.2.6",
      "pointradius": 5,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_block_size_bytes{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "instant": false,
          "interval": "",
          "intervalFactor": 1,
          "legendFormat": "Block Size",
          "refId": "A"
        }
      ],
      "thresholds": [],
      "timeRegions": [],
      "title": "Block Size",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "mode": "time",
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "decimals": 2,
          "format": "bytes",
          "label": "",
          "logBase": 1,
          "min": "0",
          "show": true
        },
        {
          "format": "none",
          "logBase": 1,
          "show": false
        }
      ],
      "yaxis": {
        "align": false
      }
    },
    {
      "aliasColors": {
        "Height for last 3 hours": "#447ebc",
        "Total Transactions for last 3 hours": "#ef843c"
      },
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": {
        "uid": "$DS"
      },
      "decimals": 0,
      "fieldConfig": {
        "defaults": {
          "links": []
        },
        "overrides": []
      },
      "fill": 3,
      "fillGradient": 0,
      "gridPos": {
        "h": 5,
        "w": 12,
        "x": 12,
        "y": 23
      },
      "hiddenSeries": false,
      "hideTimeOverride": false,
      "id": 50,
      "legend": {
        "alignAsTable": false,
        "avg": true,
        "current": false,
        "max": true,
        "min": false,
        "rightSide": false,
        "show": true,
        "total": true,
        "values": true
      },
      "lines": true,
      "linewidth": 1,
      "links": [],
      "nullPointMode": "null",
      "options": {
        "alertThreshold": true
      },
      "percentage": false,
      "pluginVersion": "9.2.6",
      "pointradius": 5,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_consensus_num_txs{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "instant": false,
          "interval": "",
          "intervalFactor": 1,
          "legendFormat": "Transactions",
          "refId": "A"
        }
      ],
      "thresholds": [],
      "timeRegions": [],
      "title": "Transactions",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "mode": "time",
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "decimals": 0,
          "format": "short",
          "label": "",
          "logBase": 1,
          "min": "0",
          "show": true
        },
        {
          "format": "none",
          "logBase": 1,
          "show": false
        }
      ],
      "yaxis": {
        "align": false
      }
    },
    {
      "collapsed": false,
      "datasource": {
        "type": "prometheus",
        "uid": "lYqnYuHVk"
      },
      "gridPos": {
        "h": 1,
        "w": 24,
        "x": 0,
        "y": 28
      },
      "id": 55,
      "panels": [],
      "repeat": "instance",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "lYqnYuHVk"
          },
          "refId": "A"
        }
      ],
      "title": "instance overview: $instance",
      "type": "row"
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [],
          "max": 20,
          "min": 0,
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "#e24d42"
              },
              {
                "color": "#ef843c",
                "value": 2
              },
              {
                "color": "#7eb26d",
                "value": 5
              }
            ]
          },
          "unit": "short"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 6,
        "x": 0,
        "y": 29
      },
      "hideTimeOverride": true,
      "id": 53,
      "links": [],
      "options": {
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "last"
          ],
          "fields": "",
          "values": false
        },
        "showThresholdLabels": false,
        "showThresholdMarkers": true
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_p2p_peers{chain_id=\"$chain_id\", instance=~\"$instance\"}",
          "format": "time_series",
          "instant": true,
          "interval": "30s",
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "title": "Connected Peers",
      "type": "gauge"
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [],
          "max": 50,
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "#7eb26d"
              },
              {
                "color": "#ef843c",
                "value": 10
              },
              {
                "color": "#e24d42",
                "value": 20
              }
            ]
          },
          "unit": "short"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 6,
        "x": 6,
        "y": 29
      },
      "hideTimeOverride": true,
      "id": 56,
      "links": [],
      "options": {
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "last"
          ],
          "fields": "",
          "values": false
        },
        "showThresholdLabels": false,
        "showThresholdMarkers": true
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_mempool_size{chain_id=\"$chain_id\", instance=~\"$instance\"}",
          "format": "time_series",
          "instant": true,
          "interval": "30s",
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "title": "Unconfirmed Transactions",
      "type": "gauge"
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green"
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "short"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 6,
        "x": 12,
        "y": 29
      },
      "hideTimeOverride": true,
      "id": 60,
      "links": [],
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "textMode": "auto"
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_mempool_failed_txs{chain_id=\"$chain_id\", instance=~\"$instance\"}",
          "format": "time_series",
          "instant": true,
          "interval": "30s",
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "title": "Failed Transactions",
      "type": "stat"
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green"
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "locale"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 6,
        "x": 18,
        "y": 29
      },
      "hideTimeOverride": true,
      "id": 61,
      "links": [],
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "textMode": "auto"
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_mempool_recheck_times{chain_id=\"$chain_id\", instance=~\"$instance\"}",
          "format": "time_series",
          "instant": true,
          "interval": "30s",
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "title": "Recheck Times",
      "type": "stat"
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisCenteredZero": false,
            "axisColorMode": "series",
            "axisGridShow": true,
            "axisLabel": "",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "drawStyle": "line",
            "fillOpacity": 100,
            "gradientMode": "hue",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "lineInterpolation": "smooth",
            "lineStyle": {
              "fill": "solid"
            },
            "lineWidth": 1,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "always",
            "spanNulls": true,
            "stacking": {
              "group": "A",
              "mode": "none"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "links": [],
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green"
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 0,
        "y": 33
      },
      "id": 59,
      "links": [],
      "options": {
        "legend": {
          "calcs": [],
          "displayMode": "table",
          "placement": "right",
          "showLegend": true
        },
        "tooltip": {
          "mode": "single",
          "sort": "none"
        }
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_p2p_peer_receive_bytes_total{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "intervalFactor": 1,
          "legendFormat": "{{peer_id}}",
          "refId": "A"
        }
      ],
      "title": "Total Network Input",
      "type": "timeseries"
    },
    {
      "datasource": {
        "uid": "$DS"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisCenteredZero": false,
            "axisColorMode": "text",
            "axisLabel": "",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "drawStyle": "bars",
            "fillOpacity": 100,
            "gradientMode": "none",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "lineInterpolation": "linear",
            "lineWidth": 1,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "never",
            "spanNulls": false,
            "stacking": {
              "group": "A",
              "mode": "none"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "links": [],
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green"
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 12,
        "y": 33
      },
      "id": 58,
      "links": [],
      "options": {
        "legend": {
          "calcs": [],
          "displayMode": "list",
          "placement": "bottom",
          "showLegend": false
        },
        "tooltip": {
          "mode": "single",
          "sort": "none"
        }
      },
      "pluginVersion": "9.2.6",
      "targets": [
        {
          "datasource": {
            "uid": "$DS"
          },
          "expr": "tendermint_p2p_peer_send_bytes_total{chain_id=\"$chain_id\", instance=\"$instance\"}",
          "format": "time_series",
          "intervalFactor": 1,
          "legendFormat": "{{peer_id}}",
          "refId": "A"
        }
      ],
      "title": "Total Network Output",
      "type": "timeseries"
    }
  ],
  "refresh": false,
  "schemaVersion": 37,
  "style": "dark",
  "tags": [
    "Blockchain",
    "Cosmos"
  ],
  "templating": {
    "list": [
      {
        "current": {
          "selected": false,
          "text": "Prometheus",
          "value": "Prometheus"
        },
        "hide": 0,
        "includeAll": false,
        "label": "Datasource",
        "multi": false,
        "name": "DS",
        "options": [],
        "query": "prometheus",
        "refresh": 1,
        "regex": "",
        "skipUrlSync": false,
        "type": "datasource"
      },
      {
        "current": {},
        "datasource": {
          "uid": "$DS"
        },
        "definition": "label_values(tendermint_consensus_height, chain_id)",
        "hide": 0,
        "includeAll": false,
        "label": "Chain ID",
        "multi": false,
        "name": "chain_id",
        "options": [],
        "query": {
          "query": "label_values(tendermint_consensus_height, chain_id)",
          "refId": "Prometheus-chain_id-Variable-Query"
        },
        "refresh": 1,
        "regex": "",
        "skipUrlSync": false,
        "sort": 0,
        "tagValuesQuery": "",
        "tagsQuery": "",
        "type": "query",
        "useTags": false
      },
      {
        "allValue": "",
        "current": {},
        "datasource": {
          "uid": "$DS"
        },
        "definition": "label_values(tendermint_consensus_height{chain_id=\"$chain_id\"}, instance)",
        "hide": 0,
        "includeAll": false,
        "label": "Instance",
        "multi": false,
        "name": "instance",
        "options": [],
        "query": {
          "query": "label_values(tendermint_consensus_height{chain_id=\"$chain_id\"}, instance)",
          "refId": "Prometheus-instance-Variable-Query"
        },
        "refresh": 1,
        "regex": "",
        "skipUrlSync": false,
        "sort": 5,
        "tagValuesQuery": "",
        "tagsQuery": "",
        "type": "query",
        "useTags": false
      }
    ]
  },
  "time": {
    "from": "now-24h",
    "to": "now"
  },
  "timepicker": {
    "refresh_intervals": [
      "5s",
      "10s",
      "30s",
      "1m",
      "5m",
      "15m",
      "30m",
      "1h",
      "2h",
      "1d"
    ],
    "time_options": [
      "5m",
      "15m",
      "1h",
      "6h",
      "12h",
      "24h",
      "2d",
      "7d",
      "30d"
    ]
  },
  "timezone": "",
  "title": "RoomIT | Tendermint Performance",
  "uid": "UJyurCTWz",
  "version": 14,
  "weekStart": ""
}
```
