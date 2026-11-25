# Implementation Plan: GitHub Pages Personal Website

## Overview

This implementation plan breaks down the creation of a single-page personal portfolio website featuring a dark SRE console aesthetic. The site will be built using only HTML5, CSS3, and vanilla JavaScript—no frameworks or build tools required. The implementation follows a modular approach, starting with core structure and styling, then adding interactive components (navigation, filtering, CLI), animations (boot sequence, scroll reveal), and finishing with responsive design and deployment setup.

## Tasks

- [x] 1. Create project structure and HTML foundation
  - Create index.html with semantic HTML5 structure
  - Add viewport meta tags and charset configuration
  - Define all section containers (hero, projects, evolution, architecture, cli, footer)
  - Implement skip-to-content link for accessibility
  - Add SVG asset reference with proper relative path (./assets/svg/Architecture-Diagram-v4.2.svg)
  - _Requirements: 1.1, 1.3, 1.4, 1.5, 15.1, 15.7_

- [x] 2. Implement CSS theme system and global styles
  - [x] 2.1 Define CSS custom properties for dark theme palette
    - Create :root variables for all colors (#0B0F14, #111827, #1F2937, #06B6D4, #10B981)
    - Define typography system with monospace font stack
    - Set up responsive font sizing using clamp()
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5, 2.6_
  
  - [x] 2.2 Implement grid background pattern
    - Create fixed background using repeating-linear-gradient
    - Apply subtle cyan accent color at 3% opacity
    - Use ::before pseudo-element on body
    - _Requirements: 2.7_
  
  - [x] 2.3 Create CSS reset and base styles
    - Write box-sizing reset for all elements
    - Set body background color and default text color
    - Define base typography and line-height
    - _Requirements: 2.1, 2.6_

- [ ] 3. Build navigation bar component
  - [x] 3.1 Create fixed navigation HTML structure
    - Build navbar with logo, menu items, and mobile hamburger button
    - Add navigation links for all four sections (hero, projects, evolution, cli)
    - Include ARIA labels for accessibility
    - _Requirements: 3.1, 3.2, 15.2, 15.3_
  
  - [x] 3.2 Style navigation bar with dark theme
    - Apply fixed positioning with z-index: 1000
    - Create flexbox layout for desktop view
    - Style active state with accent color (#06B6D4)
    - Add smooth transitions for hover and active states
    - _Requirements: 3.1, 3.4, 15.4_
  
  - [-] 3.3 Implement responsive mobile menu
    - Create hamburger button styles with CSS
    - Add media query to hide menu and show hamburger below 768px
    - Implement mobile menu toggle with JavaScript
    - _Requirements: 3.5, 12.4_

- [ ] 4. Implement hero terminal section structure
  - [x] 4.1 Create hero HTML with terminal window design
    - Build terminal window with header buttons (close, minimize, maximize)
    - Add boot-sequence container for animated output
    - Create hero-content container for static display
    - Set full viewport height layout
    - _Requirements: 4.1, 4.5, 4.7_
  
  - [x] 4.2 Style terminal with dark console aesthetic
    - Apply terminal border and background colors
    - Create terminal header with colored buttons
    - Style monospace font and terminal prompt
    - Center content using flexbox
    - _Requirements: 2.6, 4.7_
  
  - [-] 4.3 Create blinking cursor animation
    - Write @keyframes for cursor blink effect
    - Apply animation to cursor element
    - Set appropriate timing (1s interval)
    - _Requirements: 4.2, 4.3, 4.4_

- [ ] 5. Implement boot sequence animation
  - [~] 5.1 Write boot sequence JavaScript module
    - Create bootSequence object with module pattern
    - Define three boot lines (name, role, slogan)
    - Implement character-by-character typewriter function
    - Add state management to run only once
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.6_
  
  - [~] 5.2 Initialize boot sequence on page load
    - Attach to DOMContentLoaded event
    - Execute boot sequence automatically
    - Remove boot-sequence element after completion
    - Display static hero-content after animation
    - _Requirements: 4.1, 4.5, 4.6_

- [ ] 6. Create project portfolio section
  - [~] 6.1 Build project data structure
    - Define projects array with 10 project objects
    - Include name, description, category, and slug for each project
    - Assign each project to exactly one of 5 domain categories
    - Use slug to generate GitHub URLs (https://github.com/Junwu0615/[slug])
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5_
  
  - [~] 6.2 Create filter bar HTML and styles
    - Build filter buttons for 5 categories plus "All" option
    - Add data-filter attributes to buttons
    - Style active filter button with accent color
    - Position above project grid
    - _Requirements: 6.1, 6.2, 6.5, 6.7_
  
  - [~] 6.3 Implement project card HTML structure
    - Create article elements with card-header and card-body
    - Display project name, category badge, and description
    - Add GitHub link with external icon and aria-label
    - Include data-category attribute for filtering
    - _Requirements: 5.6, 15.3_
  
  - [~] 6.4 Style project cards with grid layout
    - Create CSS Grid with auto-fit and minmax(300px, 1fr)
    - Apply card background (#111827) and border (#1F2937)
    - Add hover glow effect using box-shadow
    - Implement smooth transitions for hover states
    - _Requirements: 5.7, 12.3_

- [ ] 7. Implement project filtering system
  - [~] 7.1 Write filter JavaScript module
    - Create filterSystem object with module pattern
    - Implement filter logic to show/hide cards by category
    - Add setActiveButton function to update UI state
    - Track activeFilter in module state
    - _Requirements: 6.2, 6.3, 6.4, 6.5, 6.6_
  
  - [~] 7.2 Attach filter event listeners
    - Add click handlers to all filter buttons
    - Update active button styling on click
    - Trigger filter function without page reload
    - _Requirements: 6.3, 6.6_

- [ ] 8. Build evolution timeline section
  - [~] 8.1 Create timeline HTML structure
    - Build 3 timeline stage articles
    - Add stage markers with numbered badges
    - Include stage title and description in each article
    - Add data-stage attributes for animation
    - _Requirements: 7.1, 7.2, 7.3_
  
  - [~] 8.2 Style timeline with responsive layouts
    - Create horizontal flexbox layout for desktop
    - Add connecting line using ::before pseudo-element
    - Implement vertical layout for mobile (<768px)
    - Style stage markers as circular badges
    - _Requirements: 7.6, 7.7_
  
  - [~] 8.3 Prepare timeline for scroll reveal
    - Set initial opacity: 0 and translateY(30px)
    - Define .revealed class with opacity: 1 and translateY(0)
    - Add smooth transition timing (0.6s ease)
    - _Requirements: 7.4, 7.5, 11.4, 11.6_

- [ ] 9. Implement scroll reveal animation system
  - [~] 9.1 Create scroll reveal JavaScript module
    - Build scrollReveal object with IntersectionObserver
    - Set threshold: 0.2 and appropriate rootMargin
    - Implement callback to add .revealed class
    - Unobserve elements after animation (run once)
    - _Requirements: 7.4, 7.5, 11.1, 11.2, 11.3, 11.5_
  
  - [~] 9.2 Apply scroll reveal to multiple elements
    - Target timeline stages for observation
    - Target project cards for observation
    - Target section headers for observation
    - Add staggered delay using data-delay attributes
    - _Requirements: 11.1, 11.2, 11.4_

- [ ] 10. Create architecture diagram section
  - [~] 10.1 Embed SVG with responsive container
    - Create section with diagram-container div
    - Add img element with src="./assets/svg/Architecture-Diagram-v4.2.svg"
    - Set width: 100% and height: auto
    - Add loading="lazy" for performance optimization
    - Include descriptive alt text for accessibility
    - _Requirements: 8.1, 8.2, 8.5, 13.3_
  
  - [~] 10.2 Apply CSS filters for dark theme adaptation
    - Add filter: invert(0.9) brightness(1.2) hue-rotate(180deg)
    - Ensure crisp rendering with image-rendering property
    - Add smooth transition for filter changes
    - Test visual consistency with dark theme palette
    - _Requirements: 8.3, 8.4, 8.6_

- [ ] 11. Build mini-CLI component
  - [~] 11.1 Create CLI HTML structure
    - Build terminal container with output area and input line
    - Add scrollable CLI output div with max-height
    - Create input line with prompt and text input field
    - Include aria-label and autocomplete="off" on input
    - _Requirements: 9.1, 9.11, 15.2_
  
  - [~] 11.2 Style CLI with terminal aesthetics
    - Apply monospace font and terminal background (#0B0F14)
    - Style prompt with accent color
    - Create custom scrollbar for dark theme
    - Define color-coded output classes (success, error, info)
    - _Requirements: 9.9, 2.4, 2.5_
  
  - [~] 11.3 Implement CLI command interpreter
    - Create miniCLI object with commands dictionary
    - Implement whoami command with ASCII art output
    - Implement stack command with formatted technology table
    - Implement links command with external platform URLs
    - Implement help command listing all available commands
    - Implement clear command to reset output area
    - _Requirements: 9.2, 9.3, 9.4, 9.5, 9.6, 9.7_
  
  - [~] 11.4 Add CLI input handling and history
    - Listen for Enter key to execute commands
    - Display entered command in output area
    - Execute command and display result with color coding
    - Show error message for unrecognized commands
    - Auto-scroll output area to bottom after each command
    - _Requirements: 9.8, 9.9, 9.10, 9.11_

- [ ] 12. Create footer with external links
  - [~] 12.1 Build footer HTML structure
    - Create footer element with social-links container
    - Add links to GitHub, LinkedIn, Cake, and Analytics platforms
    - Include icon graphics for each platform
    - Add descriptive aria-labels to all links
    - Set target="_blank" and rel="noopener noreferrer"
    - _Requirements: 10.1, 10.2, 10.3, 10.4, 15.3_
  
  - [~] 12.2 Style footer with theme consistency
    - Apply flexbox layout with center alignment
    - Style links with hover effects using accent colors
    - Add transitions to icons and text
    - Include copyright text with current year
    - _Requirements: 10.5, 10.6_

- [ ] 13. Implement navigation system behaviors
  - [~] 13.1 Add smooth scroll functionality
    - Create navigationSystem JavaScript module
    - Add click event listeners to all nav links
    - Implement scrollIntoView with smooth behavior
    - Prevent default anchor link behavior
    - _Requirements: 3.3_
  
  - [~] 13.2 Implement active section detection
    - Use IntersectionObserver to monitor section visibility
    - Set appropriate threshold and rootMargin for detection
    - Update .active class on corresponding nav link
    - Remove .active from previous link
    - _Requirements: 3.4, 15.5, 15.6_

- [ ] 14. Implement responsive design breakpoints
  - [~] 14.1 Add mobile-first media queries
    - Create breakpoints at 480px, 768px, 1024px, 1280px, 1920px
    - Adjust project grid columns: 1 column (<768px), 2 columns (768-1024px), 3+ columns (>1024px)
    - Toggle navigation menu display based on viewport width
    - Switch timeline orientation: vertical (<768px), horizontal (≥768px)
    - _Requirements: 12.1, 12.2, 12.3, 12.4, 12.5_
  
  - [~] 14.2 Ensure mobile touch targets
    - Set minimum 44x44 pixel size for all interactive elements
    - Adjust padding on buttons and links for mobile
    - Test tap targets on actual mobile devices
    - _Requirements: 12.7_
  
  - [~] 14.3 Optimize typography for all screen sizes
    - Use clamp() function for responsive font sizing
    - Ensure text remains readable at 320px width
    - Test line-height and letter-spacing at different viewports
    - _Requirements: 12.6_

- [ ] 15. Add CSS animations and transitions
  - [~] 15.1 Define keyframe animations
    - Create @keyframes blink for cursor animation
    - Create @keyframes fadeInUp for scroll reveal
    - Create @keyframes glow for hover effects
    - Set appropriate duration and easing functions
    - _Requirements: 11.4, 11.6_
  
  - [~] 15.2 Apply transition standards
    - Define utility classes for fast (0.2s), normal (0.3s), slow (0.6s) transitions
    - Apply transitions to interactive elements (buttons, links, cards)
    - Add transitions to filter animations
    - _Requirements: 11.4, 11.6_

- [ ] 16. Optimize performance and loading
  - [~] 16.1 Inline critical CSS and JavaScript
    - Embed all CSS in <style> block within <head>
    - Embed all JavaScript in <script> block before </body>
    - Minify inline styles and scripts
    - _Requirements: 13.1, 13.2_
  
  - [~] 16.2 Optimize asset loading
    - Add loading="lazy" to SVG image
    - Use decoding="async" for architecture diagram
    - Defer non-critical animations until DOMContentLoaded
    - _Requirements: 13.3, 13.4_
  
  - [~] 16.3 Implement deferred initialization
    - Run boot sequence on DOMContentLoaded
    - Delay other module initialization until boot completes
    - Use requestIdleCallback for non-critical features
    - _Requirements: 13.2, 13.5_

- [ ] 17. Ensure browser compatibility
  - [~] 17.1 Add vendor prefixes and fallbacks
    - Include -webkit-, -moz- prefixes for CSS properties
    - Provide fallback styles for CSS Grid
    - Provide fallback styles for flexbox
    - Test across Chrome, Firefox, Safari, Edge
    - _Requirements: 14.1, 14.2, 14.3, 14.4, 14.5, 14.6_

- [ ] 18. Implement accessibility features
  - [~] 18.1 Enhance semantic HTML
    - Verify all sections use proper HTML5 elements
    - Ensure heading hierarchy is logical (h1 → h2 → h3)
    - Add ARIA landmarks where appropriate
    - _Requirements: 15.1, 15.2_
  
  - [~] 18.2 Ensure keyboard navigation
    - Test Tab key navigation through all interactive elements
    - Verify focus indicators are visible
    - Ensure logical tab order
    - Test Enter key on buttons and links
    - _Requirements: 15.5, 15.6_
  
  - [~] 18.3 Verify color contrast compliance
    - Check all text against backgrounds for 4.5:1 ratio minimum
    - Test accent colors against dark backgrounds
    - Use browser DevTools or online contrast checkers
    - _Requirements: 15.4_

- [ ] 19. Final integration and testing
  - [~] 19.1 Wire all JavaScript modules together
    - Initialize all modules in proper sequence
    - Ensure boot sequence completes before other features activate
    - Verify no JavaScript errors in browser console
    - Test all interactive features function correctly
    - _Requirements: 4.6, 13.2_
  
  - [~] 19.2 Cross-browser testing
    - Test full functionality on Chrome 90+
    - Test full functionality on Firefox 88+
    - Test full functionality on Safari 14+
    - Test full functionality on Edge 90+
    - _Requirements: 14.1, 14.2, 14.3, 14.4_
  
  - [~] 19.3 Responsive testing
    - Test at 320px, 480px, 768px, 1024px, 1280px, 1920px, 2560px widths
    - Verify all sections display correctly at each breakpoint
    - Test touch interactions on mobile devices
    - _Requirements: 12.1, 12.2, 12.3, 12.4, 12.5, 12.7_
  
  - [~] 19.4 Performance validation
    - Measure page load time on 5 Mbps connection
    - Verify First Contentful Paint < 1 second
    - Run Lighthouse audit and achieve score >90
    - Check total page weight < 150KB
    - _Requirements: 13.1, 13.5_

- [~] 20. Checkpoint - Final review and user validation
  - Ensure all tests pass, review all implemented features, ask the user if questions arise.

- [ ] 21. GitHub Pages deployment setup
  - [~] 21.1 Verify repository configuration
    - Confirm index.html is in repository root
    - Verify assets/svg/ directory structure exists
    - Ensure Architecture-Diagram-v4.2.svg is committed
    - Check .gitignore excludes unnecessary files
    - _Requirements: 1.3, 1.4_
  
  - [~] 21.2 Configure GitHub Pages settings
    - Enable GitHub Pages in repository settings
    - Set source to main branch root directory
    - Verify custom domain configuration (if applicable)
    - Test deployment at github.io URL
    - _Requirements: 1.3_
  
  - [~] 21.3 Final deployment verification
    - Visit deployed site and test all functionality
    - Verify all assets load correctly (no 404 errors)
    - Test external links open in new tabs
    - Confirm responsive design works on live site
    - _Requirements: 1.3, 1.4, 10.3_

## Notes

- All tasks build on HTML5, CSS3, and vanilla JavaScript—no frameworks or build tools required
- Each task references specific requirements from requirements.md for traceability
- The implementation follows a modular approach: structure → styling → interactivity → animations → optimization
- Testing is integrated throughout via checkpoints rather than optional sub-tasks
- Boot sequence must complete before other JavaScript modules initialize
- Responsive design is mobile-first with progressive enhancement for larger screens
- Accessibility is built-in from the start with semantic HTML and ARIA labels
- Performance optimization is achieved through inline critical resources and lazy loading
- GitHub Pages deployment requires only committing index.html and assets to the repository

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["1"] },
    { "id": 1, "tasks": ["2.1", "2.2", "2.3"] },
    { "id": 2, "tasks": ["3.1", "4.1"] },
    { "id": 3, "tasks": ["3.2", "4.2"] },
    { "id": 4, "tasks": ["3.3", "4.3"] },
    { "id": 5, "tasks": ["5.1"] },
    { "id": 6, "tasks": ["5.2"] },
    { "id": 7, "tasks": ["6.1", "8.1", "11.1"] },
    { "id": 8, "tasks": ["6.2", "6.3", "8.2", "11.2"] },
    { "id": 9, "tasks": ["6.4", "8.3", "11.3"] },
    { "id": 10, "tasks": ["7.1"] },
    { "id": 11, "tasks": ["7.2", "11.4"] },
    { "id": 12, "tasks": ["9.1"] },
    { "id": 13, "tasks": ["9.2"] },
    { "id": 14, "tasks": ["10.1"] },
    { "id": 15, "tasks": ["10.2"] },
    { "id": 16, "tasks": ["12.1"] },
    { "id": 17, "tasks": ["12.2"] },
    { "id": 18, "tasks": ["13.1"] },
    { "id": 19, "tasks": ["13.2"] },
    { "id": 20, "tasks": ["14.1", "15.1"] },
    { "id": 21, "tasks": ["14.2", "14.3", "15.2"] },
    { "id": 22, "tasks": ["16.1", "16.2"] },
    { "id": 23, "tasks": ["16.3"] },
    { "id": 24, "tasks": ["17.1"] },
    { "id": 25, "tasks": ["18.1", "18.2", "18.3"] },
    { "id": 26, "tasks": ["19.1"] },
    { "id": 27, "tasks": ["19.2", "19.3", "19.4"] },
    { "id": 28, "tasks": ["20"] },
    { "id": 29, "tasks": ["21.1"] },
    { "id": 30, "tasks": ["21.2"] },
    { "id": 31, "tasks": ["21.3"] }
  ]
}
```
