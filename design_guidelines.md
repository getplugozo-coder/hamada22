# Hamada TV - Football Live Streaming App Design Guidelines

## Design Approach
**Hybrid Approach**: Drawing inspiration from leading sports streaming platforms (ESPN+, DAZN, FuboTV) while leveraging Material Design principles for mobile-optimized components. The design prioritizes content visibility, quick access to live streams, and touch-friendly interactions.

**Core Design Principles**:
- Content-first: Video and match information take visual priority
- Dark-optimized: Interface designed for video viewing contexts
- Touch-native: All controls sized for mobile interaction (minimum 44px tap targets)
- Immediate access: Maximum 2 taps to any live stream

## Color Palette

**Dark Mode Primary** (App Default):
- Background Base: 220 15% 8%
- Surface: 220 12% 12%
- Surface Elevated: 220 10% 16%
- Primary Brand: 142 76% 45% (vibrant green for "Live" indicators and CTAs)
- Accent: 0 84% 60% (red for live badges and urgent actions)
- Text Primary: 0 0% 95%
- Text Secondary: 0 0% 70%

**Light Mode** (Optional toggle in settings):
- Background: 0 0% 98%
- Surface: 0 0% 100%
- Primary: 142 70% 40%
- Text: 220 15% 20%

## Typography

**Font Stack**: 
- Primary: 'Inter' via Google Fonts (400, 500, 600, 700 weights)
- Fallback: -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui

**Type Scale**:
- Hero/Page Title: text-2xl md:text-3xl font-bold (match titles, page headers)
- Section Headers: text-lg md:text-xl font-semibold
- Body/Match Info: text-sm md:text-base font-medium
- Labels/Meta: text-xs md:text-sm font-normal
- Live Badge: text-xs font-bold uppercase tracking-wide

## Layout System

**Spacing Primitives**: Use Tailwind units of 2, 4, 6, 8, 12, 16 for consistency
- Component padding: p-4 or p-6
- Section gaps: gap-4 or gap-6
- Screen margins: px-4 md:px-6
- Bottom navigation height: h-16

**Container Strategy**:
- Full-width sections with max-w-7xl mx-auto px-4 for larger screens
- Mobile-first: Base styles for mobile, breakpoints for md/lg tablets
- Video player: Full-width containers, 16:9 aspect ratio preserved

## Component Library

### Navigation
**Bottom Navigation Bar** (Primary):
- Fixed position at bottom (safe-area-inset-bottom support)
- 5 icons: Home, Channels, Admin (auth-gated), About, More
- Active state: Primary green with filled icon variant
- Icon size: w-6 h-6 with text-xs label below
- Inactive: Text secondary color with outline icons

### Match Schedule Cards
**List Item Design**:
- Card background: Surface elevated with subtle border
- Layout: Two-column split (Team info left | Time/Watch button right)
- Live indicator: Pulsing red badge with "LIVE" text in top-right
- Team names: font-semibold with team logos (24px circles)
- Date/Time: text-secondary in text-xs above teams
- Watch button: Primary green, rounded-full, w-20 h-9, "Watch" text

### Channel Cards
**Grid Layout** (2 columns on mobile, 3-4 on tablet):
- Card: Aspect ratio 16:9 or 1:1 with channel logo centered
- Channel name: Below logo, text-center, font-medium
- Hover/Active state: Scale-105 transform with shadow-lg
- Watch button: Overlay on card, appears on tap/hover

### Video Player
**Full-featured M3U8 Player**:
- Container: w-full with aspect-video (16:9 ratio)
- Controls overlay: Gradient backdrop from bottom (transparent to black)
- Play/Pause: Center icon (w-16 h-16) with tap area
- Progress bar: Full-width at bottom, primary green fill
- Volume: Slider on right side (vertical for mobile)
- Fullscreen: Icon in top-right of controls
- Loading state: Spinner with primary color
- Controls auto-hide after 3s of inactivity

### Admin Panel
**Dashboard Layout**:
- Tab navigation: Matches | Channels | Ads (sticky top bar)
- Data tables: Responsive cards on mobile, table on tablet+
- Action buttons: Icon buttons (edit/delete) in accent red for delete
- Add new: FAB (Floating Action Button) in bottom-right, primary green
- Forms: Full-screen modal on mobile, side panel on tablet

### Advertisement Placeholders
**Strategic Placement**:
- Home page top: Banner ad (320x50 standard mobile banner)
- Under video player: 300x250 medium rectangle in center
- Channel list: Native ad cards every 6 items, marked "Sponsored"
- Background: Surface color with dashed border and "Ad Placeholder" text

### Static Pages (About/Contact/Privacy)
**Content Layout**:
- Hero: Small 30vh with gradient overlay and page title
- Content: max-w-3xl mx-auto with generous line-height (1.7)
- Sections: Separated by border-t borders with py-8 spacing
- Contact form: Stacked inputs, primary button full-width
- Privacy: Collapsible sections with accordion pattern

### Loading & Empty States
**Skeleton Loaders**: Animated gradient shimmer in surface color
**Empty States**: 
- Icon (w-24 h-24, text-secondary opacity-50)
- Message: text-lg font-medium
- Action button: Primary CTA if applicable

### Authentication (Admin)
**Login Screen**:
- Centered card (max-w-md) with app logo at top
- Email/Password inputs with visible password toggle
- Remember me checkbox below inputs
- Primary login button, full-width

## Images

**Channel Logos**: 
- Size: 80x80px minimum, circular crop
- Format: PNG with transparent background
- Placement: Center of channel cards and in match schedule (16px circles)

**Match Team Logos**:
- Size: 32x32px circles
- Placement: Left of team names in schedule

**Background Patterns** (Optional):
- Subtle football field texture at 5% opacity on surface backgrounds
- Gradient overlays on video player when paused

## Interaction Patterns

**Tap Feedback**: Ripple effect (Material Design) on all interactive elements in primary color at 20% opacity
**Swipe Gestures**: Horizontal swipe on match cards to reveal quick actions (edit/delete in admin)
**Pull to Refresh**: On home and channels pages with loading spinner
**Transitions**: 200ms ease-in-out for all state changes, 300ms for page transitions

## Critical Mobile Optimizations

- Video player aspect ratio locked with black letterboxing
- Safe area insets for notch devices (padding-top for status bar)
- Landscape mode: Full-screen video with minimal controls
- Offline indicator: Banner at top when Firebase connection lost
- Touch target minimum: 44x44px for all buttons and interactive elements