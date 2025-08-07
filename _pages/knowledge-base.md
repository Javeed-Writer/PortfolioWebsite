---
layout: page
title: "Knowledge Base"
permalink: /knowledge-base/
---

<div style="text-align: center; margin-bottom: 3rem;">
    <h1 style="font-size: 2.5rem; color: #2c3e50; margin-bottom: 1rem;">
        <i class="fas fa-database" style="color: #3498db; margin-right: 0.5rem;"></i>
        Knowledge Base
    </h1>
    <p style="font-size: 1.2rem; color: #666; max-width: 800px; margin: 0 auto;">
        In-depth articles, platform overviews, and comprehensive documentation covering complex technical concepts, 
        system architectures, and detailed explanations of enterprise-level solutions.
    </p>
</div>

<div class="portfolio-grid">
    {% for article in site.knowledge_base %}
        <div class="portfolio-card">
            <div class="card-icon">
                <i class="fas fa-database"></i>
            </div>
            <h3 class="card-title">{{ article.title }}</h3>
            
            {% if article.date or article.author %}
            <div class="card-meta">
                {% if article.author %}
                    <span><i class="fas fa-user"></i> {{ article.author }}</span>
                {% endif %}
                {% if article.date %}
                    <span><i class="fas fa-calendar"></i> {{ article.date | date: "%b %Y" }}</span>
                {% endif %}
            </div>
            {% endif %}
            
            <p class="card-description">
                {{ article.description | default: article.excerpt | default: "In-depth knowledge base article covering technical concepts, system architecture, and comprehensive platform documentation." | strip_html | truncate: 150 }}
            </p>
            
            {% if article.tags %}
                <div class="card-tags">
                    {% for tag in article.tags %}
                        <span class="card-tag">{{ tag }}</span>
                    {% endfor %}
                </div>
            {% endif %}
            
            <div class="card-footer">
                <a href="{{ article.url | relative_url }}" class="btn btn-primary">
                    <i class="fas fa-eye"></i> Read Article
                </a>
            </div>
        </div>
    {% endfor %}
    
    <!-- Show placeholder if no articles or few articles -->
    {% if site.knowledge_base.size == 0 or site.knowledge_base.size < 3 %}
        <div class="portfolio-card placeholder-card">
            <div class="card-icon">
                <i class="fas fa-plus-circle"></i>
            </div>
            <h3 class="card-title">More Articles Coming Soon</h3>
            <p class="card-description">
                Additional knowledge base articles and in-depth technical documentation are currently being developed. 
                Check back soon for comprehensive coverage of various platforms and technologies.
            </p>
            <div class="card-tags">
                <span class="card-tag" style="background: #f8f9fa; color: #6c757d;">Coming Soon</span>
            </div>
            <div class="card-footer">
                <a href="{{ '/contact/' | relative_url }}" class="btn btn-outline" style="border-color: #bdc3c7; color: #7f8c8d;">
                    <i class="fas fa-envelope"></i> Request Topics
                </a>
            </div>
        </div>
    {% endif %}
</div>

<div style="background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%); color: white; padding: 3rem 2rem; border-radius: 12px; margin-top: 3rem;">
    <div style="text-align: center; margin-bottom: 2rem;">
        <h3 style="margin-bottom: 1rem; font-size: 1.8rem;">
            <i class="fas fa-book-open" style="color: #f39c12; margin-right: 0.5rem;"></i>
            Deep-Dive Documentation
        </h3>
        <p style="margin-bottom: 2rem; opacity: 0.9; max-width: 700px; margin-left: auto; margin-right: auto;">
            Knowledge base articles go beyond basic how-to guides. They provide comprehensive understanding of complex systems, 
            architectural decisions, and the reasoning behind implementation choices.
        </p>
    </div>
    
    <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 2rem;">
        <div style="background: rgba(255,255,255,0.1); padding: 1.5rem; border-radius: 8px; text-align: center;">
            <div style="font-size: 2.5rem; margin-bottom: 1rem;">🏗️</div>
            <h4 style="margin-bottom: 0.5rem;">System Architecture</h4>
            <p style="opacity: 0.8; font-size: 0.9rem;">Detailed explanations of how complex systems are designed and organized</p>
        </div>
        <div style="background: rgba(255,255,255,0.1); padding: 1.5rem; border-radius: 8px; text-align: center;">
            <div style="font-size: 2.5rem; margin-bottom: 1rem;">🔍</div>
            <h4 style="margin-bottom: 0.5rem;">Technical Concepts</h4>
            <p style="opacity: 0.8; font-size: 0.9rem;">In-depth coverage of technical concepts and implementation details</p>
        </div>
        <div style="background: rgba(255,255,255,0.1); padding: 1.5rem; border-radius: 8px; text-align: center;">
            <div style="font-size: 2.5rem; margin-bottom: 1rem;">📋</div>
            <h4 style="margin-bottom: 0.5rem;">Best Practices</h4>
            <p style="opacity: 0.8; font-size: 0.9rem;">Industry standards and recommended approaches for implementation</p>
        </div>
    </div>
</div>

<div style="background: #f8f9fa; padding: 2rem; border-radius: 8px; margin-top: 3rem; text-align: center;">
    <h3 style="color: #2c3e50; margin-bottom: 1rem;">
        <i class="fas fa-lightbulb" style="color: #f39c12; margin-right: 0.5rem;"></i>
        Looking for Specific Technical Documentation?
    </h3>
    <p style="color: #666; margin-bottom: 1.5rem; max-width: 600px; margin-left: auto; margin-right: auto;">
        Need comprehensive documentation for your platform, detailed technical overviews, or in-depth system explanations? 
        I specialize in creating knowledge base content that serves as a definitive reference for complex systems.
    </p>
    <div style="display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap;">
        <a href="/contact/" class="btn btn-primary">
            <i class="fas fa-envelope"></i> Discuss Your Needs
        </a>
        <a href="/" class="btn btn-outline">
            <i class="fas fa-user"></i> Learn More About My Work
        </a>
    </div>
</div>
