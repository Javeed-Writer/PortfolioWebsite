---
layout: page
title: "Developer Guides"
permalink: /developer-guides/
---

<div style="text-align: center; margin-bottom: 3rem;">
    <h1 style="font-size: 2.5rem; color: #2c3e50; margin-bottom: 1rem;">
        <i class="fas fa-code" style="color: #3498db; margin-right: 0.5rem;"></i>
        Developer Guides
    </h1>
    <p style="font-size: 1.2rem; color: #666; max-width: 800px; margin: 0 auto;">
        Comprehensive API documentation, technical references, and integration guides designed to help developers 
        implement and work with various platforms and services effectively.
    </p>
</div>

<div class="portfolio-grid">
    {% for guide in site.developer_guides %}
        <div class="portfolio-card">
            <div class="card-icon">
                <i class="fas fa-code"></i>
            </div>
            <h3 class="card-title">{{ guide.title }}</h3>
            
            {% if guide.date or guide.author %}
            <div class="card-meta">
                {% if guide.author %}
                    <span><i class="fas fa-user"></i> {{ guide.author }}</span>
                {% endif %}
                {% if guide.date %}
                    <span><i class="fas fa-calendar"></i> {{ guide.date | date: "%b %Y" }}</span>
                {% endif %}
            </div>
            {% endif %}
            
            <p class="card-description">
                {{ guide.description | default: guide.excerpt | default: "Comprehensive developer guide with technical documentation and implementation details." | strip_html | truncate: 150 }}
            </p>
            
            {% if guide.tags %}
                <div class="card-tags">
                    {% for tag in guide.tags %}
                        <span class="card-tag">{{ tag }}</span>
                    {% endfor %}
                </div>
            {% endif %}
            
            <div class="card-footer">
                <a href="{{ guide.url | relative_url }}" class="btn btn-primary">
                    <i class="fas fa-eye"></i> View Guide
                </a>
            </div>
        </div>
    {% endfor %}
    
    <!-- Show a message if no guides are available -->
    {% if site.developer_guides.size == 0 %}
        <div class="portfolio-card placeholder-card">
            <div class="card-icon">
                <i class="fas fa-plus-circle"></i>
            </div>
            <h3 class="card-title">Developer Guides Coming Soon</h3>
            <p class="card-description">
                Comprehensive API documentation and developer guides are currently being prepared. 
                These will include REST API references, GraphQL documentation, and integration tutorials.
            </p>
            <div class="card-tags">
                <span class="card-tag" style="background: #f8f9fa; color: #6c757d;">Coming Soon</span>
            </div>
            <div class="card-footer">
                <a href="{{ '/contact/' | relative_url }}" class="btn btn-outline" style="border-color: #bdc3c7; color: #7f8c8d;">
                    <i class="fas fa-envelope"></i> Request Documentation
                </a>
            </div>
        </div>
    {% endif %}
</div>

<div style="background: #f8f9fa; padding: 2rem; border-radius: 8px; margin-top: 3rem; text-align: center;">
    <h3 style="color: #2c3e50; margin-bottom: 1rem;">
        <i class="fas fa-lightbulb" style="color: #f39c12; margin-right: 0.5rem;"></i>
        Need Custom Documentation?
    </h3>
    <p style="color: #666; margin-bottom: 1.5rem;">
        Looking for API documentation, technical guides, or developer resources tailored to your specific needs? 
        I specialize in creating clear, comprehensive documentation that helps developers succeed.
    </p>
    <a href="{{ '/contact/' | relative_url }}" class="btn btn-primary">
        <i class="fas fa-envelope"></i> Get in Touch
    </a>
</div>
