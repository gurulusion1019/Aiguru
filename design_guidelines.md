# Design Guidelines: Salon Owner Lead Capture Wizard

## Design Approach: Hybrid - Utility with Aesthetic Appeal

**Rationale**: While this is a functional lead capture form requiring efficiency and high conversion, it targets salon owners (aesthetic-conscious professionals) arriving from Instagram/Facebook ads. The design must inspire trust and professionalism while maintaining seamless mobile usability.

**Primary References**: 
- **Airbnb's** clean, trustworthy form patterns for multi-step processes
- **Instagram's** mobile-first visual hierarchy and smooth transitions
- **Typeform's** engaging wizard UX with clear progress indication

---

## Core Design Elements

### A. Color Palette

**Light Mode (Primary)**
- Background: 0 0% 98% (soft warm white)
- Primary Brand: 280 65% 60% (sophisticated purple - beauty industry standard)
- Surface/Cards: 0 0% 100% (pure white)
- Text Primary: 240 10% 15% (near black with warmth)
- Text Secondary: 240 5% 50% (medium gray)
- Success: 150 60% 45% (professional green for OTP/validation)
- Border: 240 10% 90% (subtle dividers)

**Dark Mode** (for low-light environments)
- Background: 240 10% 10%
- Primary Brand: 280 60% 65%
- Surface/Cards: 240 8% 15%
- Text Primary: 0 0% 95%
- Text Secondary: 240 5% 65%

**Accent Colors**
- Error/Alert: 0 70% 55% (clear error indication)
- Upload Progress: 200 80% 50% (vibrant upload feedback)

### B. Typography

**Font Families** (via Google Fonts CDN):
- Primary: 'Inter' (400, 500, 600) - clean, professional, highly legible on mobile
- Headings: 'Plus Jakarta Sans' (600, 700) - modern, friendly for step titles

**Type Scale**:
- Step Headings: text-2xl font-semibold (Plus Jakarta Sans)
- Input Labels: text-sm font-medium 
- Body/Helper Text: text-sm
- Buttons: text-base font-medium
- Mini-tab Progress: text-xs font-medium

### C. Layout System

**Tailwind Spacing Primitives**: Use consistent units of **4, 6, 8, 12, 16, 20** for predictable rhythm
- Mobile padding: px-4 or px-6 (outer container)
- Section spacing: space-y-6 or space-y-8
- Component gaps: gap-4 for tight grouping, gap-6 for sections
- Sticky elements: top-0 and bottom-0 with appropriate safe-area padding

**Container Structure**:
- Max width: max-w-md mx-auto (optimal mobile reading/interaction width ~448px)
- Full-bleed for sticky chrome (progress bar, bottom CTA)
- Card-based content sections with rounded-xl and shadow-sm

### D. Component Library

#### Navigation & Progress
**Sticky Top Chrome** (bg-white/95 backdrop-blur-md with shadow):
- Mini-tabs showing 4 steps with active state (primary color underline, bold)
- Progress bar: h-1 gradient from primary to lighter shade, smooth width transitions
- Close (×) top-right, back arrow top-left (both touchable 44×44px minimum)
- Compact height (h-16) to maximize content area

**Bottom CTA Bar** (sticky bottom, elevated shadow-lg):
- Full-width gradient button (primary color) with rounded-t-2xl container
- Disabled state: opacity-40, cursor-not-allowed
- Safe area padding (pb-safe) for iOS devices
- Button: h-14 with clear label ("Next Step" / "Submit Application")

#### Form Elements
**Text Inputs**:
- Large touch targets: h-12 to h-14
- Rounded corners: rounded-lg
- Border: 2px solid border-color, focus:ring-2 ring-primary/20
- Clear labels above inputs with required asterisk (*)
- Helper text below in text-secondary color (text-xs)

**Phone Input** (+91 India):
- Flag icon prefix with +91 non-editable chip
- 10-digit input with tel keyboard on mobile
- Auto-format with spaces for readability (XXXXX XXXXX)

**OTP Verification**:
- 6-digit input boxes displayed as individual squares (grid grid-cols-6 gap-2)
- Large numbers, center-aligned in each box (w-12 h-14)
- Auto-focus next box on input, auto-submit when complete
- "Resend OTP" link with countdown timer (text-sm text-primary)

**URL Validation Feedback**:
- Google Maps link shows inline validation with checkmark icon
- Preview chip appears below when valid: soft background card with Google Maps pin icon, extracted business name if available

#### Upload Components

**Single Upload (Menu Card)**:
- Large drop zone: min-h-48, dashed border-2 when empty
- Two prominent buttons: "Upload Photo" (primary) and "Take Picture" (outline with camera icon)
- Live preview: rounded-lg with overlay controls (replace/remove icons on corners)
- Upload status: circular progress spinner → checkmark animation

**Multi-Upload Gallery**:
- Grid layout: grid-cols-3 gap-2 (mobile optimized for 3 across)
- Thumbnail previews: aspect-square with rounded-lg
- Drag handles for reorder (6-dot icon overlay on long-press)
- Count indicator: "3 / 8 photos uploaded" (text-sm, subtle badge style)
- Add more: dashed border card with + icon, same grid size
- Remove: X button top-right of each thumbnail (bg-red-500 text-white, small circle)

#### Review Cards
- Grouped sections with subtle borders (divide-y)
- Icons for each section (user, building, camera) using Heroicons
- Edit links as text-primary with chevron-right icon
- Thumbnails in 2-column grid for gallery preview
- All text read-only in text-secondary except headings

**Consent Checkbox**:
- Large custom checkbox (w-6 h-6) with primary accent
- Multi-line label with Privacy Policy as inline link (underline text-primary)
- Required state indicator

#### Success Screen
- Centered celebration icon (checkmark in circle, large scale)
- Heading: "Application Submitted!" (text-3xl)
- Subheading: Timeline info (text-secondary)
- Three action buttons stacked (gap-3):
  - Primary: "Add More Photos" (outline)
  - Secondary: "View Privacy Policy" (ghost)
  - Support: "WhatsApp Support" (green bg with WhatsApp icon from Font Awesome)

### E. Micro-interactions

**Use Sparingly**:
- Step transitions: Slide-in animation (transform-x) when advancing, slide-out when going back
- OTP success: Subtle scale + checkmark animation
- Upload complete: Progress circle → checkmark morph
- Form validation: Shake animation on error (animate-wiggle)
- Button press: Subtle scale down (active:scale-98)

**No Animations For**:
- Form input focus states (instant)
- Text changes
- Static content

---

## Mobile-First Specifications

**Touch Targets**: Minimum 44×44px for all interactive elements
**Input Focus**: Large, visible focus rings (ring-2 ring-primary/30)
**Keyboard Handling**: Appropriate input types (tel, url, text) for optimal mobile keyboards
**Safe Areas**: Respect iOS notch/home indicator with safe-area-inset padding
**Offline UX**: Clear indicators when connection lost, queued uploads shown with retry option

---

## Visual Hierarchy Principles

1. **Progressive Disclosure**: Show one focused task per step, hide complexity
2. **Clear Affordances**: Buttons and inputs immediately recognizable as interactive
3. **Validation Feedback**: Immediate inline validation (green checkmarks, red errors)
4. **Trust Signals**: Professional typography, clean spacing, secure padlock icons where relevant
5. **Momentum**: Progress bar and step indicators maintain user motivation

---

## Images

**No hero images required** - This is a utility-focused wizard form. However:

**Icon Usage**: 
- Step icons in mini-tabs (user, building, camera, checkmark) - use Heroicons outline style
- Input prefix icons (phone, link, location) - subtle, text-secondary color
- Upload placeholder: Large camera icon (from Heroicons) in empty drop zone
- Success screen: Celebration icon (checkmark-circle, ~80px, primary color)

**User-Generated Content**:
- Uploaded images are the primary visual content
- Display with care: proper aspect ratios, loading states, compression indicators

This design creates a professional, trustworthy experience that converts Instagram/Facebook traffic into qualified leads while maintaining the efficiency needed for a mobile lead capture flow.