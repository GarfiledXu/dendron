---
id: yspvrqx4d3hluxztv2g1nzn
title: Logview
desc: ''
updated: 1748916403108
created: 1748916399869
---

// VSCode Plugin: Log Viewer with Filter & Highlight Features
// Author: Based on your requirements

// Project Outline (Markdown/Planning Style)

## Plugin Name
`log-filter-highlight`

## Description
A VSCode extension for viewing, highlighting, and filtering logs with support for multiple rulesets per firmware version. Includes sidebar UI, rule management, per-file and workspace-wide filtering, and export functionality.

## Features
- Sidebar icon with full rule configuration panel
- Per-firmware-version ruleset support
- Log unit filtering (e.g., motor module, protocol module, etc.)
- Highlighter support using existing VSCode plugin (`naumovs.color-highlight` style)
- File-by-file or workspace-wide processing
- Filter & highlight features are separately toggleable
- Display filtered results in a side-by-side virtual document
- Export filtered results to a timestamped directory

## File Structure
```bash
log-filter-highlight/
├── package.json
├── extension.js
├── src/
│   ├── sidebar.js
│   ├── rulesetManager.js
│   ├── filterEngine.js
│   ├── highlighter.js
│   ├── exportManager.js
├── media/
│   └── sidebar-ui.html
├── schemas/
│   └── ruleset-schema.json
└── README.md
```

## Key Technologies
- JavaScript (Node.js, VSCode API)
- WebView (for sidebar UI)
- JSON for ruleset definitions
- `vscode.TextDocument` & `vscode.workspace` API

## Example Workflow
1. Click sidebar icon -> open rule configuration panel
2. Add/select firmware version -> define filtering units and rules
3. Enable/disable highlighting and filtering
4. Choose: current file or all workspace files
5. Run filter -> output in a new tab or export to folder

## Ruleset Example (JSON)
```json
{
  "firmware": "v1.2.3",
  "modules": [
    {
      "name": "motor",
      "enabled": true,
      "patterns": ["MOTOR_START", "MOTOR_STOP"]
    },
    {
      "name": "protocol:login",
      "enabled": false,
      "patterns": ["LOGIN_REQ", "LOGIN_ACK"]
    }
  ]
}
```

## Export Result Format
```
/exported_logs/
  20250529_153022/
    file1_filtered.log
    file2_filtered.log
```

## Highlighting Logic
- Wrap matches with span class
- Use CSS to define color codes

## UI Components
- Sidebar: firmware version dropdown, module/unit checklist
- Buttons: Save Rules, Apply Filter, Export, Toggle Highlighting

## Dependencies
- `vscode` API
- Optional: `highlight.js` or similar for HTML coloring (for preview)

## Next Steps
- Scaffold extension using `yo code`
- Build sidebar UI and state management
- Implement rule parser and engine
- Hook into file and workspace APIs
- Add export and virtual document preview logic

---
Would you like help scaffolding the actual plugin with `yo code`, or should we proceed with the code implementation for a specific part?
