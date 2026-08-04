{
  "name": "Week 8 Atomic Validation Coverage",
  "versions": {
    "attack": "18",
    "navigator": "5.1.0",
    "layer": "4.5"
  },
  "domain": "enterprise-attack",
  "description": "Update test outcomes before publication.",
  "filters": {
    "platforms": [
      "Windows"
    ]
  },
  "sorting": 0,
  "layout": {
    "layout": "side",
    "aggregateFunction": "average",
    "showID": true,
    "showName": true,
    "showAggregateScores": false,
    "countUnscored": false
  },
  "hideDisabled": false,
  "techniques": [
    {
      "techniqueID": "T1082",
      "score": 1,
      "comment": "VAL-001 telemetry validation attempted. DET-004 candidate.",
      "enabled": true
    },
    {
      "techniqueID": "T1016",
      "score": 1,
      "comment": "VAL-002 telemetry validation attempted. DET-005 candidate.",
      "enabled": true
    },
    {
      "techniqueID": "T1059.003",
      "score": 1,
      "comment": "VAL-003 telemetry validation attempted. DET-006 candidate.",
      "enabled": true
    },
    {
      "techniqueID": "T1027",
      "score": 1,
      "comment": "VAL-004 DET-002 validation attempted. Update with pass or miss.",
      "enabled": true
    },
    {
      "techniqueID": "T1110.001",
      "score": 2,
      "comment": "Week 7 DET-001 validated detection.",
      "enabled": true
    },
    {
      "techniqueID": "T1059.001",
      "score": 2,
      "comment": "Week 7 PowerShell detection coverage.",
      "enabled": true
    }
  ],
  "gradient": {
    "colors": [
      "#ffffff",
      "#ffcc66",
      "#66cc99"
    ],
    "minValue": 0,
    "maxValue": 2
  },
  "legendItems": [
    {
      "label": "Telemetry or validation attempt",
      "color": "#ffcc66"
    },
    {
      "label": "Validated detection",
      "color": "#66cc99"
    }
  ],
  "metadata": [
    {
      "name": "Project",
      "value": "Week 8 Atomic Red Team Detection Validation Lab"
    },
    {
      "name": "Scope",
      "value": "Windows 11 ARM isolated lab"
    }
  ],
  "links": [],
  "showTacticRowBackground": false,
  "tacticRowBackground": "#dddddd",
  "selectTechniquesAcrossTactics": true,
  "selectSubtechniquesWithParent": false,
  "selectVisibleTechniques": false
}
