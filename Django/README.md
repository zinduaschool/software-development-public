 # Django Course
Official Curriculum for Django Course in the Software Engineering Program.

## Course Objectives

By the end of this course, learners will be able to:

- **Build and run Django applications**
  - Set up a Django project and run it on the development server
  - Create and configure views and URLs
  - Develop view functions and class-based views
  - Map URLs to views to create a navigable web app

- **Work with Django templates**
  - Use template inheritance, tags, and filters to create dynamic HTML pages

- **Design and manage databases**
  - Create models with fields and relationships
  - Use migrations to structure a PostgreSQL database
  - Understand and visualize database schemas
  - Use the Django admin interface to manage application data

- **Implement user interactions**
  - Create web forms for collecting and validating user data
  - Handle image uploads securely using Cloudinary
  - Display success/error messages and manage form errors

- **Implement authentication and authorization**
  - Authenticate users with signup, login, logout, and password reset
  - Create custom authentication models and backends
  - Enable social authentication (Google) and two-factor authentication (2FA)
  - Implement Role-Based Access Control (RBAC) to protect views, templates, and APIs

- **Develop and test APIs using Django REST Framework (DRF)**
  - Create function-based and class-based APIs
  - Use serializers, viewsets, and routers
  - Implement API authentication and object-level permissions
  - Test APIs manually with Postman and automate testing
  - Document APIs using OpenAPI/Swagger
  - Integrate third-party APIs like Google Maps and M-Pesa Daraja
  - Apply API security best practices

- **Write tests and implement TDD**
  - Write unit, model, view, and form tests
  - Conduct integration testing with mocks and coverage reports
  - Apply Test-Driven Development (TDD) to ensure code quality

- **Master advanced Django concepts**
  - Optimize databases with advanced ORM queries and indexing
  - Extend functionality with Custom Middleware and Signals
  - Implement caching and asynchronous background tasks using Celery
  - Build real-time features using Django Channels and WebSockets

- **Dockerize and deploy Django applications**
  - Containerize applications using Docker and Docker Compose
  - Set up basic CI/CD pipelines to automate testing and deployment
  - Deploy Django applications to cloud environments such as AWS
  - Manage production settings, static/media files, migrations, monitoring, and logging

## Curriculum Breakdown
### Week 1: Foundations of Django

#### Lesson 1: Set up a Django Project
  - Understanding django, History, Why Django
  - MVT
  - Environment Set-Up
  - First Django App
#### Lesson 2: Django Templates
  - Templating
  - Template Inheritance
  - Tags and Filters
#### Lesson 3: Django Object Relational Mapper
  - Why PostgreSQL (vs SQLite)
  - PostgreSQL Installation and configuration
  - Models, Fields, Relationships
  - Migrations
  - Understanding the Database Schema
  - Common Database Design Mistakes
  - Admin Interface
  - Display Model Data
#### Lesson 4: Static Files, Styling, Debugging and Deployment
  - Adding Tailwind
  - Custom CSS and Images
  - Debugging
  - Render Deployment
#### **Project:** Calorie Tracker

### Week 2: SQL and Authentication
#### Lesson 1 Title: SQL Basics
  - Understanding SQL
  - Basic Operations
  - Tables
  - Tables Operations
  - Clauses and Operations
  - Joins
  - Understanding Django ORM
#### Lesson 2 Title: Forms, Uploading Images and Error Messages
  - Django Forms
  - Custom Forms
  - Crispy Forms
  - Styling Forms
  - Uploading Images with Cloudinary
  - Messages and 404 Error
  - Securing Credentials with Python Decouple
#### Lesson 3 Title: Django Authentication
  - Built-in Auth 
    - Login
    - Registration
    - Logout
    - Password Reset
  - Custom Auth
    - Custom Model
    - Forms and Backends
    - Auth Templates
    - Social Authentication - Google
    - Two-Factor Authentication (2FA)
#### Lesson 4 Title: Role Based Authentication
  - Understanding RBAC(Auth vs Authorization, Why RBAC is needed, Common roles - Admin, User, Editor), Permissions and Groups, Demo
  - RBAc Model
  - Forms and Templates
  - Protecting Views and Templates with Roles
#### **Project:** Photos App

### Week 3: Django Rest Framework
#### Lesson 1 Title: Creating APIs with DRF
  - Introduction to APIs in Django
  - Function-Based APIs with DRF
  - Serializers
  - Class-Based APIs (APIView & Generic Views)
  - ViewSets and Routers
  - API Authentication
  - RBAC APIS
    - RBAC concepts
    - Authentication
    - Global permissions
    - Role-based permissions
    - Object-level permissions
    - Endpoint protection
    - Testing
#### Lesson 2 Title: API Testing and Documentation 
  - Why testing & docs matter
  - Manual API Testing (Postman/Curl)
  - Automated API Testing with DRF
  - Documenting APIs with OpenAPI & Swagger
  - Securing & Maintaining API Documentation
#### Lesson 3 Title: API Integration
  - Third-party API concepts
  - Google Maps integration
  - Geolocation use cases
  - M-Pesa Daraja setup
  - STK Push payments
  - Callbacks & confirmations
  - Error handling & testing
#### Lesson 4 Title: API Security
  - API Threats & Security Fundamentals
  - Authentication & Token Security
  - Authorization & Permission Hardening
  - Input Validation & Data Exposure Control
  - Rate Limiting, Throttling & Abuse Prevention
  - Secure Headers, CORS & HTTPS
  - Logging, Monitoring & Incident Response
#### **Project:** Social Media App

### Week 4: Django Tests and Deployment
#### Lesson 1 Title: Writing and Running Tests
  - Unit Tests
  - Model Tests
  - View Tests 
  - Form Tests
  - Integration Tests
  - Mocks
  - Coverage
#### Lesson 2 Title: Test-Driven Development
  - Understanding the TDD Philosophy (Red-Green-Refactor)
  - Configuring Pytest and pytest-django
  - Using Factory Boy for Test Data Generation
  - Writing Failing Tests First (Models, Views, APIs)
  - Implementing Code and Refactoring
  - Mocking and Patching External Services
#### Lesson 3: Docker & CI/CD
  - Introduction to Docker
  - Containerizing a Django App
  - Docker Compose for Multi-Container Apps
  - Introduction to CI/CD
  - Basic CI/CD Pipeline for Django
  - Docker + CI/CD Integration
#### Lesson 4 Title: Deployment(AWS)
  - Deployment Overview & Concepts
  - Preparing Django for Production
  - Handling Static & Media Files
  - Database Migrations in Production
  - Deploying Django to AWS (Demo)
  - Production Monitoring & Logging
#### **Project:** Social Media App(Tests, Dockerize app and Deploy to AWS)
### Week 5: Advanced Topics, Conclusion & Next Steps
#### Lesson 1: Advanced ORM & Performance
  - QuerySet Optimization (`select_related` vs `prefetch_related`)
  - Custom Model Managers and QuerySets
  - Database Indexing, Constraints, and Transactions
  - Profiling Queries with Django Debug Toolbar
#### Lesson 2: Middleware & Signals
  - The Django Request/Response Cycle
  - Creating and Applying Custom Middleware
  - Utilizing Built-in Signals (`post_save`, `pre_delete`)
  - Defining and Dispatching Custom Signals
#### Lesson 3: Asynchronous Tasks & Caching
  - Introduction to Redis and Memcached
  - Implementing Caching Strategies (Per-view, Template, Data)
  - Setting up Celery for Background Tasks
  - Offloading Email Delivery and Heavy Task Processing
#### Lesson 4: WebSockets & Next Steps
  - Introduction to Django Channels and WebSockets
  - Building Real-time Features (e.g., Live Chat, Notifications)
  - Course Summary and Final Review
- **Capstone Projects:**
  - Task Management system
  - Law Firm System
  - Job Board Platform
  - Fitness Tracking system
  - Real Estate Management system
- **Optional Bonus Lessons:**
  - Deploy a Django app to AWS, GCP
  - Manage APIs with WSO2

## Course Contributors
### Author
Ezekiel Kibiego kibiezekiel@gmail.com
### Acknowledgements
- [Django Documentation](https://docs.djangoproject.com/)
- [Django REST Framework](https://www.django-rest-framework.org/)
- **Books & Resources:** *Django for Beginners*, *Django for APIs*, *Django for Professionals* by William S. Vincent, and *Two Scoops of Django* by Daniel & Audrey Roy Greenfeld.
