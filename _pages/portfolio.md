---
layout: default
title: "Portfolio"
permalink: /portfolio/
---

<div class="portfolio-container">
    <!-- Left Pane: Navigation -->
    <aside class="portfolio-sidebar">
        <div class="sidebar-header">
            <h3>Samples</h3>
        </div>
        
        <div class="sidebar-content">
            <!-- Developer Docs Category -->
            <div class="category-group">
                <button class="category-toggle" data-category="developer_guides">
                    <i class="fas fa-chevron-down"></i>
                    <span>Developer Docs</span>
                </button>
                <ul class="guide-list" id="developer_guides">
                    {% for guide in site.developer_guides %}
                        <li><a href="#" class="guide-link" data-guide="{{ guide.url | relative_url }}" data-title="{{ guide.title }}">{{ guide.title }}</a></li>
                    {% endfor %}
                </ul>
            </div>
            
            <!-- User Guides Category -->
            <div class="category-group">
                <button class="category-toggle" data-category="user_guides">
                    <i class="fas fa-chevron-down"></i>
                    <span>User Guides</span>
                </button>
                <ul class="guide-list" id="user_guides">
                    {% for guide in site.user_guides %}
                        <li><a href="#" class="guide-link" data-guide="{{ guide.url | relative_url }}" data-title="{{ guide.title }}">{{ guide.title }}</a></li>
                    {% endfor %}
                </ul>
            </div>
            
            <!-- KB Articles Category -->
            <div class="category-group">
                <button class="category-toggle" data-category="knowledge_base">
                    <i class="fas fa-chevron-down"></i>
                    <span>KB Articles</span>
                </button>
                <ul class="guide-list" id="knowledge_base">
                    {% for article in site.knowledge_base %}
                        <li><a href="#" class="guide-link" data-guide="{{ article.url | relative_url }}" data-title="{{ article.title }}">{{ article.title }}</a></li>
                    {% endfor %}
                </ul>
            </div>
        </div>
    </aside>
    
    <!-- Center Pane: Content -->
    <main class="portfolio-content">
        <div id="guide-content" class="guide-content-wrapper">
            <div class="empty-state">
                <i class="fas fa-folder-open"></i>
                <h2>Select a Guide</h2>
                <p>Choose a guide from the left sidebar to view its content</p>
            </div>
        </div>
    </main>
    
    <!-- Right Pane: Table of Contents -->
    <aside class="portfolio-toc">
        <div class="toc-header">
            <h3>On This Page</h3>
        </div>
        <div id="table-of-contents" class="toc-content">
            <p style="color: #999; font-size: 0.9rem;">No content selected</p>
        </div>
    </aside>
</div>

<script id="portfolio-script">
document.addEventListener('DOMContentLoaded', function() {
    const guideContent = document.getElementById('guide-content');
    const tableOfContents = document.getElementById('table-of-contents');
    const categoryToggles = document.querySelectorAll('.category-toggle');
    const guideLinks = document.querySelectorAll('.guide-link');
    
    // Handle category toggle
    categoryToggles.forEach(toggle => {
        toggle.addEventListener('click', function() {
            const category = this.dataset.category;
            const list = document.getElementById(category);
            const icon = this.querySelector('i');
            
            list.classList.toggle('expanded');
            icon.classList.toggle('rotated');
        });
        
        // Expand only Developer Docs on load (first category)
        if (toggle.dataset.category === 'developer_guides') {
            toggle.click();
        }
    });
    
    // Handle guide links
    guideLinks.forEach(link => {
        link.addEventListener('click', function(e) {
            e.preventDefault();
            const guideUrl = this.dataset.guide;
            const guideTitle = this.dataset.title;
            
            // Fetch guide content
            fetch(guideUrl)
                .then(response => response.text())
                .then(html => {
                    // Parse HTML to extract content
                    const parser = new DOMParser();
                    const doc = parser.parseFromString(html, 'text/html');
                    const content = doc.querySelector('article.guide') || doc.querySelector('main') || doc.body;
                    
                    // Display content
                    guideContent.innerHTML = content.innerHTML;
                    
                    // Remove table of contents section from guide content
                    removeTocFromContent(guideContent);
                    
                    // Highlight active guide
                    guideLinks.forEach(g => g.classList.remove('active'));
                    this.classList.add('active');
                    
                    // Generate table of contents from headings in right pane
                    generateTableOfContents(guideContent);
                    
                    // Add smooth scroll behavior
                    addSmoothScroll();
                })
                .catch(error => {
                    guideContent.innerHTML = '<div class="error-message"><p>Error loading guide content. Please try again.</p></div>';
                    console.error('Error loading guide:', error);
                });
        });
    });
    
    // Remove table of contents from guide content
    function removeTocFromContent(container) {
        const headings = container.querySelectorAll('h1, h2, h3, h4, h5, h6');
        
        headings.forEach(heading => {
            // Check if heading contains "Table of Contents"
            if (heading.textContent.toLowerCase().includes('table of contents')) {
                // Find and remove the heading and the following list
                let currentElement = heading;
                
                // Remove the heading
                const headingToRemove = heading.parentElement || heading;
                
                // Look for following ul or ol elements
                let nextElement = heading.nextElementSibling;
                while (nextElement && (nextElement.tagName === 'UL' || nextElement.tagName === 'OL')) {
                    const elementToRemove = nextElement;
                    nextElement = nextElement.nextElementSibling;
                    elementToRemove.remove();
                }
                
                // Remove the heading itself
                if (heading.tagName.match(/^H[1-6]$/)) {
                    heading.remove();
                }
            }
        });
    }
    
    // Generate table of contents from headings (H1 and H2 only)
    function generateTableOfContents(container) {
        const headings = container.querySelectorAll('h1, h2');
        const toc = [];
        
        headings.forEach((heading, index) => {
            // Skip the first h1 (title)
            if (index === 0 && heading.tagName === 'H1') return;
            
            const level = parseInt(heading.tagName[1]);
            const text = heading.textContent;
            const id = heading.id || `heading-${index}`;
            
            if (!heading.id) {
                heading.id = id;
            }
            
            toc.push({
                level: level,
                text: text,
                id: id
            });
        });
        
        // Build HTML
        let html = '<nav class="toc-nav">';
        let currentLevel = 0;
        
        toc.forEach(item => {
            if (item.level > currentLevel) {
                html += '<ul class="toc-list">'.repeat(item.level - currentLevel);
                currentLevel = item.level;
            } else if (item.level < currentLevel) {
                html += '</ul>'.repeat(currentLevel - item.level);
                currentLevel = item.level;
            }
            
            html += `<li class="toc-item toc-level-${item.level}"><a href="#${item.id}" class="toc-link">${item.text}</a></li>`;
        });
        
        html += '</ul>'.repeat(currentLevel);
        html += '</nav>';
        
        tableOfContents.innerHTML = html || '<p style="color: #999; font-size: 0.9rem;">No headings found</p>';
        
        // Add click handlers to ToC links
        document.querySelectorAll('.toc-link').forEach(link => {
            link.addEventListener('click', function(e) {
                e.preventDefault();
                const target = document.getElementById(this.getAttribute('href').substring(1));
                if (target) {
                    target.scrollIntoView({ behavior: 'smooth', block: 'start' });
                }
            });
        });
    }
    
    // Add smooth scroll behavior for anchor links
    function addSmoothScroll() {
        const internalLinks = guideContent.querySelectorAll('a[href^="#"]');
        internalLinks.forEach(link => {
            link.addEventListener('click', function(e) {
                const href = this.getAttribute('href');
                const target = document.getElementById(href.substring(1));
                if (target) {
                    e.preventDefault();
                    target.scrollIntoView({ behavior: 'smooth', block: 'start' });
                }
            });
        });
    }
    
    // Load first guide on page load
    const firstGuideLink = document.querySelector('.guide-link');
    if (firstGuideLink) {
        firstGuideLink.click();
    }
});
</script>
