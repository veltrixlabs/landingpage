# Customization Guide - Veltrix Labs Landing Page

## Quick Edits

### 1. Change Company Name
**File:** `index.html`

Find and replace:
```html
<span class="font-bold text-xl text-white">Veltrix Labs</span>
```

### 2. Update Contact Information
**File:** `index.html`, Footer section

```html
<!-- Email -->
hello@veltrix-labs.com

<!-- Phone (add if needed) -->
+1 (555) 123-4567

<!-- Address (add if needed) -->
San Francisco, CA 94105
```

### 3. Change Logo/Brand Icon
Replace this with your logo:
```html
<div class="w-10 h-10 bg-gradient-to-br from-cyan-400 to-blue-600 rounded-lg flex items-center justify-center font-bold text-white">
    VL
</div>
```

Option 1: Replace with text
```html
<img src="assets/logo.png" alt="Veltrix Labs" class="h-10 w-auto" />
```

Option 2: Use emoji
```html
<span class="text-2xl">⚡</span>
```

### 4. Customize Colors

**Primary Colors (Hero, Buttons):**
Look for these and replace:
```css
background: linear-gradient(135deg, #00d4ff 0%, #0099ff 50%, #6366f1 100%);
```

**Change to your brand colors:**
```css
/* Example: Purple & Pink */
background: linear-gradient(135deg, #ec4899 0%, #8b5cf6 50%, #6366f1 100%);
```

**Text colors:**
- Cyan: `#00d4ff` - Replace with primary
- Blue: `#0099ff` - Replace with secondary
- Indigo: `#6366f1` - Replace with accent

### 5. Update Hero Section

**Main Heading:**
```html
<h1 class="text-5xl md:text-7xl font-bold leading-tight">
    Building <span class="gradient-text">Intelligent Systems</span> for a <span class="gradient-text">Connected World</span>
</h1>
```

**Subheading:**
```html
<p class="text-lg md:text-xl text-gray-400 max-w-2xl mx-auto leading-relaxed">
    Enterprise-grade AI solutions, advanced messaging systems, and intelligent connectivity platforms...
</p>
```

### 6. Update Statistics

**Current stats:**
```html
<div class="text-3xl md:text-4xl font-bold text-cyan-400">10M+</div>
<div class="text-gray-400">Messages Processed Daily</div>
```

Change to your metrics:
```html
<div class="text-3xl md:text-4xl font-bold text-cyan-400">YOUR_NUMBER</div>
<div class="text-gray-400">YOUR_METRIC_NAME</div>
```

### 7. Customize Features Section

Each feature card:
```html
<div class="gradient-border card-hover">
    <div class="p-8 space-y-4">
        <div class="w-12 h-12 bg-gradient-to-br from-cyan-400 to-blue-600 rounded-lg flex items-center justify-center text-xl">
            ⚡  <!-- Change emoji -->
        </div>
        <h3 class="text-xl font-bold">Feature Title</h3>
        <p class="text-gray-400">Feature description goes here.</p>
    </div>
</div>
```

### 8. Update Testimonials/Case Studies

Replace each testimonial card:
```html
<div class="flex items-center space-x-2">
    <div class="w-10 h-10 rounded-full bg-gradient-to-br from-cyan-400 to-blue-600"></div>
    <div>
        <p class="font-semibold">Company Name</p>
        <p class="text-sm text-gray-400">Use case / Industry</p>
    </div>
</div>
<p class="text-gray-400">"Testimonial text goes here..."</p>
```

### 9. Modify Pricing Plans

```html
<span class="text-4xl font-bold">$99</span>
<span class="text-gray-400 text-sm">/month</span>
```

Update plan features:
```html
<ul class="space-y-3 text-gray-300 text-sm">
    <li>✓ Feature 1</li>
    <li>✓ Feature 2</li>
    <li>✗ Feature not included</li>
</ul>
```

### 10. Update Footer Links

Social links:
```html
<a href="https://twitter.com/veltrix-labs">Twitter</a>
<a href="https://linkedin.com/company/veltrix-labs">LinkedIn</a>
<a href="https://github.com/veltrix-labs">GitHub</a>
```

Footer navigation:
```html
<li><a href="https://your-blog.com">Blog</a></li>
<li><a href="https://docs.veltrix-labs.com">Docs</a></li>
```

## Advanced Customization

### Add Navigation Items

In the navbar section:
```html
<a href="#solutions" class="text-gray-300 hover:text-cyan-400 transition nav-link">Solutions</a>
<a href="#blog" class="text-gray-300 hover:text-cyan-400 transition nav-link">Blog</a>
```

Then create corresponding sections with `id="blog"` etc.

### Add New Sections

Template for new sections:
```html
<section id="your-section" class="relative py-32 px-6">
    <div class="max-w-7xl mx-auto">
        <div class="text-center mb-20">
            <h2 class="text-4xl md:text-5xl font-bold mb-4">Section Title</h2>
            <p class="text-gray-400 text-lg">Section description</p>
        </div>
        
        <!-- Your content here -->
    </div>
</section>
```

### Add Images

Create `assets/` folder and add images:
```bash
mkdir assets
cp your-image.jpg assets/
```

Use in HTML:
```html
<img src="assets/your-image.jpg" alt="Description" class="w-full rounded-lg" />
```

### Form Integration

To make the CTA buttons functional:
```html
<button onclick="openContactForm()" class="...">
    Schedule Demo
</button>
```

Add script:
```javascript
function openContactForm() {
    // Redirect to your form or email
    window.location.href = 'mailto:hello@veltrix-labs.com';
    // Or redirect to Calendly, Typeform, etc.
}
```

### Add Newsletter Signup

```html
<form id="newsletter" class="flex gap-2">
    <input type="email" placeholder="Enter your email" required class="flex-1 px-4 py-2 rounded-lg bg-slate-900 border border-slate-700 text-white" />
    <button type="submit" class="bg-cyan-500 text-white px-6 py-2 rounded-lg font-semibold">Subscribe</button>
</form>

<script>
document.getElementById('newsletter').addEventListener('submit', async (e) => {
    e.preventDefault();
    const email = e.target.querySelector('input[type="email"]').value;
    // Send to your backend
    console.log('Subscribe:', email);
});
</script>
```

## Font Changes

Edit in `<style>` section:
```css
@import url('https://fonts.googleapis.com/css2?family=YOUR_FONT:wght@300;400;600;700;800&display=swap');

h1, h2, h3 {
    font-family: 'YOUR_FONT', sans-serif;
}
```

## Animation Customization

Adjust animation delays in HTML:
```html
<div data-aos="fade-up" data-aos-delay="0">
```

Change `data-aos` values:
- `fade-up` - Fade and slide up
- `fade-down` - Fade and slide down
- `zoom-in` - Zoom in effect
- `flip-left` - Flip animation
- See AOS docs: https://michalsnik.github.io/aos/

## Performance Tips

1. **Compress images** (if adding):
   ```bash
   imagemin images/* --out-dir=assets
   ```

2. **Minify HTML** for production (optional):
   - Use online minifiers
   - Removes unnecessary whitespace

3. **Lazy load images**:
   ```html
   <img src="image.jpg" loading="lazy" alt="Description" />
   ```

---

**Need help?** Check DEPLOYMENT.md for deployment options.
