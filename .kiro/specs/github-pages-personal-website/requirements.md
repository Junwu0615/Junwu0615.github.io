# Requirements Document

## Introduction

This document specifies the requirements for a single-page personal portfolio website deployed on GitHub Pages. The website showcases the professional profile of Ping Chun (Junwu0615), a Data Engineer transitioning to Platform Engineering, with a dark SRE console aesthetic. The site features four major sections: a hero terminal with navigation, a filterable project portfolio (10 sub-projects across 5 categories), a career evolution timeline, and an interactive mini-CLI with footer.

The website is a pure static site built with HTML5, CSS3, and vanilla JavaScript, requiring no build tools or frameworks. It emphasizes terminal-style animations, scroll-based reveal effects, and interactive elements that reflect a systems engineering aesthetic.

## Glossary

- **Website**: The complete single-page application containing all sections, styles, scripts, and assets
- **Hero_Terminal**: The top section displaying animated typewriter boot sequence with personal information
- **Navigation_Bar**: The fixed top navigation menu providing section links
- **Project_Portfolio**: The section displaying 10 sub-projects with domain-based filtering capabilities
- **Filter_System**: The interactive control allowing users to filter projects by 5 domain categories
- **Evolution_Timeline**: The section displaying 3 career stages with descriptions
- **Mini_CLI**: The interactive command-line interface accepting user input and returning formatted output
- **Architecture_Diagram**: The SVG graphic located at ./assets/svg/Architecture-Diagram-v4.2.svg
- **Boot_Sequence**: The typewriter animation displaying name, role, and slogan character-by-character
- **Scroll_Reveal**: Progressive animation effect triggered when elements enter the viewport
- **Project_Card**: Individual card component representing one sub-project with metadata
- **External_Link**: Hyperlink to external platforms (GitHub, LinkedIn, Cake, Analytics)
- **Dark_Theme_Palette**: Color scheme using #0B0F14, #111827, #1F2937, #06B6D4, #10B981
- **GitHub_Repository_URL**: Standard GitHub URL pattern https://github.com/Junwu0615/[project-name]
- **Domain_Category**: One of five project classification types (Data Engineering, MLOps, Platform Engineering, Analytics, DevOps)
- **CLI_Command**: User input accepted by Mini_CLI (whoami, stack, links, help, clear)
- **Viewport**: The visible portion of the webpage in the user's browser window

## Requirements

### Requirement 1: Static Website Structure

**User Story:** As a visitor, I want to access a fully functional single-page website, so that I can view the portfolio content without requiring server-side processing or build tools.

#### Acceptance Criteria

1. THE Website SHALL be implemented using only HTML5, CSS3, and vanilla JavaScript
2. THE Website SHALL NOT require build tools, package managers, or JavaScript frameworks
3. THE Website SHALL be deployable directly to GitHub Pages without compilation steps
4. THE Website SHALL load all resources from relative paths within the repository
5. THE Website SHALL contain exactly one HTML file as the entry point (index.html)

### Requirement 2: Visual Theme and Styling

**User Story:** As a visitor, I want to experience a dark SRE console aesthetic, so that the website reflects the platform engineering focus.

#### Acceptance Criteria

1. THE Website SHALL apply the Dark_Theme_Palette as the primary color scheme
2. THE Website SHALL use #0B0F14 as the primary background color
3. THE Website SHALL use #111827 and #1F2937 as secondary background colors for sections
4. THE Website SHALL use #06B6D4 as the primary accent color for highlights and links
5. THE Website SHALL use #10B981 as the secondary accent color for success states and active elements
6. THE Website SHALL apply terminal-style monospace fonts for text content
7. THE Website SHALL display a subtle grid background pattern across all sections

### Requirement 3: Navigation Bar

**User Story:** As a visitor, I want to navigate between sections using a fixed menu, so that I can quickly access different parts of the portfolio.

#### Acceptance Criteria

1. THE Navigation_Bar SHALL be fixed at the top of the viewport during scrolling
2. THE Navigation_Bar SHALL contain links to all four major sections
3. WHEN a navigation link is clicked, THE Website SHALL smoothly scroll to the corresponding section
4. THE Navigation_Bar SHALL highlight the active section based on scroll position
5. THE Navigation_Bar SHALL remain visible and accessible at all viewport widths

### Requirement 4: Hero Terminal and Boot Sequence

**User Story:** As a visitor, I want to see an animated terminal boot sequence when I first load the page, so that I experience an engaging introduction to the portfolio owner.

#### Acceptance Criteria

1. WHEN the Website loads, THE Hero_Terminal SHALL initiate the Boot_Sequence automatically
2. THE Boot_Sequence SHALL display "Ping Chun (Junwu0615)" character-by-character using typewriter animation
3. THE Boot_Sequence SHALL display "Data Engineer → Platform Engineering" character-by-character after the name completes
4. THE Boot_Sequence SHALL display "Platform Engineering Through Evolution, Not Accumulation" character-by-character after the role completes
5. THE Hero_Terminal SHALL complete the full Boot_Sequence before displaying static content
6. THE Boot_Sequence SHALL execute only once per page load
7. THE Hero_Terminal SHALL occupy the full viewport height on initial load

### Requirement 5: Project Portfolio Structure

**User Story:** As a visitor, I want to browse 10 sub-projects organized by domain categories, so that I can explore relevant work based on my interests.

#### Acceptance Criteria

1. THE Project_Portfolio SHALL display exactly 10 Project_Cards
2. THE Project_Portfolio SHALL organize projects into 5 Domain_Categories: Data Engineering, MLOps, Platform Engineering, Analytics, DevOps
3. THE Project_Portfolio SHALL assign each project to exactly one Domain_Category
4. WHEN a Project_Card is clicked, THE Website SHALL navigate to the corresponding GitHub_Repository_URL
5. THE Project_Portfolio SHALL generate GitHub_Repository_URLs using the pattern https://github.com/Junwu0615/[project-name]
6. THE Project_Card SHALL display project name, description, and assigned Domain_Category
7. THE Project_Portfolio SHALL apply hover glow effects to Project_Cards

### Requirement 6: Project Filtering System

**User Story:** As a visitor, I want to filter projects by domain category, so that I can focus on specific areas of expertise.

#### Acceptance Criteria

1. THE Filter_System SHALL provide exactly 5 filter buttons corresponding to Domain_Categories
2. THE Filter_System SHALL provide an "All" filter button to display all projects
3. WHEN a filter button is clicked, THE Project_Portfolio SHALL display only Project_Cards matching the selected Domain_Category
4. WHEN the "All" filter button is clicked, THE Project_Portfolio SHALL display all 10 Project_Cards
5. THE Filter_System SHALL visually indicate the currently active filter
6. THE Filter_System SHALL update the displayed projects without page reload
7. THE Filter_System SHALL be positioned above the Project_Portfolio

### Requirement 7: Evolution Timeline

**User Story:** As a visitor, I want to understand the career progression from Data Engineering to Platform Engineering, so that I can see the professional evolution narrative.

#### Acceptance Criteria

1. THE Evolution_Timeline SHALL display exactly 3 career stages
2. THE Evolution_Timeline SHALL present stages in chronological order
3. THE Evolution_Timeline SHALL provide a title and description for each stage
4. THE Evolution_Timeline SHALL apply Scroll_Reveal animations to each stage
5. WHEN a stage element enters the Viewport, THE Website SHALL animate the stage into visibility
6. THE Evolution_Timeline SHALL use vertical timeline layout on mobile devices
7. THE Evolution_Timeline SHALL use horizontal timeline layout on desktop devices

### Requirement 8: Architecture Diagram Display

**User Story:** As a visitor, I want to view an architecture diagram that matches the dark theme, so that I can understand technical system designs without visual inconsistency.

#### Acceptance Criteria

1. THE Website SHALL embed the Architecture_Diagram inline within a designated section
2. THE Website SHALL load the Architecture_Diagram from ./assets/svg/Architecture-Diagram-v4.2.svg
3. THE Website SHALL apply CSS filter properties to the Architecture_Diagram for dark theme adaptation
4. THE Website SHALL use invert and brightness CSS filters to adapt SVG colors to the Dark_Theme_Palette
5. THE Architecture_Diagram SHALL be responsive and scale to fit different viewport widths
6. THE Architecture_Diagram SHALL maintain aspect ratio during responsive scaling

### Requirement 9: Interactive Mini-CLI

**User Story:** As a visitor, I want to interact with a mini command-line interface, so that I can explore information through terminal-style commands.

#### Acceptance Criteria

1. THE Mini_CLI SHALL accept user text input via a command prompt
2. THE Mini_CLI SHALL recognize the following CLI_Commands: whoami, stack, links, help, clear
3. WHEN "whoami" is entered, THE Mini_CLI SHALL display formatted personal information with ASCII art
4. WHEN "stack" is entered, THE Mini_CLI SHALL display a structured table of technical skills and tools
5. WHEN "links" is entered, THE Mini_CLI SHALL display formatted External_Links with descriptions
6. WHEN "help" is entered, THE Mini_CLI SHALL display available CLI_Commands with usage descriptions
7. WHEN "clear" is entered, THE Mini_CLI SHALL remove all previous command output from the display
8. WHEN an unrecognized command is entered, THE Mini_CLI SHALL display an error message with help suggestion
9. THE Mini_CLI SHALL support color-coded output matching the Dark_Theme_Palette
10. THE Mini_CLI SHALL maintain command history within the display area
11. WHEN the Enter key is pressed, THE Mini_CLI SHALL execute the current command

### Requirement 10: External Links Integration

**User Story:** As a visitor, I want to access external platforms related to the portfolio owner, so that I can connect on GitHub, LinkedIn, Cake, and view Analytics.

#### Acceptance Criteria

1. THE Website SHALL provide External_Links to GitHub, LinkedIn, Cake, and Analytics platforms
2. THE Website SHALL display External_Links in both the footer and Mini_CLI links command output
3. WHEN an External_Link is clicked, THE Website SHALL open the target URL in a new browser tab
4. THE External_Link to GitHub SHALL point to https://github.com/Junwu0615
5. THE Website SHALL apply icon graphics to External_Links for visual identification
6. THE Website SHALL apply hover effects to External_Links

### Requirement 11: Scroll Reveal Animations

**User Story:** As a visitor, I want elements to animate into view as I scroll, so that I experience a dynamic and engaging browsing experience.

#### Acceptance Criteria

1. THE Website SHALL apply Scroll_Reveal animations to Project_Cards, Evolution_Timeline stages, and section headers
2. WHEN an element with Scroll_Reveal enters the Viewport, THE Website SHALL trigger the reveal animation
3. THE Scroll_Reveal animation SHALL execute only once per element
4. THE Scroll_Reveal animation SHALL use fade-in and slide-up effects
5. THE Website SHALL detect Viewport intersection using native browser APIs
6. THE Scroll_Reveal animation SHALL complete within 0.6 seconds

### Requirement 12: Responsive Design

**User Story:** As a mobile visitor, I want the website to adapt to my device screen size, so that I can comfortably browse all content on smartphones and tablets.

#### Acceptance Criteria

1. THE Website SHALL be fully functional on viewport widths from 320px to 2560px
2. THE Website SHALL use CSS media queries to adapt layouts at standard breakpoints (768px, 1024px, 1280px)
3. WHEN viewport width is below 768px, THE Project_Portfolio SHALL display Project_Cards in a single column
4. WHEN viewport width is below 768px, THE Navigation_Bar SHALL collapse into a mobile-friendly menu
5. WHEN viewport width is below 768px, THE Evolution_Timeline SHALL display stages in vertical layout
6. THE Website SHALL maintain text readability at all viewport sizes
7. THE Website SHALL ensure all interactive elements have minimum touch target size of 44x44 pixels on mobile devices

### Requirement 13: Performance and Loading

**User Story:** As a visitor, I want the website to load quickly, so that I can access content without delay.

#### Acceptance Criteria

1. THE Website SHALL load all critical rendering resources (HTML, CSS, inline JavaScript) in a single HTTP request for index.html
2. THE Website SHALL defer non-critical animations until the DOM is fully loaded
3. THE Website SHALL optimize the Architecture_Diagram SVG file size for web delivery
4. THE Website SHALL load External_Link icons asynchronously to prevent render blocking
5. THE Website SHALL complete initial rendering within 1 second on standard broadband connections (5 Mbps)

### Requirement 14: Browser Compatibility

**User Story:** As a visitor using different browsers, I want the website to function consistently, so that I have a reliable experience regardless of my browser choice.

#### Acceptance Criteria

1. THE Website SHALL function correctly on Chrome version 90 and above
2. THE Website SHALL function correctly on Firefox version 88 and above
3. THE Website SHALL function correctly on Safari version 14 and above
4. THE Website SHALL function correctly on Edge version 90 and above
5. THE Website SHALL use standard CSS properties with appropriate vendor prefixes for compatibility
6. THE Website SHALL provide fallback styles for browsers not supporting CSS grid or flexbox

### Requirement 15: Accessibility

**User Story:** As a visitor using assistive technologies, I want to navigate and understand the website content, so that I can access the portfolio information effectively.

#### Acceptance Criteria

1. THE Website SHALL provide semantic HTML5 elements (header, nav, section, article, footer)
2. THE Website SHALL include ARIA labels for interactive elements without visible text labels
3. THE Website SHALL ensure all External_Links have descriptive aria-label attributes
4. THE Website SHALL maintain color contrast ratios of at least 4.5:1 for normal text against backgrounds
5. THE Website SHALL support keyboard navigation for all interactive elements
6. WHEN the Tab key is pressed, THE Website SHALL move focus to the next interactive element in logical order
7. THE Website SHALL provide skip-to-content link for keyboard users to bypass Navigation_Bar
