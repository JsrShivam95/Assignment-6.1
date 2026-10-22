1. box_model.htmlHTML<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Assignment 6.1 — CSS Box Model Inspection</title>
  <meta name="description" content="Visualizing and inspecting CSS box model layers and box-sizing behaviors in DevTools.">
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>📦</text></svg>">
  <link rel="stylesheet" href="box_model.css">
</head>
<body>

  <header class="header">
    <h1>CSS Box Model Inspection Lab</h1>
    <p>Compare how padding, border, margin, and <code>box-sizing</code> determine rendered layout footprint.</p>
  </header>

  <main class="container">

    <!-- Card 1: Default / Small values with border-box -->
    <div class="box card-one">
      <h2>Box 01 &mdash; Compact Baseline</h2>
      <p>
        <strong>Sizing:</strong> <code>border-box</code><br>
        <strong>Padding:</strong> 12px | <strong>Border:</strong> 2px solid<br>
        <strong>Margin:</strong> 16px bottom<br>
        Total width stays locked to exactly 320px.
      </p>
    </div>

    <!-- Card 2: Generous padding & thick border with border-box -->
    <div class="box card-two">
      <h2>Box 02 &mdash; Cushioned Card</h2>
      <p>
        <strong>Sizing:</strong> <code>border-box</code><br>
        <strong>Padding:</strong> 28px | <strong>Border:</strong> 6px solid<br>
        <strong>Margin:</strong> 24px bottom<br>
        Content automatically shrinks inward to keep the overall width at 320px.
      </p>
    </div>

    <!-- Card 3: Explicit content-box to demonstrate box explosion -->
    <div class="box card-three">
      <h2>Box 03 &mdash; Content-Box Growth</h2>
      <p>
        <strong>Sizing:</strong> <code>content-box</code><br>
        <strong>Padding:</strong> 28px | <strong>Border:</strong> 6px dashed<br>
        <strong>Margin:</strong> 32px bottom<br>
        Declared width (320px) applies only to content. Total width expands to 388px!
      </p>
    </div>

  </main>

  <footer class="footer">
    <p>&copy; 2026 Box Model Inspection Lab &middot; Inspect via Chrome DevTools</p>
  </footer>

</body>
</html>
2. box_model.cssCSS/* ==========================================================================
   Base Reset
   Note: We deliberately do NOT apply universal box-sizing here so we can 
   explicitly demonstrate content-box vs. border-box on the cards.
   ========================================================================== */
* {
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  background-color: #f1f5f9;
  color: #1e293b;
  line-height: 1.6;
  padding: 32px 16px;
}

.header {
  text-align: center;
  margin-bottom: 36px;
}

.header h1 {
  font-size: 2rem;
  color: #0f172a;
  margin-bottom: 8px;
}

.header p {
  color: #64748b;
  font-size: 1rem;
}

.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
  gap: 24px;
  max-width: 1200px;
  margin: 0 auto;
}

/* Base style shared across all 3 cards */
.box {
  width: 320px;
  background-color: #ffffff;
  border-radius: 8px;
}

.box h2 {
  font-size: 1.2rem;
  margin-bottom: 10px;
  color: #0f172a;
}

.box p {
  font-size: 0.92rem;
  color: #475569;
}

.box code {
  background-color: #e2e8f0;
  padding: 2px 6px;
  border-radius: 4px;
  font-size: 0.85rem;
}

/* ==========================================================================
   Step 3 & 4: Distinct Padding, Border, Margin & Box-Sizing
   ========================================================================== */

/* Box 01: Standard compact border-box */
.card-one {
  box-sizing: border-box;
  padding: 12px;
  border: 2px solid #3b82f6;          /* Blue border */
  margin-bottom: 16px;
  background-color: #f0f7ff;
}

/* Box 02: Generous padding, heavy border, but still border-box */
.card-two {
  box-sizing: border-box;
  padding: 28px;
  border: 6px solid #10b981;          /* Green border */
  margin-bottom: 24px;
  background-color: #ecfdf5;
}

/* Box 03: content-box (W3C Default) showing width expansion */
.card-three {
  box-sizing: content-box;
  padding: 28px;
  border: 6px dashed #f59e0b;         /* Amber dashed border */
  margin-bottom: 32px;
  background-color: #fffbeb;
}

/* Footer */
.footer {
  text-align: center;
  margin-top: 48px;
  color: #94a3b8;
  font-size: 0.85rem;
}
3. Step 4: content-box vs. border-box ArithmeticWhen you inspect the elements in Chrome DevTools, here is the exact math governing their actual physical width on the screen:$$\text{Rendered Width} = \text{Width} + \text{Left/Right Padding} + \text{Left/Right Border}$$ElementDeclared widthPadding (L+R)Border (L+R)Formula appliedRendered On-Screen WidthBox 02 (border-box)320px$28\text{px} + 28\text{px} = 56\text{px}$$6\text{px} + 6\text{px} = 12\text{px}$Content width shrinks to $320 - (56 + 12) = \mathbf{252\text{px}}$320px (Preserved)Box 03 (content-box)320px$28\text{px} + 28\text{px} = 56\text{px}$$6\text{px} + 6\text{px} = 12\text{px}$Rendered width expands to $320 + 56 + 12 = \mathbf{388\text{px}}$388px (Exploded by 68px!)Takeaway: With border-box, padding and borders stay inside the declared width. With content-box, padding and borders are added outside, disrupting grid alignments and pushing sibling elements around.4. Step 5 & 6: DevTools Inspection & Annotated Box Model GuideHow to Inspect the Diagram in Chrome DevToolsDouble-click box_model.html to open it in Google Chrome.Press F12 (or right-click Box 03 and select Inspect).In the right-hand panel, select the Styles or Computed tab.Scroll to the bottom of the pane to locate the interactive Box Model Diagram.+--------------------------------------------------------------+
|                         MARGIN                               |
|        (Transparent space separating this element            |
|         from its surrounding sibling elements)               |
|                                                              |
|       +----------------------------------------------+       |
|       |                     BORDER                   |       |
|       |         (Visible edge or decorative line     |       |
|       |          encapsulating the padding/content)  |       |
|       |                                              |       |
|       |       +------------------------------+       |       |
|       |       |            PADDING           |       |       |
|       |       |    (Clearing space between   |       |       |
|       |       |     the border and content)  |       |       |
|       |       |                              |       |       |
|       |       |       +--------------+       |       |       |
|       |       |       |   CONTENT    |       |       |       |
|       |       |       | (Text/Images |       |       |       |
|       |       |       |  e.g., h2, p)|       |       |       |
|       |       |       +--------------+       |       |       |
|       |       |                              |       |       |
|       |       +------------------------------+       |       |
|       +----------------------------------------------+       |
+--------------------------------------------------------------+
Explanations of the 4 Box Model Layers (for your submission write-up)Content (Innermost):The central area where text, images, or child elements render.Represented by the blue box in Chrome DevTools (showing width $\times$ height, e.g., 252 x 128 or 320 x 128).Padding:The transparent clearance space between the content and the inner perimeter of the border.Adopts the background color of the element. Represented by the green layer in DevTools.Border:The perimeter line enclosing the padding and content.Can have specific thickness, styles (solid, dashed), and colors. Represented by the yellow/amber layer in DevTools.Margin (Outermost):The empty space surrounding the border that pushes other neighboring elements away.Margins collapse vertically and are transparent. Represented by the orange layer in DevTools.