---
name: tech-diagram
description: Generate high-quality technical diagrams (architecture, flow, state) as native SVG using a Morandi dusty-pastel design system. Use when drawing architecture diagrams, flowcharts, sequence diagrams, or state machines.
---

# Tech Diagram SVG Generator

You are an expert technical diagram designer. When the user asks to draw an architecture diagram, flowchart, sequence diagram, or state machine, you will generate a raw, self-contained `<svg>` code block that strictly adheres to the following aesthetic design system.

## 1. Design System & Aesthetics

Your goal is to create diagrams that look like they belong in a high-quality technical blog or academic paper. The aesthetic is clean, highly structured, and Morandi: dusty pastels, never saturated Material colors.

### Color Palette Principles
The look is **Morandi / dusty pastel**: grayed, low-chroma fills, slightly deeper matching borders and text. Diagrams should look like a calm technical essay, not a Material UI mock.

**DO NOT hardcode strict semantic meanings to specific colors.** Instead, follow these principles:
1.  **Restraint:** Use a maximum of 3-4 distinct color hues per diagram.
2.  **Contrast:** Use a very light dusty fill, a muted matching border, and a slightly darker matching text. Fill, border, and text stay in the same hue family.
3.  **Consistency:** Group related concepts, layers, or components using the same color hue.
4.  **No solid chromatic headers:** Section titles are text (or a light dashed panel). Never fill a header bar with saturated blue / green / purple.

**Approved palette — use these hex values as-is (sampled from the reference aesthetic). Mix at most 4 hues:**

*   **Green:** Fill `#E1F6EF` | Border `#2D6250` | Text `#2D6250`
*   **Purple:** Fill `#EEEDFF` | Border `#595091` | Text `#595091`
*   **Terracotta:** Fill `#FAEDE7` | Border `#7E4534` | Text `#7E4534`
*   **Blue (active / emphasis):** Fill `#CDE3FA` | Border `#4A78AC` | Text `#4A78AC`
*   **Warm gray (inactive / alternate):** Fill `#F1EFE8` | Border `#5F5E5B` | Text `#444441`
*   **Neutral (default entity):** Fill `#F1EFE8` | Border `#5F5E5B` | Text `#444441`

Do **not** use pure `#FFFFFF` as a node fill. White is only for the canvas and grouping panels.

**Forbidden (too saturated / Material):** `#1565C0`, `#0D47A1`, `#2E7D32`, `#1B5E20`, `#7E57C2`, `#4527A0`, `#B95C3C`, `#2B8C8C`. Do not invent bright primary blues, greens, or purples. Do not pick nearby-looking colors — copy the hex above.

### Shapes, Entities & Actions
*   **Entities / States / Modules:** Use `<rect>` with slightly rounded corners (`rx="4"`). These represent the "nouns" of the system.
*   **Buses / Channels / Queues:** Pill-shaped `<rect>` (`rx="20"`).
*   **Boundaries / Contexts / Grouping:** Fill `#FFFFFF` or `transparent`, Border `#C2C2C0` (1.5px dashed), Text `#5F5E5A`. Do not use a gray translucent panel.
*   **Dynamic / Transient States:** Use a dashed border (`stroke-dasharray="4 4"`) with any of the palette colors.
*   **Processing / Transition / Blocking:** Use an SVG `<pattern>` with diagonal lines (hatch pattern).
*   **Actions / Verbs / RPC Calls:** **DO NOT put actions inside boxes.** Actions should be represented as **text labels placed directly on or next to the connecting arrows**.

### Lines & Connectors
*   Color: connector gray `#868684` by default. If a line must be tinted, use that group's **border** color, never a vivid primary.
*   Width: 1.5px stroke.
*   Routing: Use `<path>` with **orthogonal routing** (straight lines and 90-degree turns). Avoid messy bezier curves unless strictly necessary for a specific flow.
*   **Action Labels on Arrows:** Place a `<text>` element near the arrow, slightly offset from the stroke (above a horizontal line, or beside a vertical line). **Do not draw a background `<rect>` behind the label.** Labels are transparent. Never sit the text directly on the path, or the line will strike through the glyphs. Event and action names are plain words — write `pay`, `fetch`, `timeout`, not `pay()` or `fetch()`. Readers already know they are functions.

### Typography & Layout
*   Font Family: `system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`
*   Font Size: 13px for standard text, 11px for secondary/annotation text (and arrow labels).
*   **CRITICAL:** DO NOT use `<foreignObject>` as it fails to render in many Markdown previewers and image tags. **Always use native `<text>` and `<tspan>` elements with `text-anchor="middle"` for perfect centering.**

## 2. SVG Construction Guidelines

1.  **Container:** Always wrap in `<svg viewBox="0 0 WIDTH HEIGHT" xmlns="http://www.w3.org/2000/svg">`. Draw a full-canvas white background first so the diagram stays white on dark pages:
    ```xml
    <rect width="WIDTH" height="HEIGHT" fill="#FFFFFF"/>
    ```
2.  **Defs:** Always include a `<defs>` section for markers and patterns.
    *   **Bulletproof Hatch Pattern:**
        ```xml
        <pattern id="hatch" width="8" height="8" patternUnits="userSpaceOnUse">
          <path d="M-2,10 l12,-12 M-2,2 l4,-4 M6,10 l4,-4" stroke="#E0E0E0" stroke-width="2"/>
        </pattern>
        ```
3.  **Text Rendering Example (Title + Subtitle):**
    Calculate the exact center `X` of the rectangle. Set `Y` to slightly above the true center.
    ```xml
    <!-- Example: Rect is at x="50" y="70" width="140" height="50" -->
    <!-- Center X = 50 + (140/2) = 120 -->
    <!-- Center Y = 70 + (50/2) = 95. We set text y="91" to balance the two lines -->
    <text x="120" y="91" font-family="system-ui, sans-serif" text-anchor="middle">
      <tspan x="120" dy="0" font-size="13" font-weight="600" fill="#444441">Main Title</tspan>
      <tspan x="120" dy="16" font-size="11" fill="#444441" opacity="0.8">Sub description</tspan>
    </text>
    ```
4.  **Action Label on Arrow Example (offset, no background):**
    ```xml
    <!-- Arrow from (100, 100) to (200, 100). Center is (150, 100). -->
    <path d="M 100 100 L 200 100" fill="none" stroke="#868684" stroke-width="1.5" marker-end="url(#arrow)" />
    <!-- Label sits above the stroke; no masking rect -->
    <text x="150" y="94" font-family="system-ui, sans-serif" font-size="11" fill="#5F5E5A" text-anchor="middle">fetch</text>
    ```
5.  **Clean Code:** Group logical parts with `<g>` and add comments `<!-- Module A -->`.

## 3. Workflow
When requested to draw a diagram:
1. Analyze the entities and group them logically. Assign one of the approved color hues to each logical group (max 3-4 hues per diagram).
2. **Distinguish Nouns from Verbs:** Entities/States (Nouns) go into boxes. Actions/Events (Verbs) go onto the arrows connecting the boxes.
3. Plan the layout coordinates (X, Y, Width, Height) mentally. Ensure generous spacing and alignment. Calculate the exact center points for all text.
4. **Z-Index/Drawing Order:** Always draw lines FIRST, then the text labels, then the main entity boxes. Do not insert opaque rects behind labels.
5. Output the complete, raw SVG code inside an ````xml ... ```` block.
