# Argument Analytics — Design System (extracted)

Derived from index.html, overview.html, documents.html on 2026-05-14.

## Spacing
- Base: **4px**
- Scale: **4, 8, 12, 16, 20, 24, 28, 40, 60**
- Allow 2px (hairline gap)
- Off-grid values found in code: 5, 7, 9, 13, 15, 18, 22, 32, 36, 41, 58, 70

## Radius
- Scale: **2px** (chips/bars), **8px** (cards), **20px** (pills), **50%** (avatar)
- Consolidate the near-duplicates 2/3/4 → pick **2px** for inputs and small surfaces

## Typography
- Family: Lato (400, 500, 700)
- Sizes: **20** (page title), **16** (card title), **14** (body / default), **13** (secondary), **12** (meta / pill)
- Drift to resolve: 10, 15, 18 (collapse 15→14 or 16; 18→16 or 20; 10→12)

## Color

### Text
- Heading: **#1a1a1a**
- Body / strong: **#434345**
- Secondary / meta: **#606060**
- Link / primary: **#1248A0**
- Accent (active/CTA): **#D52B1E**
- Disallow ad-hoc grays: #888, #aaa, #C0C0C0 → map to #606060

### Border (gray ramp — consolidate)
- Strong divider: **#D0D0D0** (inputs, search dropdown)
- Section divider: **#E0E0E0** (tab bar, panel edges)
- Subtle / card chrome: **#F0F0F0**
- Drift to remove: #525253 (keep only inside dark nav), #EFEFEF, #F4F4F5 → collapse to #F0F0F0 or #E0E0E0

### Surface
- Page: **#fff**
- Body: **#f0f0f0**
- Nav dark: **#373739** / **#434345**
- Selected/hover blue tint: **#eef2fa** (pick one)
- Drift to remove: **#f0f4fb**, **#f7f9fc** → consolidate to #eef2fa

## Depth
**Borders-only.** Shadows only allowed for floating overlays (citation popups, dropdown menus).

## Patterns

### Tag pill
- Height: ~22px (padding 3px 10px)
- Border: 1px solid #1248A0
- Radius: 20px
- Font: 12px / #1248A0
- Selected: bg #eef2fa, border-color rgba(18,72,160,0.7)

### Card
- Border: 1px solid #F0F0F0
- Radius: 8px
- Padding: 20px

### Tab
- Padding: 8px 0 10px
- Active: 2px bottom border #D52B1E, color #D52B1E, weight 700
- Spacing between: margin-right 28px

### Search button (primary CTA)
- Width 44px, bg #D52B1E, white icon

### Sidebar filter
- Header: 14px / 700 / #1a1a1a
- Item: 13px / #434345, 3px vertical padding
- Count: 13px / #606060

### Doc list item
- Padding: 14px 16px (consider 16px 16px)
- Border-bottom: 1px solid #F0F0F0
- Selected: bg #eef2fa, 3px left border #1248A0
