# Routing Tool for Alternate Golf Holes

A two-stage tool for designing and solving alternate golf hole routings. Map which greens are reachable from each tee, then find valid 18-hole sequences.

## Live Demo

**🔗 [https://alternateroutings.vercel.app/](https://alternateroutings.vercel.app/)**

## Project Overview

This project helps design alternate golf course routings where tees can play to non-standard greens. Instead of always playing Tee 1 to Green 1, explore creative routing options like Tee 1 to Green 9.

**Use case:** Course layouts where multiple greens are accessible from a single tee, creating opportunities for varied hole sequences.

## Two-Stage Workflow

### Stage 1: Data Collection
Use the Tee-Green Mapper to record which of the 17 alternate greens (excluding the standard green) are reachable from each tee. Saves to `tee-green-mapping.json`

### Stage 2: Route Finding
Use the Routing Solver to find valid 18-hole sequences. The solver ensures each tee and green is used exactly once, following the rule: after playing to Green X, the next hole starts at Tee X+1.

## Tools

### Tee to Green Mapper (Stage 1)
Interactive mapper for recording which greens are reachable from each tee. Select a tee (1-18), click reachable greens, save and move to the next. Progress tracked visually. Data auto-saves to browser and exports to JSON.

**Features:**
- 18 clickable tee badges for quick navigation
- Dynamic green selection (excludes matching tee number)
- Progress tracking (X/18 tees mapped)
- Auto-save to localStorage + JSON export/import

### Golf Routing Solver (Stage 2)
Backtracking algorithm that finds all valid 18-hole routings from your mapping data. Features: custom starting tee, routing constraints, next-tee options, and partial routing display when complete sequences aren't possible.

**Features:**
- Selectable starting tee (1-18)
- Tee-to-green constraints (force specific pairings)
- Next-tee options (override default Green X → Tee X+1 logic)
- Finds all valid solutions or best partial routing
- Export solutions as JSON

## Routing Logic

### Default Rule
After finishing at Green X, the next hole starts at Tee (X+1)

**Example:** Tee 1 → Green 9, then Tee 10 → Green Y, then Tee (Y+1) → ...

### Constraints
- Each tee used once
- Each green used once
- Exactly 18 holes
- Green 18 wraps to Tee 1

### Custom Next-Tee Options
Override the default routing logic to allow multiple routing choices after specific greens. For example, after finishing at Green 9, you could specify that the next hole can start at Tee 1, Tee 10, or Tee 17.

## Getting Started

1. Open the **Tee-Green Mapper** and record your course's alternate green options
2. Download `tee-green-mapping.json` when complete
3. Open the **Routing Solver** and load your mapping data
4. (Optional) Add constraints or next-tee options
5. Click "Find Routings" to discover valid 18-hole sequences

## Technical Details

### Tee-Green Mapper
- Pure HTML/CSS/JavaScript (no dependencies)
- Data persistence via localStorage
- JSON export/import for data portability
- Responsive grid layout for green selection
- Real-time progress tracking

### Routing Solver
- Backtracking algorithm with constraint satisfaction
- Tracks both used tees and used greens
- Supports custom starting positions
- Configurable next-tee routing options
- Returns complete solutions or longest partial paths
- Multiple solution exploration with prev/next navigation

### Data Format

**Mapping Data** (`tee-green-mapping.json`):
```json
{
  "tee_1": [2, 5, 9, 12],
  "tee_2": [1, 3, 8],
  ...
}
```

**Next-Tee Options** (`next-tee-options.json`):
```json
{
  "next_tee_options": {
    "9": [1, 10, 17],
    "11": [1, 12, 17]
  }
}
```

**Routing Solution**:
```json
{
  "solution_number": 1,
  "holes_count": 18,
  "is_complete": true,
  "routing": [
    { "tee": 1, "green": 9 },
    { "tee": 10, "green": 5 },
    ...
  ]
}
```

## Algorithm

The routing solver uses a recursive backtracking algorithm:

1. Start from specified tee (default: Tee 1)
2. For current tee, get available greens from mapping data
3. Apply constraints (if any) to filter greens
4. For each available green:
   - Check if green has been used
   - Determine next tee(s) based on routing rules
   - Check if next tee has been used
   - Recursively explore from next tee
5. Track longest path found if complete solution not possible
6. Return all valid 18-hole sequences

## Browser Compatibility

Works in all modern browsers that support:
- ES6 JavaScript (Set, Map, arrow functions)
- CSS Grid
- localStorage
- File API (for JSON import/export)

Tested on Chrome, Firefox, Safari, and Edge.

## License

MIT
