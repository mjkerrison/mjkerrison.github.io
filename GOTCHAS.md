# Technical Gotchas & Quick Fixes

## 🚨 Critical Issues

### **JSON Loading Fails Locally**
**Problem:** `fetch('projects.json')` returns CORS error when opening `index.html` directly  
**Cause:** Browser blocks file:// protocol requests for security  
**Solution:** Always use local server: `./start-server.sh` or `python -m http.server`  
**Sign:** Carousel appears empty, console shows fetch error

### **Mobile Horizontal Scroll**
**Problem:** Layout overflows on screens < 372px (iPhone SE)  
**Affected:** Logo + hamburger exceeds viewport width  
**Impact:** ~2-5% of users, mostly legacy devices  
**Status:** Deferred - ship first, fix if needed  
**Fix:** `@media (max-width: 372px)` with tighter padding

## ⚠️ Common Mistakes

### **Project JSON Format**
**Wrong:**
```json
{
  "type": "link",  // Invalid type
  "url": "..."     // Missing structure
}
```
**Right:**
```json
{
  "type": "gdocs",
  "link": {
    "url": "...",
    "text": "...",
    "subtitle": "..."
  }
}
```

### **Glide.js Initialization**
**Problem:** Carousel breaks if initialized before DOM/JSON loads  
**Solution:** Always initialize in callback after projects are rendered  
**Code:** `this.initializeGlide()` called AFTER `this.renderProjects()`

### **Mobile Menu Z-Index**
**Problem:** Menu appears behind other content  
**Solution:** Nav menu has `z-index: 1000`, ensure no higher values elsewhere  
**Watch:** Carousel controls, modals, tooltips

## 🔧 Development Workflow

### **Testing Checklist**
- [ ] Desktop navigation works
- [ ] Mobile hamburger menu opens/closes  
- [ ] Carousel arrows and dots functional
- [ ] All project types render correctly
- [ ] Smooth scrolling to sections works
- [ ] Logo displays properly

### **Responsive Breakpoints**
- **320px** - Breaks (acceptable)
- **375px** - Works perfectly  
- **768px** - Mobile/desktop transition
- **1200px** - Container max-width

### **Browser Compatibility**
- **Modern browsers** - Full support
- **IE11** - Not supported (uses modern JS)
- **Safari** - Tested, works
- **Mobile browsers** - Tested, works

## 📱 Mobile-Specific Issues

### **Hamburger Animation**
**Problem:** Lines don't rotate properly  
**Cause:** Missing `transform-origin: center`  
**Check:** Hamburger lines should form clean X when active

### **Touch Scrolling**
**Problem:** Carousel doesn't respond to swipe  
**Solution:** Glide.js handles this automatically  
**Note:** If broken, check Glide initialization

### **Viewport Meta Tag**
**Critical:** `<meta name="viewport" content="width=device-width, initial-scale=1.0">`  
**Without:** Site appears zoomed out on mobile  
**Already included:** ✅ Present in `index.html`

## 🎨 Styling Gotchas

### **CSS Variables**
**Always use:** `var(--brand-color)` instead of hex codes  
**Defined in:** `:root` selector  
**Benefits:** Consistent theming, easy updates

### **Flexbox Centering**
**Container:** `.projects-glide` uses `margin: 0 auto`  
**Content:** Cards use `display: flex; flex-direction: column`  
**Mobile:** Ensure no fixed widths break layout

### **Smooth Scrolling**
**Enabled:** `html { scroll-behavior: smooth; }`  
**Issue:** May be overridden by user preferences  
**Fallback:** JavaScript smooth scroll if needed

---

## 🆘 Emergency Fixes

### **Site Completely Broken**
1. Check browser console for errors
2. Verify local server is running  
3. Clear browser cache
4. Check `projects.json` syntax

### **Projects Don't Load**
1. Confirm server serves JSON files
2. Check network tab for 404/CORS errors
3. Validate JSON syntax
4. Check JavaScript console

### **Mobile Layout Broken**
1. Verify viewport meta tag
2. Test responsive breakpoints  
3. Check hamburger menu JavaScript
4. Clear mobile browser cache

**Last Updated:** 2025-01-14  
**Next Review:** When issues arise or major changes made