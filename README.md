#  Gym Progress Tracking & Social Platform

> **Note:** This repository is a showcase of my Bachelor's Thesis. The full source code is private to comply with university academic integrity policies.

##  Overview
A full-stack social media and progress-tracking application tailored for gym enthusiasts. It allows users to manage training plans, posts, comments and groups. 

##  Tech Stack
Based on the official Laravel Livewire starter kit, customized for relational data and interactive UIs.

**Core Frameworks**
* **Backend:** PHP 8.3, Laravel 13
* **Frontend:** Livewire 4, Alpine.js, TailwindCSS
* **UI Components:** Livewire Flux, Livewire PowerGrid

**Security**
* **Authorization:** Spatie Laravel Permission

**Media & Notifications**
* **File Management:** Spatie Laravel MediaLibrary
* **Alerts:** PHP Flasher & SweetAlert

**Tools**
* **Dev Tools:** Laravel Sail (Docker), Debugbar

##  Key Features
* **Role-Based Access Control:** Secure user roles (User, Group author, Group moderator, Admin) managed via Spatie Permissions.
* **Dynamic Data Grids:** Integrated Livewire PowerGrid to display highly interactive, searchable, and filterable tables.
* **Media Management:** Seamless profile picture and group banner uploads utilizing Spatie MediaLibrary.
* **Interactive Notifications:** Real-time user feedback using PHP Flasher and SweetAlert integration.

##  How It's Made: Technical Deep Dives

### 1. Preventing Race Conditions in Moderation
**The Problem:**
A concurrency issue existed in the admin panel where multiple administrators might attempt to review and resolve the same report simultaneously, leading to conflicting database states.

**The Solution:**
Implemented an assignment mechanism for the moderation queue. Before a report can be resolved, it must be claimed by an admin. This assigns the ticket exclusively to them, ensuring only that specific admin can dictate the outcome of the report.

### 2. Component-Driven UI Refactoring (DRY Principle)
**The Problem:**
Initially, there was a high level of code duplication when rendering post feeds across different contexts (e.g., global feed, followed groups, user profiles), which increased maintenance overhead.

**The Solution:**
Refactored the UI into dynamic, reusable Blade and Livewire components. The core post-feed logic was extracted into a single component that accepts context-specific query filters. This strictly adheres to the DRY principle and ensures uniform behavior across the application.

### 3. Database Normalization via Polymorphism
**The Problem:**
The database schema was becoming bloated and complex due to the creation of separate tables for likes, reports, and media for every single entity (e.g., `post_likes`, `comment_likes`, `group_media`).

**The Solution:**
Utilized Laravel's Polymorphic Relationships (`morphTo` / `morphMany`). By refactoring to shared polymorphic `media`, `likes`, and `reports` tables, I significantly reduced schema complexity and eliminated redundant tables.

### 4. Scoped Authorization for Groups
**The Problem:**
The platform required a granular access control system where user roles (like Author or Moderator) are scoped strictly to specific groups, rather than granting global administrative privileges across the entire application.

**The Solution:**
Leveraged Spatie Laravel Permission's scoped role functionality to bind roles directly to specific group models. This ensures strict tenant isolation—meaning a user can possess absolute Moderator privileges within one group while remaining a standard, unprivileged user in the rest of the application.

##  Future Roadmap

* **Test Coverage:** Implementing a comprehensive automated testing suite (Unit and Feature tests) using the PEST framework to ensure platform stability.
* **Mobile Ecosystem:** Expanding the ecosystem by developing iOS and Android applications using **NativePHP for Mobile**. This will allow the platform to run the Laravel backend natively on-device, leveraging hardware features (like push notifications and local storage) for a seamless mobile gym-tracking experience.
