# Design Preservation Guide

## Objective

Preserve **exact** design system from `blytz.work` when migrating to T3 stack in `t3.blytz.work`.

**Key Goal**: Zero visual differences. Users should not notice any design changes.

---

## Design Assets to Copy

### 1. Tailwind Configuration

**Source**: `/home/gmsas95/blytz.work/frontend/tailwind.config.ts`

**Destination**: `/home/gmsas95/t3.blytz.work/tailwind.config.ts`

**Action**: Copy file exactly as-is

**What this preserves**:
- Custom color palette
- Custom spacing scale
- Custom font sizes
- Custom border radius
- Custom shadows
- Custom animations

---

### 2. Global Styles

**Source**: `/home/gmsas95/blytz.work/frontend/src/styles/globals.css`

**Destination**: `/home/gmsas95/t3.blytz.work/src/app/globals.css`

**Action**: Copy entire file exactly as-is

**What this preserves**:
- CSS custom properties (--primary, --secondary, etc.)
- Global CSS resets
- Base typography styles
- Utility classes
- Animation keyframes

---

### 3. Radix UI Components

**Source Directory**: `/home/gmsas95/blytz.work/frontend/src/components/`

**Destination Directory**: `/home/gmsas95/t3.blytz.work/src/components/`

**Action**: Copy entire `components/` directory recursively

**Action**:
```bash
cp -r /home/gmsas95/blytz.work/frontend/src/components/* \
      /home/gmsas95/t3.blytz.work/src/components/
```

**Components to Copy**:

#### Auth Components
- `auth/EnhancedAuthForm.tsx`
- `auth/SimpleAuthForm.tsx`

#### UI Components (Radix UI)
- `ui/Button.tsx`
- `ui/Input.tsx`
- `ui/Textarea.tsx`
- `ui/Select.tsx`
- `ui/Dialog.tsx`
- `ui/DropdownMenu.tsx`
- `ui/Avatar.tsx`
- `ui/Card.tsx`
- `ui/Table.tsx`
- `ui/Tabs.tsx`
- `ui/Switch.tsx`
- `ui/Slider.tsx`
- `ui/Toast.tsx`
- `ui/Alert.tsx`
- And all other Radix UI components in use

#### Layout Components
- `Footer.tsx`
- `Header.tsx`
- `Navigation.tsx`
- `Sidebar.tsx`

#### Dashboard Components
- `DashboardLayout.tsx`
- `StatCard.tsx`
- `RecentActivity.tsx`
- `QuickActions.tsx`

---

### 4. Design Tokens (from globals.css)

**Color Palette**

Preserve exact color values:

```css
/* Primary Colors */
--color-primary: #<exact-value-from-blytz.work>;
--color-primary-light: #<exact-value>;
--color-primary-dark: #<exact-value>;

/* Secondary Colors */
--color-secondary: #<exact-value-from-blytz.work>;
--color-secondary-light: #<exact-value>;
--color-secondary-dark: #<exact-value>;

/* Accent Colors */
--color-accent: #<exact-value-from-blytz.work>;
--color-success: #<exact-value>;
--color-warning: #<exact-value>;
--color-error: #<exact-value>;
--color-info: #<exact-value>;

/* Neutral Colors */
--color-background: #<exact-value>;
--color-surface: #<exact-value>;
--color-border: #<exact-value>;
--color-text-primary: #<exact-value>;
--color-text-secondary: #<exact-value>;
--color-text-tertiary: #<exact-value>;
```

**Typography**

Preserve exact fonts and sizes:

```css
/* Font Families */
--font-sans: #<exact-value-from-blytz.work>;
--font-mono: #<exact-value>;

/* Font Sizes */
--text-xs: #<exact-value>;
--text-sm: #<exact-value>;
--text-base: #<exact-value>;
--text-lg: #<exact-value>;
--text-xl: #<exact-value>;
--text-2xl: #<exact-value>;
--text-3xl: #<exact-value>;

/* Font Weights */
--font-light: #<exact-value>;
--font-normal: #<exact-value>;
--font-medium: #<exact-value>;
--font-semibold: #<exact-value>;
--font-bold: #<exact-value>;
```

**Spacing**

Preserve exact spacing scale:

```css
--spacing-xs: #<exact-value>;
--spacing-sm: #<exact-value>;
--spacing-md: #<exact-value>;
--spacing-lg: #<exact-value>;
--spacing-xl: #<exact-value>;
--spacing-2xl: #<exact-value>;
--spacing-3xl: #<exact-value>;
```

**Border Radius**

Preserve exact border radius:

```css
--radius-sm: #<exact-value>;
--radius-md: #<exact-value>;
--radius-lg: #<exact-value>;
--radius-xl: #<exact-value>;
--radius-full: #<exact-value>;
```

**Shadows**

Preserve exact shadows:

```css
--shadow-sm: #<exact-value>;
--shadow-md: #<exact-value>;
--shadow-lg: #<exact-value>;
--shadow-xl: #<exact-value>;
```

---

### 5. Typography Styles

**Source**: `/home/gmsas95/blytz.work/frontend/src/styles/globals.css`

**Preserve**:
- Line heights
- Letter spacing
- Heading styles
- Body text styles
- Link styles
- Button text styles

---

### 6. Component-Specific Styles

**Button Styles**

Preserve:
- Primary button colors
- Secondary button colors
- Outline button styles
- Ghost button styles
- Hover states
- Active states
- Disabled states
- Loading states

**Form Styles**

Preserve:
- Input border colors
- Input focus states
- Input error states
- Label styles
- Helper text styles
- Validation error styles

**Card Styles**

Preserve:
- Card background colors
- Card border colors
- Card shadow
- Card hover effects
- Card padding

**Table Styles**

Preserve:
- Table border colors
- Table header background
- Table row hover
- Table cell padding
- Table text styles

---

## Layout Preservation

### 1. Container Widths

**Source**: Check `blytz.work` layout components

**Preserve**:
- Max-width values
- Container padding
- Responsive breakpoints
- Grid layouts
- Flex layouts

### 2. Navigation

**Preserve**:
- Header height
- Logo size
- Navigation links spacing
- Mobile menu breakpoint
- Dropdown menu styles

### 3. Footer

**Preserve**:
- Footer height
- Footer background
- Footer links spacing
- Copyright text styles

---

## Responsive Design

### 1. Breakpoints

**Source**: Check `blytz.work` Tailwind config

**Preserve**:
- Mobile breakpoint (exact value)
- Tablet breakpoint (exact value)
- Desktop breakpoint (exact value)
- Large desktop breakpoint (exact value)

### 2. Responsive Patterns

**Preserve**:
- Mobile-first approach
- Grid column changes
- Padding changes
- Font size scaling
- Component stacking

---

## Animation Preservation

### 1. Transitions

**Source**: Check `blytz.work` components

**Preserve**:
- Transition durations
- Transition timing functions
- Transition properties
- Hover animations

### 2. Keyframes

**Source**: Check `blytz.work` globals.css`

**Preserve**:
- Fade in/out animations
- Slide animations
- Pulse animations
- Spin animations

---

## Icon Preservation

### 1. Icon Library

**Source**: `/home/gmsas95/blytz.work/frontend/src/components`

**Current**: Lucide React

**Preserve**: Use exact same Lucide icons

**Action**: Import `lucide-react` and use same icon names

### 2. Icon Sizes

**Preserve**:
- Small icon size
- Medium icon size
- Large icon size
- Icon colors

---

## Copying Process

### Step 1: Copy Tailwind Config

```bash
# From blytz.work
cp /home/gmsas95/blytz.work/frontend/tailwind.config.ts \
   /home/gmsas95/t3.blytz.work/tailwind.config.ts
```

### Step 2: Copy Global Styles

```bash
# From blytz.work
cp /home/gmsas95/blytz.work/frontend/src/styles/globals.css \
   /home/gmsas95/t3.blytz.work/src/app/globals.css
```

### Step 3: Copy Components

```bash
# From blytz.work
cp -r /home/gmsas95/blytz.work/frontend/src/components/* \
      /home/gmsas95/t3.blytz.work/src/components/
```

### Step 4: Copy Public Assets

```bash
# From blytz.work
cp -r /home/gmsas95/blytz.work/frontend/public/images/* \
      /home/gmsas95/t3.blytz.work/public/images/
```

### Step 5: Copy Utility Functions

```bash
# From blytz.work
cp -r /home/gmsas95/blytz.work/frontend/src/lib/* \
      /home/gmsas95/t3.blytz.work/src/lib/
```

---

## Verification Checklist

### 1. Colors

- [ ] Primary color matches blytz.work
- [ ] Secondary color matches
- [ ] All accent colors match
- [ ] All neutral colors match
- [ ] Dark mode colors match (if applicable)

### 2. Typography

- [ ] Font families match
- [ ] Font sizes match
- [ ] Font weights match
- [ ] Line heights match
- [ ] Letter spacing matches

### 3. Spacing

- [ ] Component padding matches
- [ ] Component margins match
- [ ] Gap values match
- [ ] Container widths match
- [ ] Section spacing matches

### 4. Components

- [ ] Button styles match (all variants)
- [ ] Input styles match
- [ ] Card styles match
- [ ] Table styles match
- [ ] Dialog/Modal styles match
- [ ] Dropdown menu styles match
- [ ] Toast notification styles match

### 5. Layouts

- [ ] Header matches blytz.work
- [ ] Footer matches blytz.work
- [ ] Navigation matches blytz.work
- [ ] Dashboard layout matches
- [ ] Page layouts match

### 6. Responsive

- [ ] Mobile view matches
- [ ] Tablet view matches
- [ ] Desktop view matches
- [ ] Breakpoints match
- [ ] Responsive behaviors match

### 7. Animations

- [ ] Hover effects match
- [ ] Transitions match
- [ ] Loading states match
- [ ] Keyframe animations match

---

## Common Mistakes to Avoid

### 1. Not Copying Exact Values

**Mistake**: Using "similar" colors instead of exact hex values

**Solution**: Copy exact hex codes from blytz.work

### 2. Missing CSS Custom Properties

**Mistake**: Rewriting styles instead of using existing CSS variables

**Solution**: Keep all `--color-*`, `--spacing-*`, etc. variables

### 3. Changing Component Props

**Mistake**: Modifying component interfaces during migration

**Solution**: Keep exact same props and types

### 4. Not Testing Against Original

**Mistake**: Not comparing side-by-side with blytz.work

**Solution**: Have both apps open to compare visually

### 5. Not Checking Dark Mode

**Mistake**: Forgetting dark mode if blytz.work has it

**Solution**: Test both light and dark modes

---

## Design Audit Process

### 1. Side-by-Side Comparison

**Action**: Open blytz.work and t3.blytz.work in separate browser tabs

**Check**:
- [ ] Colors match exactly
- [ ] Typography matches exactly
- [ ] Spacing matches exactly
- [ ] Component styles match exactly

### 2. Cross-Browser Testing

**Action**: Test in Chrome, Firefox, Safari, Edge

**Check**:
- [ ] Design consistent across browsers
- [ ] No browser-specific styling issues

### 3. Device Testing

**Action**: Test on mobile, tablet, desktop

**Check**:
- [ ] Mobile view matches blytz.work
- [ ] Tablet view matches
- [ ] Desktop view matches

---

## Post-Migration Verification

### 1. Screenshot Comparison

**Action**: Take screenshots of blytz.work and t3.blytz.work pages

**Pages to Compare**:
- [ ] Homepage
- [ ] Login page
- [ ] Register page
- [ ] VA dashboard
- [ ] Employer dashboard
- [ ] VA onboarding
- [ ] Employer onboarding
- [ ] VA profile creation
- [ ] Employer discover page
- [ ] Pricing page

### 2. User Acceptance Testing

**Action**: Ask users (or yourself) to spot any visual differences

**Questions**:
- [ ] Do the colors look the same?
- [ ] Is the typography the same?
- [ ] Is the spacing the same?
- [ ] Are there any visual bugs?

---

## Troubleshooting

### Issue: Colors Don't Match

**Solution**:
1. Check Tailwind config is copied exactly
2. Check globals.css is copied exactly
3. Clear browser cache
4. Restart development server

### Issue: Fonts Don't Match

**Solution**:
1. Check font imports in layout.tsx
2. Check globals.css font declarations
3. Check Tailwind font config
4. Verify fonts are loaded in browser

### Issue: Component Styles Don't Match

**Solution**:
1. Verify component files are copied
2. Check component props are same
3. Check CSS classes are same
4. Check CSS custom properties are used

### Issue: Layout Issues

**Solution**:
1. Check max-widths in Tailwind config
2. Check container padding
3. Check responsive breakpoints
4. Check grid/flex layouts

---

## Design System Documentation

### Component Variants

Document all component variants from blytz.work:

#### Button Variants
- Primary
- Secondary
- Outline
- Ghost
- Disabled
- Loading

#### Input Variants
- Default
- Error
- Success
- Disabled

#### Card Variants
- Default
- Hover
- Interactive
- Static

---

## Success Criteria

Design is successfully preserved when:

- [ ] All colors match blytz.work exactly
- [ ] All typography matches blytz.work exactly
- [ ] All spacing matches blytz.work exactly
- [ ] All component styles match blytz.work exactly
- [ ] All layouts match blytz.work exactly
- [ ] All responsive behaviors match blytz.work exactly
- [ ] All animations match blytz.work exactly
- [ ] No visual differences detectable by users
- [ ] Cross-browser consistency achieved

---

**Last Updated**: January 1, 2026  
**Version**: 1.0  
**Status**: 📋 Ready for Phase 1
