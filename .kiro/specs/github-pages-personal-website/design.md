# Design Document

## Overview

This design document specifies the architecture for a single-page personal portfolio website deployed on GitHub Pages. The site showcases Ping Chun's professional profile with a dark SRE console aesthetic, featuring four major sections: hero terminal with navigation, filterable project portfolio (10 projects across 5 categories), career evolution timeline, and interactive mini-CLI.

The website is a pure static site built with HTML5, CSS3, and vanilla JavaScript, requiring no build tools or frameworks. The architecture emphasizes modularity, progressive enhancement, and performance optimization suitable for direct GitHub Pages deployment.

## Architecture

### System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        index.html                           │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  <head>                                               │  │
│  │    • Meta tags (viewport, charset, description)      │  │
│  │    • Inline CSS (<style> block)                      │  │
│  │  </head>                                              │  │
│  │                                                        │  │
│  │  <body>                                               │  │
│  │    • Navigation Bar (fixed)                          │  │
│  │    • Hero Terminal Section                           │  │
│  │    • Project Portfolio Section                       │  │
│  │    • Evolution Timeline Section                      │  │
│  │    • Architecture Diagram Section                    │  │
│  │    • Mini-CLI Section                                │  │
│  │    • Footer with External Links                      │  │
│  │    • Inline JavaScript (<script> block)              │  │
│  │  </body>                                              │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ├─── ./assets/svg/Architecture-Diagram-v4.2.svg
                              └─── (Optional: external icon fonts/CDN)
```

### Architectural Principles

1. **Single-File Deployment**: All HTML, CSS, and JavaScript contained in index.html for minimal HTTP requests
2. **Progressive Enhancement**: Core content accessible without JavaScript; animations enhance experience
3. **Mobile-First Responsive**: CSS designed for small screens first, enhanced for larger viewports
4. **Performance-Optimized**: Inline critical CSS/JS, deferred animations, optimized SVG
5. **Accessibility-First**: Semantic HTML5, ARIA labels, keyboard navigation, contrast compliance
6. **Zero Dependencies**: No frameworks, build tools, or external libraries required

## Components and Interfaces

### 1. Navigation Bar Component

**Purpose**: Provide fixed top navigation with smooth scrolling and active section highlighting.

**Structure**:
```html
<nav id="navbar" class="navbar">
  <div class="nav-container">
    <a href="#hero" class="nav-logo">PC</a>
    <ul class="nav-menu">
      <li><a href="#hero" class="nav-link active">Home</a></li>
      <li><a href="#projects" class="nav-link">Projects</a></li>
      <li><a href="#evolution" class="nav-link">Evolution</a></li>
      <li><a href="#cli" class="nav-link">CLI</a></li>
    </ul>
    <button class="nav-toggle" aria-label="Toggle navigation menu">
      <span class="hamburger"></span>
    </button>
  </div>
</nav>
```

**CSS Architecture**:
- **Layout**: Flexbox with space-between for logo and menu
- **Positioning**: `position: fixed; top: 0; z-index: 1000`
- **Responsive**: Hamburger menu below 768px breakpoint
- **Active State**: `.nav-link.active` with accent color (#06B6D4)
- **Transitions**: 0.3s ease for hover and active state changes

**JavaScript Behavior**:
- **Smooth Scroll**: `element.scrollIntoView({ behavior: 'smooth' })` on nav link click
- **Active Highlighting**: IntersectionObserver monitors section visibility, updates `.active` class
- **Mobile Toggle**: Click handler toggles `.nav-menu.open` class for hamburger menu

### 2. Hero Terminal Component

**Purpose**: Display animated typewriter boot sequence with personal information.

**Structure**:
```html
<section id="hero" class="hero-section">
  <div class="terminal-window">
    <div class="terminal-header">
      <span class="terminal-button close"></span>
      <span class="terminal-button minimize"></span>
      <span class="terminal-button maximize"></span>
    </div>
    <div class="terminal-body">
      <div class="boot-sequence">
        <span class="prompt">$</span>
        <span class="cursor">_</span>
      </div>
    </div>
  </div>
  <div class="hero-content">
    <!-- Static content displayed after boot sequence -->
    <h1 class="hero-name">Ping Chun (Junwu0615)</h1>
    <p class="hero-role">Data Engineer → Platform Engineering</p>
    <p class="hero-slogan">Platform Engineering Through Evolution, Not Accumulation</p>
  </div>
</section>
```

**CSS Architecture**:
- **Layout**: Full viewport height (`min-height: 100vh`), centered flexbox
- **Terminal Styling**: Border with terminal header buttons, monospace font
- **Background**: Grid pattern using CSS `background-image: repeating-linear-gradient()`
- **Cursor**: Blinking cursor using `@keyframes blink` animation
- **Responsive**: Font sizes scale with viewport (`clamp()` function)

**JavaScript Behavior**:
- **Boot Sequence Module**:
  ```javascript
  const bootSequence = {
    lines: [
      'Ping Chun (Junwu0615)',
      'Data Engineer → Platform Engineering',
      'Platform Engineering Through Evolution, Not Accumulation'
    ],
    currentLine: 0,
    currentChar: 0,
    typeSpeed: 50, // ms per character
    
    async type() {
      // Typewriter logic with character-by-character insertion
    },
    
    init() {
      // Start on DOMContentLoaded, run once
    }
  };
  ```
- **Execution**: Runs once on page load, removes `.boot-sequence` and displays `.hero-content` on completion
- **State Management**: Boolean flag `hasRun` prevents re-execution

### 3. Project Portfolio Component

**Purpose**: Display 10 project cards with domain-based filtering.

**Structure**:
```html
<section id="projects" class="projects-section">
  <h2 class="section-title">Project Portfolio</h2>
  
  <div class="filter-bar">
    <button class="filter-btn active" data-filter="all">All</button>
    <button class="filter-btn" data-filter="data-engineering">Data Engineering</button>
    <button class="filter-btn" data-filter="mlops">MLOps</button>
    <button class="filter-btn" data-filter="platform-engineering">Platform Engineering</button>
    <button class="filter-btn" data-filter="analytics">Analytics</button>
    <button class="filter-btn" data-filter="devops">DevOps</button>
  </div>
  
  <div class="projects-grid">
    <!-- Project cards generated from data -->
  </div>
</section>
```

**Project Card Structure**:
```html
<article class="project-card" data-category="data-engineering">
  <div class="card-header">
    <h3 class="project-name">Project Name</h3>
    <span class="project-category">Data Engineering</span>
  </div>
  <p class="project-description">Project description text...</p>
  <a href="https://github.com/Junwu0615/project-name" 
     class="project-link" 
     target="_blank" 
     rel="noopener noreferrer"
     aria-label="View Project Name on GitHub">
    View on GitHub →
  </a>
</article>
```

**CSS Architecture**:
- **Grid Layout**: CSS Grid with auto-fit and minmax for responsive columns
  ```css
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
  }
  ```
- **Card Styling**: Background #111827, border #1F2937, hover glow effect using `box-shadow`
- **Responsive**: Single column below 768px, 2 columns 768-1024px, 3+ columns above 1024px
- **Transitions**: 0.3s ease for hover effects and filter transitions
- **Filter Animation**: Fade-out/fade-in with opacity and transform

**JavaScript Behavior**:
- **Project Data Structure**:
  ```javascript
  const projects = [
    {
      name: 'Project 1',
      description: 'Description...',
      category: 'data-engineering',
      slug: 'project-1'
    },
    // ... 9 more projects
  ];
  ```
- **Rendering Function**: Generates project cards from data array, ensures single category per project
- **URL Generation**: `https://github.com/Junwu0615/${project.slug}`
- **Filter Module**:
  ```javascript
  const filterSystem = {
    activeFilter: 'all',
    
    filter(category) {
      const cards = document.querySelectorAll('.project-card');
      cards.forEach(card => {
        const shouldShow = category === 'all' || 
                          card.dataset.category === category;
        card.style.display = shouldShow ? 'block' : 'none';
      });
    },
    
    setActive(button) {
      // Update active button styling
    }
  };
  ```
- **Event Handling**: Click handlers on filter buttons update active state and trigger filter

### 4. Evolution Timeline Component

**Purpose**: Display 3 career stages with scroll-reveal animations.

**Structure**:
```html
<section id="evolution" class="evolution-section">
  <h2 class="section-title">Career Evolution</h2>
  
  <div class="timeline">
    <article class="timeline-stage" data-stage="1">
      <div class="stage-marker">1</div>
      <div class="stage-content">
        <h3 class="stage-title">Data Engineering Foundation</h3>
        <p class="stage-description">
          Built data pipelines and analytics systems...
        </p>
      </div>
    </article>
    <!-- Stages 2 and 3 -->
  </div>
</section>
```

**CSS Architecture**:
- **Desktop Layout**: Horizontal flexbox with connecting line using `::before` pseudo-element
- **Mobile Layout**: Vertical flexbox with vertical connecting line
- **Stage Marker**: Circular badge with stage number, positioned on timeline
- **Scroll Reveal**: Initial state `opacity: 0; transform: translateY(30px)`
- **Revealed State**: `.revealed { opacity: 1; transform: translateY(0); transition: 0.6s ease; }`
- **Responsive Breakpoint**: Switch to vertical at 768px

**JavaScript Behavior**:
- **Scroll Reveal Module**:
  ```javascript
  const scrollReveal = {
    observer: null,
    
    init() {
      const options = {
        root: null,
        threshold: 0.2, // Trigger when 20% visible
        rootMargin: '0px'
      };
      
      this.observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting && !entry.target.classList.contains('revealed')) {
            entry.target.classList.add('revealed');
            this.observer.unobserve(entry.target); // Once only
          }
        });
      }, options);
      
      // Observe all timeline stages
      document.querySelectorAll('.timeline-stage').forEach(el => {
        this.observer.observe(el);
      });
    }
  };
  ```

### 5. Architecture Diagram Component

**Purpose**: Display embedded SVG with dark theme CSS filters.

**Structure**:
```html
<section id="architecture" class="architecture-section">
  <h2 class="section-title">System Architecture</h2>
  
  <div class="diagram-container">
    <img src="./assets/svg/Architecture-Diagram-v4.2.svg" 
         alt="System architecture diagram showing component relationships"
         class="architecture-diagram"
         loading="lazy">
  </div>
</section>
```

**CSS Architecture**:
- **Container**: Max-width constraint with centered alignment
- **SVG Styling**: Dark theme adaptation using CSS filters
  ```css
  .architecture-diagram {
    width: 100%;
    height: auto;
    filter: invert(0.9) brightness(1.2) hue-rotate(180deg);
    /* Adapts light SVG colors to dark theme */
  }
  ```
- **Responsive**: `width: 100%; height: auto` maintains aspect ratio
- **Performance**: `loading="lazy"` for below-fold optimization

**Aspect Ratio Preservation**:
- Use `aspect-ratio` CSS property with fallback
- SVG viewBox attribute ensures proportional scaling

### 6. Mini-CLI Component

**Purpose**: Provide interactive command-line interface.

**Structure**:
```html
<section id="cli" class="cli-section">
  <h2 class="section-title">Interactive Terminal</h2>
  
  <div class="cli-terminal">
    <div class="cli-output" id="cli-output">
      <div class="cli-welcome">
        Type 'help' for available commands
      </div>
    </div>
    
    <div class="cli-input-line">
      <span class="cli-prompt">visitor@portfolio:~$</span>
      <input type="text" 
             class="cli-input" 
             id="cli-input"
             aria-label="Command input"
             autocomplete="off"
             spellcheck="false">
    </div>
  </div>
</section>
```

**CSS Architecture**:
- **Terminal Styling**: Monospace font, background #0B0F14, padding for terminal spacing
- **Output Area**: Scrollable with `overflow-y: auto`, max-height constraint
- **Input Line**: Flexbox with prompt and input field
- **Color Coding**: CSS classes for different output types (`.cli-success`, `.cli-error`, `.cli-info`)
- **Scrollbar**: Styled custom scrollbar for dark theme

**JavaScript Behavior**:
- **CLI Module**:
  ```javascript
  const miniCLI = {
    commands: {
      whoami: () => {
        return `
╔════════════════════════════════════════╗
║  Ping Chun (Junwu0615)                ║
║  Data Engineer → Platform Engineering  ║
║  Location: Taiwan                      ║
╚════════════════════════════════════════╝
        `;
      },
      
      stack: () => {
        return `
┌─────────────────┬────────────────────────────────┐
│ Category        │ Technologies                   │
├─────────────────┼────────────────────────────────┤
│ Languages       │ Python, JavaScript, Go         │
│ Cloud           │ AWS, GCP                       │
│ Data            │ Spark, Kafka, Airflow          │
│ Containers      │ Docker, Kubernetes             │
│ IaC             │ Terraform, CDK                 │
└─────────────────┴────────────────────────────────┘
        `;
      },
      
      links: () => {
        return `
External Links:
  • GitHub:    https://github.com/Junwu0615
  • LinkedIn:  [LinkedIn URL]
  • Cake:      [Cake URL]
  • Analytics: [Analytics URL]
        `;
      },
      
      help: () => {
        return `
Available Commands:
  whoami  - Display personal information
  stack   - Show technology stack
  links   - Display external platform links
  help    - Show this help message
  clear   - Clear terminal output
        `;
      },
      
      clear: () => {
        document.getElementById('cli-output').innerHTML = '';
        return '';
      }
    },
    
    execute(input) {
      const cmd = input.trim().toLowerCase();
      const output = this.commands[cmd] || 
        `Command not found: ${cmd}. Type 'help' for available commands.`;
      return output;
    },
    
    addOutput(text, type = 'normal') {
      const outputDiv = document.getElementById('cli-output');
      const entry = document.createElement('div');
      entry.className = `cli-entry cli-${type}`;
      entry.textContent = text;
      outputDiv.appendChild(entry);
      outputDiv.scrollTop = outputDiv.scrollHeight; // Auto-scroll
    },
    
    init() {
      const input = document.getElementById('cli-input');
      input.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') {
          const command = input.value;
          this.addOutput(`visitor@portfolio:~$ ${command}`, 'command');
          const result = this.execute(command);
          if (result) this.addOutput(result, 'output');
          input.value = '';
        }
      });
    }
  };
  ```

### 7. Footer Component

**Purpose**: Display external links with icons.

**Structure**:
```html
<footer class="footer">
  <div class="footer-content">
    <div class="social-links">
      <a href="https://github.com/Junwu0615" 
         target="_blank" 
         rel="noopener noreferrer"
         aria-label="Visit GitHub profile"
         class="social-link">
        <span class="icon-github"></span>
        GitHub
      </a>
      <!-- Additional social links -->
    </div>
    
    <p class="footer-text">
      © 2024 Ping Chun (Junwu0615) | Built with ❤️ and vanilla JavaScript
    </p>
  </div>
</footer>
```

**CSS Architecture**:
- **Layout**: Flexbox with center alignment
- **Links**: Inline-flex with icon and text, hover effects with accent color
- **Icons**: CSS pseudo-elements or inline SVG for GitHub/LinkedIn/etc icons
- **Responsive**: Stack vertically on mobile

## CSS Architecture

### Color System

**Dark Theme Palette**:
```css
:root {
  /* Backgrounds */
  --color-bg-primary: #0B0F14;
  --color-bg-secondary: #111827;
  --color-bg-tertiary: #1F2937;
  
  /* Accents */
  --color-accent-primary: #06B6D4; /* Cyan */
  --color-accent-secondary: #10B981; /* Green */
  
  /* Text */
  --color-text-primary: #F9FAFB;
  --color-text-secondary: #D1D5DB;
  --color-text-muted: #9CA3AF;
  
  /* Borders */
  --color-border: #374151;
  
  /* Semantic Colors */
  --color-success: #10B981;
  --color-error: #EF4444;
  --color-warning: #F59E0B;
  --color-info: #06B6D4;
}
```

### Typography System

**Font Stack**:
```css
:root {
  --font-mono: 'Fira Code', 'Consolas', 'Monaco', 'Courier New', monospace;
  --font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif;
}

body {
  font-family: var(--font-mono);
  font-size: 16px;
  line-height: 1.6;
  color: var(--color-text-primary);
  background-color: var(--color-bg-primary);
}

h1 { font-size: clamp(2rem, 5vw, 3.5rem); }
h2 { font-size: clamp(1.75rem, 4vw, 2.5rem); }
h3 { font-size: clamp(1.25rem, 3vw, 1.75rem); }
```

### Animation System

**Keyframe Animations**:
```css
/* Blinking cursor */
@keyframes blink {
  0%, 50% { opacity: 1; }
  51%, 100% { opacity: 0; }
}

/* Fade in up */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Glow pulse */
@keyframes glow {
  0%, 100% {
    box-shadow: 0 0 5px var(--color-accent-primary);
  }
  50% {
    box-shadow: 0 0 20px var(--color-accent-primary),
                0 0 30px var(--color-accent-primary);
  }
}
```

**Transition Standards**:
```css
.transition-fast { transition: all 0.2s ease; }
.transition-normal { transition: all 0.3s ease; }
.transition-slow { transition: all 0.6s ease; }
```

### Responsive Breakpoints

```css
/* Mobile First Approach */

/* Small devices (phones) */
@media (min-width: 480px) {
  /* Enhanced mobile styles */
}

/* Medium devices (tablets) */
@media (min-width: 768px) {
  .nav-menu { display: flex; }
  .nav-toggle { display: none; }
  .projects-grid { grid-template-columns: repeat(2, 1fr); }
  .timeline { flex-direction: row; }
}

/* Large devices (laptops) */
@media (min-width: 1024px) {
  .projects-grid { grid-template-columns: repeat(3, 1fr); }
}

/* Extra large devices (desktops) */
@media (min-width: 1280px) {
  .container { max-width: 1200px; }
}

/* Ultra wide */
@media (min-width: 1920px) {
  .container { max-width: 1400px; }
}
```

### Grid Background Pattern

```css
body::before {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  background-image: 
    repeating-linear-gradient(
      0deg,
      transparent,
      transparent 50px,
      rgba(6, 182, 212, 0.03) 50px,
      rgba(6, 182, 212, 0.03) 51px
    ),
    repeating-linear-gradient(
      90deg,
      transparent,
      transparent 50px,
      rgba(6, 182, 212, 0.03) 50px,
      rgba(6, 182, 212, 0.03) 51px
    );
}
```

## JavaScript Module Architecture

### Boot Sequence and Initialization

**Initialization Flow**:
```javascript
document.addEventListener('DOMContentLoaded', () => {
  // Phase 1: Boot Sequence (blocks other animations)
  bootSequence.init();
  
  // Phase 2: Initialize interactive components
  setTimeout(() => {
    navigationSystem.init();
    scrollReveal.init();
    filterSystem.init();
    miniCLI.init();
  }, bootSequence.estimatedDuration);
});
```

### Module Pattern

All JavaScript components follow a module pattern for encapsulation:

```javascript
const ModuleName = {
  // State
  state: {},
  
  // Initialization
  init() {
    this.setupEventListeners();
    this.initialRender();
  },
  
  // Event handlers
  setupEventListeners() {},
  
  // Public methods
  publicMethod() {},
  
  // Private methods (convention: prefix with _)
  _privateMethod() {}
};
```

### Scroll Reveal System

**Implementation**:
```javascript
const scrollReveal = {
  observer: null,
  targets: [
    '.project-card',
    '.timeline-stage',
    '.section-title'
  ],
  
  init() {
    const options = {
      root: null,
      threshold: 0.2,
      rootMargin: '0px 0px -50px 0px'
    };
    
    this.observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('revealed');
          entry.target.style.transitionDelay = 
            `${entry.target.dataset.delay || 0}ms`;
          this.observer.unobserve(entry.target);
        }
      });
    }, options);
    
    this.targets.forEach(selector => {
      document.querySelectorAll(selector).forEach((el, index) => {
        el.dataset.delay = index * 100; // Stagger effect
        this.observer.observe(el);
      });
    });
  },
  
  destroy() {
    if (this.observer) {
      this.observer.disconnect();
    }
  }
};
```

### Navigation System

**Active Section Detection**:
```javascript
const navigationSystem = {
  sections: [],
  navLinks: [],
  
  init() {
    this.sections = document.querySelectorAll('section[id]');
    this.navLinks = document.querySelectorAll('.nav-link');
    this.setupSmoothScroll();
    this.setupActiveDetection();
  },
  
  setupSmoothScroll() {
    this.navLinks.forEach(link => {
      link.addEventListener('click', (e) => {
        e.preventDefault();
        const targetId = link.getAttribute('href').substring(1);
        const target = document.getElementById(targetId);
        target.scrollIntoView({ behavior: 'smooth', block: 'start' });
      });
    });
  },
  
  setupActiveDetection() {
    const options = {
      root: null,
      threshold: 0.3,
      rootMargin: '-100px 0px -60% 0px'
    };
    
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          this.setActiveLink(entry.target.id);
        }
      });
    }, options);
    
    this.sections.forEach(section => observer.observe(section));
  },
  
  setActiveLink(sectionId) {
    this.navLinks.forEach(link => {
      link.classList.remove('active');
      if (link.getAttribute('href') === `#${sectionId}`) {
        link.classList.add('active');
      }
    });
  }
};
```

### Filter System

**Filtering Logic**:
```javascript
const filterSystem = {
  activeFilter: 'all',
  buttons: [],
  cards: [],
  
  init() {
    this.buttons = document.querySelectorAll('.filter-btn');
    this.cards = document.querySelectorAll('.project-card');
    this.setupEventListeners();
  },
  
  setupEventListeners() {
    this.buttons.forEach(btn => {
      btn.addEventListener('click', () => {
        this.setFilter(btn.dataset.filter);
        this.setActiveButton(btn);
      });
    });
  },
  
  setFilter(category) {
    this.activeFilter = category;
    
    this.cards.forEach(card => {
      const shouldShow = category === 'all' || 
                        card.dataset.category === category;
      
      if (shouldShow) {
        card.style.display = 'block';
        // Re-trigger scroll reveal if needed
        setTimeout(() => card.classList.add('revealed'), 50);
      } else {
        card.style.display = 'none';
      }
    });
  },
  
  setActiveButton(activeBtn) {
    this.buttons.forEach(btn => btn.classList.remove('active'));
    activeBtn.classList.add('active');
  }
};
```

### CLI Interpreter

**Command Processing**:
```javascript
const miniCLI = {
  history: [],
  historyIndex: -1,
  
  commands: {
    whoami: () => `...`,
    stack: () => `...`,
    links: () => `...`,
    help: () => `...`,
    clear: function() {
      document.getElementById('cli-output').innerHTML = '';
      return '';
    }
  },
  
  init() {
    const input = document.getElementById('cli-input');
    
    input.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') {
        this.executeCommand(input.value);
        this.history.push(input.value);
        this.historyIndex = this.history.length;
        input.value = '';
      } else if (e.key === 'ArrowUp') {
        e.preventDefault();
        this.navigateHistory(-1, input);
      } else if (e.key === 'ArrowDown') {
        e.preventDefault();
        this.navigateHistory(1, input);
      }
    });
  },
  
  executeCommand(input) {
    const cmd = input.trim().toLowerCase();
    
    // Display command
    this.addOutput(`visitor@portfolio:~$ ${input}`, 'command');
    
    // Execute
    if (cmd in this.commands) {
      const result = this.commands[cmd]();
      if (result) this.addOutput(result, 'success');
    } else if (cmd === '') {
      // Empty command, do nothing
    } else {
      this.addOutput(
        `Command not found: ${cmd}. Type 'help' for available commands.`,
        'error'
      );
    }
  },
  
  addOutput(text, type) {
    const output = document.getElementById('cli-output');
    const entry = document.createElement('pre');
    entry.className = `cli-entry cli-${type}`;
    entry.textContent = text;
    output.appendChild(entry);
    output.scrollTop = output.scrollHeight;
  },
  
  navigateHistory(direction, input) {
    this.historyIndex = Math.max(0, 
      Math.min(this.history.length, this.historyIndex + direction)
    );
    input.value = this.history[this.historyIndex] || '';
  }
};
```

## SVG Integration Strategy

### Dark Theme Adaptation

**CSS Filter Approach**:
```css
.architecture-diagram {
  /* Invert colors for dark theme */
  filter: invert(0.9) brightness(1.2) hue-rotate(180deg);
  
  /* Smooth transition if theme toggle added later */
  transition: filter 0.3s ease;
  
  /* Ensure crisp rendering */
  image-rendering: crisp-edges;
}
```

**Alternative: Inline SVG with CSS Variables** (if manual editing allowed):
```css
/* Define colors in CSS */
:root {
  --svg-bg: #111827;
  --svg-text: #F9FAFB;
  --svg-primary: #06B6D4;
}

/* Reference in inline SVG */
.svg-element {
  fill: var(--svg-bg);
  stroke: var(--svg-primary);
}
```

### Responsive SVG

**Container-based Scaling**:
```css
.diagram-container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.architecture-diagram {
  width: 100%;
  height: auto;
  display: block;
}

@media (max-width: 768px) {
  .diagram-container {
    padding: 1rem;
  }
}
```

## Performance Optimization

### Critical Rendering Path

**Inline Critical CSS**:
```html
<head>
  <style>
    /* Critical above-the-fold CSS inlined */
    /* - Reset styles */
    /* - Layout structure */
    /* - Navigation */
    /* - Hero section */
    /* Approx 10-15KB after minification */
  </style>
</head>
```

**Deferred Non-Critical CSS**:
```html
<link rel="preload" href="animations.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="animations.css"></noscript>
```

### JavaScript Optimization

**Deferred Execution**:
```javascript
// Boot sequence runs immediately on DOMContentLoaded
// Other features deferred until boot completes

document.addEventListener('DOMContentLoaded', () => {
  bootSequence.init();
  
  setTimeout(() => {
    // Initialize non-critical features
    requestIdleCallback(() => {
      miniCLI.init();
      // ... other features
    });
  }, bootSequence.estimatedDuration);
});
```

**Event Delegation**:
```javascript
// Instead of individual listeners on each card
// Use delegation on parent container
document.querySelector('.filter-bar').addEventListener('click', (e) => {
  if (e.target.classList.contains('filter-btn')) {
    filterSystem.setFilter(e.target.dataset.filter);
  }
});
```

### Asset Optimization

**SVG Optimization**:
- Remove unnecessary metadata and comments
- Simplify paths using SVGO
- Target file size: <100KB for Architecture-Diagram-v4.2.svg

**Lazy Loading**:
```html
<img src="./assets/svg/Architecture-Diagram-v4.2.svg" 
     loading="lazy" 
     decoding="async"
     alt="Architecture diagram">
```

### Performance Budget

- **Initial HTML Load**: <50KB (minified)
- **Total Page Weight**: <150KB (HTML + inlined CSS/JS + SVG)
- **First Contentful Paint**: <1.0s (on 5 Mbps connection)
- **Time to Interactive**: <2.0s
- **Lighthouse Performance Score**: >90

## File Structure

```
Junwu0615.github.io/
├── index.html              # Single-file application (HTML + CSS + JS)
├── assets/
│   └── svg/
│       └── Architecture-Diagram-v4.2.svg
├── .gitignore
└── README.md              # GitHub repository documentation
```

**index.html Structure**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Ping Chun - Platform Engineering Portfolio">
  <title>Ping Chun (Junwu0615) | Platform Engineering</title>
  
  <style>
    /* All CSS inlined here (~20-30KB) */
  </style>
</head>
<body>
  <!-- Navigation -->
  <nav>...</nav>
  
  <!-- Hero Terminal -->
  <section id="hero">...</section>
  
  <!-- Project Portfolio -->
  <section id="projects">...</section>
  
  <!-- Evolution Timeline -->
  <section id="evolution">...</section>
  
  <!-- Architecture Diagram -->
  <section id="architecture">...</section>
  
  <!-- Mini-CLI -->
  <section id="cli">...</section>
  
  <!-- Footer -->
  <footer>...</footer>
  
  <script>
    /* All JavaScript inlined here (~15-20KB) */
  </script>
</body>
</html>
```

## Data Models

### Project Data Structure

```javascript
const projects = [
  {
    name: 'String',           // Project display name
    slug: 'String',           // URL-safe project identifier
    description: 'String',    // Brief description (100-200 chars)
    category: 'String',       // One of: data-engineering, mlops, 
                              //         platform-engineering, analytics, devops
    githubUrl: 'String'       // Full GitHub URL (auto-generated from slug)
  }
];

// Example:
{
  name: 'Real-Time Data Pipeline',
  slug: 'realtime-pipeline',
  description: 'Apache Kafka and Spark streaming pipeline for real-time analytics',
  category: 'data-engineering',
  githubUrl: 'https://github.com/Junwu0615/realtime-pipeline'
}
```

### Timeline Stage Data Structure

```javascript
const timelineStages = [
  {
    stage: Number,            // Stage number (1-3)
    title: 'String',          // Stage title
    description: 'String',    // Stage description (200-300 chars)
    period: 'String'          // Time period (optional)
  }
];

// Example:
{
  stage: 1,
  title: 'Data Engineering Foundation',
  description: 'Built robust data pipelines and analytics systems using Python, Spark, and AWS',
  period: '2020-2022'
}
```

### CLI Output Data Structure

```javascript
const cliOutputs = {
  whoami: {
    type: 'ascii-art',
    content: 'String'         // Formatted ASCII art with info
  },
  stack: {
    type: 'table',
    headers: ['Category', 'Technologies'],
    rows: [
      ['Languages', 'Python, JavaScript, Go'],
      // ...
    ]
  },
  links: {
    type: 'list',
    items: [
      { label: 'GitHub', url: 'String' },
      // ...
    ]
  }
};
```

## Error Handling

### JavaScript Error Handling

**Graceful Degradation**:
```javascript
const safeInit = (module, name) => {
  try {
    module.init();
  } catch (error) {
    console.error(`Failed to initialize ${name}:`, error);
    // Site remains functional even if one module fails
  }
};

document.addEventListener('DOMContentLoaded', () => {
  safeInit(bootSequence, 'Boot Sequence');
  safeInit(navigationSystem, 'Navigation');
  safeInit(scrollReveal, 'Scroll Reveal');
  safeInit(filterSystem, 'Filter System');
  safeInit(miniCLI, 'Mini CLI');
});
```

**CLI Error Messages**:
```javascript
const errorMessages = {
  unknownCommand: (cmd) => 
    `Command not found: ${cmd}. Type 'help' for available commands.`,
  emptyCommand: () => 
    '', // Silent on empty input
  executionError: (cmd) => 
    `Error executing '${cmd}'. Please try again.`
};
```

## Accessibility Features

### Semantic HTML

```html
<!-- Use semantic elements throughout -->
<header>
  <nav aria-label="Main navigation">...</nav>
</header>

<main>
  <section aria-labelledby="projects-heading">
    <h2 id="projects-heading">Projects</h2>
    ...
  </section>
</main>

<footer role="contentinfo">...</footer>
```

### ARIA Labels

```html
<!-- Interactive elements without visible labels -->
<button class="nav-toggle" 
        aria-label="Toggle navigation menu"
        aria-expanded="false">
  <span class="hamburger"></span>
</button>

<a href="https://github.com/Junwu0615" 
   aria-label="Visit Ping Chun's GitHub profile"
   target="_blank"
   rel="noopener noreferrer">
  <span class="icon-github" aria-hidden="true"></span>
</a>
```

### Keyboard Navigation

**Focus Management**:
```css
/* Visible focus indicators */
*:focus {
  outline: 2px solid var(--color-accent-primary);
  outline-offset: 2px;
}

/* Skip to content link */
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: var(--color-accent-primary);
  padding: 0.5rem 1rem;
  z-index: 10000;
}

.skip-link:focus {
  top: 0;
}
```

**Tab Order**:
```javascript
// Ensure logical tab order
const focusableElements = document.querySelectorAll(
  'a[href], button, input, [tabindex]:not([tabindex="-1"])'
);

// Manage tab order in mobile menu
const mobileMenuToggle = () => {
  const menu = document.querySelector('.nav-menu');
  const isOpen = menu.classList.toggle('open');
  
  // Update ARIA state
  document.querySelector('.nav-toggle')
    .setAttribute('aria-expanded', isOpen);
  
  // Manage focus
  if (isOpen) {
    menu.querySelector('.nav-link').focus();
  }
};
```

### Color Contrast

**Contrast Ratios** (against #0B0F14 background):
- Primary text (#F9FAFB): 15.8:1 ✓
- Secondary text (#D1D5DB): 12.6:1 ✓
- Muted text (#9CA3AF): 7.5:1 ✓
- Accent primary (#06B6D4): 7.2:1 ✓
- Accent secondary (#10B981): 6.8:1 ✓

All exceed WCAG AAA standard (7:1 for normal text, 4.5:1 for large text).

## Browser Compatibility

### CSS Feature Support

**Fallbacks**:
```css
/* Grid with flexbox fallback */
@supports not (display: grid) {
  .projects-grid {
    display: flex;
    flex-wrap: wrap;
  }
  
  .project-card {
    flex: 1 1 300px;
  }
}

/* CSS variables with fallback */
.element {
  color: #06B6D4; /* Fallback */
  color: var(--color-accent-primary);
}

/* Clamp with fallback */
.heading {
  font-size: 2rem; /* Fallback */
  font-size: clamp(1.5rem, 4vw, 2.5rem);
}
```

### JavaScript Feature Detection

```javascript
// IntersectionObserver with fallback
if ('IntersectionObserver' in window) {
  scrollReveal.init();
} else {
  // Fallback: show all elements immediately
  document.querySelectorAll('.timeline-stage, .project-card')
    .forEach(el => el.classList.add('revealed'));
}

// Smooth scroll with fallback
const smoothScroll = (target) => {
  if ('scrollBehavior' in document.documentElement.style) {
    target.scrollIntoView({ behavior: 'smooth' });
  } else {
    target.scrollIntoView();
  }
};
```

## Deployment Strategy

### GitHub Pages Configuration

**Repository Settings**:
- Source: Deploy from main branch, root directory
- Custom domain: Optional
- Enforce HTTPS: Enabled

**Build Process**:
No build process required. Push index.html and assets directly to repository.

**Deployment Workflow**:
1. Develop locally (live server or `python -m http.server`)
2. Test across browsers and devices
3. Validate HTML/CSS/accessibility
4. Commit to main branch
5. GitHub Pages automatically deploys changes

### Local Development

**Simple HTTP Server**:
```bash
# Python 3
python -m http.server 8000

# Node.js (if installed)
npx serve .

# PHP (if installed)
php -S localhost:8000
```

Access at `http://localhost:8000`

## Testing Strategy

### Manual Testing Checklist

**Functional Testing**:
- [ ] Boot sequence plays once on page load
- [ ] Navigation smooth scrolls to sections
- [ ] Active nav link updates on scroll
- [ ] All 10 project cards render correctly
- [ ] Filter buttons show/hide correct cards
- [ ] Timeline stages animate on scroll
- [ ] Architecture SVG loads and displays
- [ ] CLI accepts commands and shows output
- [ ] All external links open in new tabs

**Responsive Testing**:
- [ ] Test at 320px, 375px, 768px, 1024px, 1920px widths
- [ ] Mobile menu works correctly
- [ ] Timeline switches to vertical on mobile
- [ ] Touch targets are at least 44x44px

**Accessibility Testing**:
- [ ] Tab through all interactive elements
- [ ] Screen reader announces elements correctly
- [ ] Skip-to-content link works
- [ ] Color contrast passes WCAG AA

**Browser Testing**:
- [ ] Chrome 90+
- [ ] Firefox 88+
- [ ] Safari 14+
- [ ] Edge 90+

### Automated Testing (Optional)

**HTML Validation**:
```bash
# W3C HTML validator
npx html-validator --file=index.html
```

**Accessibility Audit**:
```bash
# pa11y
npx pa11y http://localhost:8000
```

**Performance Testing**:
```bash
# Lighthouse CI
npx lighthouse http://localhost:8000 --view
```

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

### Property 1: Resource Path Integrity

*For any* resource reference (src, href, url) in the HTML, CSS, or JavaScript, the path SHALL be relative to the repository root or use a valid relative path pattern.

**Validates: Requirements 1.4**

### Property 2: Theme Color Consistency

*For any* CSS color declaration in the stylesheet, the color value SHALL match one of the defined Dark_Theme_Palette colors (#0B0F14, #111827, #1F2937, #06B6D4, #10B981, or standard text colors), and all text elements SHALL use monospace font-family.

**Validates: Requirements 2.2, 2.3, 2.4, 2.5, 2.6**

### Property 3: Navigation Scroll Behavior

*For any* navigation link click event, the page SHALL smoothly scroll to the corresponding section without page reload, and the active navigation link SHALL update to reflect the current viewport section.

**Validates: Requirements 3.3, 3.4**

### Property 4: Navigation Visibility

*For any* viewport width from 320px to 2560px, the Navigation_Bar SHALL remain visible and accessible (either as full menu or mobile menu).

**Validates: Requirements 3.5**

### Property 5: Boot Sequence Idempotence

*For any* number of page interactions or attempted re-triggers, the Boot_Sequence SHALL execute exactly once per page load.

**Validates: Requirements 4.6**

### Property 6: Project Card Completeness

*For any* Project_Card rendered in the portfolio, it SHALL display exactly three required fields (name, description, category) and contain a valid GitHub URL following the pattern `https://github.com/Junwu0615/{slug}`.

**Validates: Requirements 5.3, 5.4, 5.5, 5.6**

### Property 7: Filter Display Correctness

*For any* selected category filter, all visible Project_Cards SHALL match the selected category, or if "All" is selected, all 10 cards SHALL be visible.

**Validates: Requirements 6.3**

### Property 8: Filter State Indication

*For any* active filter button, it SHALL have the visual "active" class applied, and no page reload SHALL occur during filter changes.

**Validates: Requirements 6.5, 6.6**

### Property 9: Timeline Stage Completeness

*For any* timeline stage element, it SHALL contain both a title and a description field.

**Validates: Requirements 7.3**

### Property 10: Scroll Reveal Animation Attachment

*For any* element designated for scroll reveal (project cards, timeline stages, section titles), it SHALL have an IntersectionObserver attached and the reveal animation SHALL trigger when the element enters the viewport.

**Validates: Requirements 7.4, 11.1, 11.2**

### Property 11: Scroll Reveal Idempotence

*For any* scroll-reveal element, the reveal animation SHALL execute exactly once, even if the element exits and re-enters the viewport multiple times.

**Validates: Requirements 11.3**

### Property 12: Timeline Responsive Layout

*For any* viewport width below 768px, the Evolution_Timeline SHALL display in vertical layout, and for any viewport width at or above 768px, it SHALL display in horizontal layout.

**Validates: Requirements 7.6, 7.7**

### Property 13: SVG Responsive Scaling

*For any* viewport width, the Architecture_Diagram SHALL scale to fit the container width while maintaining its original aspect ratio.

**Validates: Requirements 8.5, 8.6**

### Property 14: CLI Command Recognition

*For any* recognized CLI command (whoami, stack, links, help, clear), executing the command SHALL produce the specified output format without error.

**Validates: Requirements 9.2**

### Property 15: CLI Error Handling

*For any* unrecognized command input, the Mini_CLI SHALL display an error message suggesting the help command.

**Validates: Requirements 9.8**

### Property 16: CLI Output Color Coding

*For any* CLI output displayed, the text SHALL use CSS classes that apply colors from the Dark_Theme_Palette.

**Validates: Requirements 9.9**

### Property 17: CLI History Persistence

*For any* sequence of CLI commands executed, all previous command outputs SHALL remain visible in the display area until the clear command is executed.

**Validates: Requirements 9.10**

### Property 18: External Link Target Attribute

*For any* external link on the website, it SHALL have `target="_blank"` and `rel="noopener noreferrer"` attributes applied.

**Validates: Requirements 10.3**

### Property 19: External Link Icon Presence

*For any* external link element, it SHALL contain an associated icon graphic and have hover effect CSS applied.

**Validates: Requirements 10.5, 10.6**

### Property 20: Responsive Viewport Functionality

*For any* viewport width in the range 320px to 2560px, all core website functionality (navigation, filtering, CLI input, link clicking) SHALL remain operational.

**Validates: Requirements 12.1**

### Property 21: Mobile Touch Target Size

*For any* interactive element (buttons, links, inputs) when viewport width is below 768px, the element's computed width and height SHALL be at least 44 pixels.

**Validates: Requirements 12.7**

### Property 22: CSS Vendor Prefix Presence

*For any* CSS property known to require vendor prefixes for compatibility with browsers from 2021 (e.g., flexbox, transitions, transforms), the stylesheet SHALL include appropriate vendor-prefixed versions where applicable.

**Validates: Requirements 14.5**

### Property 23: Semantic HTML Usage

*For any* major section of the website (navigation, main content, footer), it SHALL use semantic HTML5 elements (nav, section, article, header, footer) rather than generic div elements.

**Validates: Requirements 15.1**

### Property 24: Interactive Element ARIA Labels

*For any* interactive element without visible text content (icon-only buttons, logo links), it SHALL have an `aria-label` attribute providing a descriptive label.

**Validates: Requirements 15.2, 15.3**

### Property 25: Color Contrast Compliance

*For any* text element and its background, the color contrast ratio SHALL be at least 4.5:1 for normal text or 3:1 for large text (18pt+), meeting WCAG AA standards.

**Validates: Requirements 15.4**

### Property 26: Keyboard Navigation Support

*For any* interactive element, it SHALL be reachable via Tab key navigation and have a visible focus indicator applied.

**Validates: Requirements 15.5, 15.6**

## Security Considerations

### External Link Security

**rel="noopener noreferrer"**:
```html
<a href="https://external-site.com" 
   target="_blank" 
   rel="noopener noreferrer">
```
- **noopener**: Prevents new page from accessing `window.opener`
- **noreferrer**: Prevents referrer information leakage

### Content Security Policy (Optional)

**Meta Tag CSP**:
```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; 
               style-src 'unsafe-inline'; 
               script-src 'unsafe-inline'; 
               img-src 'self' data:;">
```

**Note**: 'unsafe-inline' required for inline CSS/JS in single-file architecture.

### Input Sanitization

**CLI Input Handling**:
```javascript
const sanitizeInput = (input) => {
  // Escape HTML special characters
  const div = document.createElement('div');
  div.textContent = input;
  return div.innerHTML;
};

miniCLI.addOutput(sanitizeInput(userInput), 'command');
```

## Future Enhancements

### Potential Additions (Not in Current Scope)

1. **Dark/Light Theme Toggle**: CSS custom properties with theme switching
2. **Project Details Modal**: Expandable project information without navigation
3. **CLI Command History Navigation**: Arrow up/down for history
4. **Search Functionality**: Filter projects by keyword
5. **Analytics Integration**: Google Analytics or Plausible
6. **RSS Feed**: Blog post integration
7. **Internationalization**: Multi-language support
8. **Print Stylesheet**: Optimized print layout
9. **Service Worker**: Offline functionality
10. **Blog Section**: Static blog with markdown rendering

### Maintainability Considerations

**Code Organization for Future Modularization**:
If the site grows beyond single-file architecture, the current module pattern allows easy extraction:

```
src/
├── css/
│   ├── reset.css
│   ├── variables.css
│   ├── layout.css
│   ├── components/
│   │   ├── navigation.css
│   │   ├── hero.css
│   │   ├── projects.css
│   │   └── cli.css
│   └── utilities.css
├── js/
│   ├── main.js
│   ├── modules/
│   │   ├── bootSequence.js
│   │   ├── navigation.js
│   │   ├── scrollReveal.js
│   │   ├── filterSystem.js
│   │   └── miniCLI.js
│   └── data/
│       ├── projects.js
│       └── timeline.js
└── index.html
```

Then use a build tool (Vite, Parcel) to bundle back to single file for deployment.

## Conclusion

This design provides a comprehensive architecture for a performant, accessible, and maintainable single-page portfolio website. The modular JavaScript design, semantic HTML structure, and CSS architecture support both current requirements and future enhancements while maintaining the simplicity required for direct GitHub Pages deployment.

Key architectural decisions:
- **Single-file deployment** for minimal HTTP requests and simplicity
- **Vanilla JavaScript modules** for maintainability without framework overhead
- **Progressive enhancement** ensures core functionality without JavaScript
- **Mobile-first responsive design** adapts to all viewport sizes
- **Accessibility-first approach** supports all users and assistive technologies
- **Performance optimization** targets <1s initial render time

The design balances developer experience (clear module boundaries, documented patterns) with user experience (fast loading, smooth animations, accessible interactions) while meeting all 15 requirements specified in the requirements document.
