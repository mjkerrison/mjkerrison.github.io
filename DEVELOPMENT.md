# Proof Consulting Website - Development Documentation

## 🎯 Project Overview

Professional landing page for Proof Consulting, an AI safety management consultancy. Built as a single-page application with dynamic project showcase and responsive design.

**Live Site:** [mjkerrison.github.io](https://mjkerrison.github.io)

## 🏗️ Architecture & Tech Stack

### **Design Philosophy**
- **Custom HTML/CSS/JS** - No frameworks for maximum control and performance
- **GitHub Pages compatible** - No build process required
- **Brand-first design** - Custom styling matching existing brand palette
- **Mobile-responsive** - Professional experience across all devices

### **Key Technologies**
- **HTML5** - Semantic structure with accessibility features
- **CSS3** - Custom styling with CSS variables for brand consistency  
- **Vanilla JavaScript** - Dynamic content loading and interactions
- **Glide.js** - Professional carousel library (~23kb)
- **Google Fonts** - Lato (headings) + Roboto (body text)

### **Brand Colors**
```css
--dark-slate-grey: #335c67
--orange: #F3A41F  
--turquoise: #1dd3b0
--dark-purple: #372549
--african-violet: #a675a1
--cambridge-blue: #94A89A
```

## 🚀 Development Setup

### **⚠️ IMPORTANT: Local Development Server Required**

The site uses `fetch()` to load `projects.json`, which requires HTTP protocol due to CORS restrictions.

**Option 1: Use the provided script**
```bash
./start-server.sh  # User-created bash script
```

**Option 2: Manual Python server**
```bash
python -m http.server 8000
# Then visit: http://localhost:8000
```

**Option 3: Any other local server**
```bash
# Node.js
npx http-server

# PHP  
php -S localhost:8000

# VS Code Live Server extension
```

### **File Structure**
```
├── index.html              # Main page
├── style.css              # All styling  
├── projects.json          # Dynamic project data
├── assets/
│   └── Proof logo text.svg # Brand logo
├── brand reference/       # Brand assets & colors
├── DEVELOPMENT.md        # This file
└── start-server.sh       # Local development helper
```

## 📱 Key Features

### **Dynamic Project System**
- **JSON-driven** - All projects in `projects.json`
- **Multiple templates** - Google Docs, GitHub, metrics, confidential
- **Auto-generation** - Cards, navigation bullets, carousel all dynamic
- **Easy maintenance** - Add/remove/reorder projects by editing JSON

### **Responsive Design**
- **Desktop** - Horizontal nav, side carousel arrows
- **Mobile** - Hamburger menu, touch-friendly carousel
- **Smooth transitions** - Professional animations throughout

### **Professional Carousel**
- **Glide.js powered** - Battle-tested, accessible
- **Auto-play** - 6-second intervals with hover-pause
- **Dual navigation** - Side arrows + dot indicators
- **Touch/swipe support** - Native mobile gestures

## 🐛 Known Issues & Future Improvements

### **🔴 Mobile Overflow Issue**
**Screens < 372px** (iPhone SE, budget Android)
- **Problem:** Logo + hamburger + padding exceeds viewport width
- **Impact:** ~2-5% of users, creates horizontal scroll
- **Status:** Deferred until traffic justifies fix
- **Solution:** Add `@media (max-width: 372px)` with tighter padding/smaller logo

### **🔶 Technical Gotchas**

#### **JSON Loading Requires Server**
- `fetch('projects.json')` fails when opening HTML directly in browser
- **Always use local server for development**
- Production (GitHub Pages) works fine - serves over HTTP

#### **SVG Logo Sizing**
- Logo height set to 5rem for good visual balance
- May need adjustment if brand guidelines change
- Controlled via `.logo-svg` class

#### **Carousel Arrow Positioning** 
- Uses 50px padding on carousel container for arrow space
- Arrows positioned absolutely in these "gutters"
- Mobile hides arrows to prevent overlap

### **🔮 Future Enhancements**

#### **High Priority**
- [ ] Google Analytics integration
- [ ] Contact form with backend
- [ ] SEO optimization (meta tags, structured data)

#### **Medium Priority**  
- [ ] Blog section for thought leadership
- [ ] Case study detail pages
- [ ] Testimonials section
- [ ] Performance optimization (lazy loading, CDN)

#### **Low Priority**
- [ ] Dark mode toggle
- [ ] Animation on scroll
- [ ] Multi-language support
- [ ] Advanced project filtering

## 🎨 Design System

### **Typography Scale**
- **Hero Title:** `clamp(2.5rem, 5vw, 4rem)`
- **Section Headers:** `2.5rem` desktop, `2rem` mobile  
- **Body Text:** `1.1rem` with `1.7` line-height
- **UI Elements:** `0.9rem` for tags, `1.3rem` for buttons

### **Spacing System**
- **Section Padding:** `6rem` desktop, `4rem` mobile
- **Card Padding:** `2rem`
- **Element Gaps:** `2rem` desktop, `1rem` mobile

### **Component Patterns**
- **Cards:** White background, subtle shadow, turquoise left border
- **Buttons:** Orange primary, dark-slate-grey secondary  
- **Hover Effects:** `translateY(-5px)` + increased shadow
- **Transitions:** `0.3s ease` for most interactions

## 🚢 Deployment

### **GitHub Pages**
1. Push to `main` branch
2. Auto-deployed to `mjkerrison.github.io`
3. No build process required

### **Custom Domain** (Future)
1. Add CNAME file with domain
2. Configure DNS A/CNAME records
3. Enable HTTPS in GitHub Pages settings

## 📊 Performance Notes

### **Current Metrics** (Estimated)
- **Page Size:** ~150kb total
- **Load Time:** <2 seconds on 3G
- **Lighthouse Score:** 95+ (Performance, Accessibility, Best Practices)

### **Optimization Opportunities**
- Image optimization (logo is SVG - already optimal)
- CSS minification for production
- JavaScript minification
- Font loading optimization

## 🔧 Development Workflow

### **Adding New Projects**
1. Edit `projects.json`
2. Use appropriate template type (`gdocs`, `github`, `metrics`, `confidential`)
3. Test locally with server
4. Commit and push

### **Styling Changes**  
1. Use CSS variables for brand consistency
2. Test responsive breakpoints (320px, 375px, 768px, 1200px)
3. Verify mobile hamburger menu functionality

### **Brand Updates**
1. Update CSS variables in `:root`
2. Replace logo SVG in `assets/`
3. Update brand reference materials

---

## 📞 Support

For questions about this codebase, refer to:
- This documentation
- Git commit history for context
- `projects.json` structure for content management

**Built with ❤️ using modern web standards and professional development practices.**