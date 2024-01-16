# RPG-Style Portfolio Website

## Introduction
Welcome to the GitHub repository of my RPG-style portfolio website. This project is an interactive web application designed as a RPG game to showcase my journey from Marketing to Full Stack Development. The site is built using Svelte, SvelteKit, TailwindCSS, NES.CSS and Firebase for backend services.

## Features
- **Svelte & SvelteKit:** Utilizes the reactive and efficient framework for building responsive web interfaces.
- **Firebase:** Backend integration for features like blog posts, contact forms, and project data.

## Project Structure
```
/src
  /components - Reusable Svelte components
  /routes - SvelteKit pages representing different sections of the map
  /threejs - Three.js components and utilities
```

## Getting Started

### Prerequisites
- Node.js (LTS version recommended)
- Git
- Firebase account (for backend services)

### Installation
1. **Clone the repository**
   ```bash
   git clone https://github.com/JordanRobo/portfolio.git
   ```

2. **Navigate to the project directory**
   ```bash
   cd portfolio
   ```

3. **Install dependencies**
   ```bash
   npm install
   ```

4. **Set up Firebase**
   - Create a new project in Firebase.
   - Configure Firestore, Authentication, and other required services.
   - Add your Firebase project configurations to a `.env` file.

5. **Run the development server**
   ```bash
   npm run dev
   ```

## Deployment
- The project is ready for deployment to platforms like Vercel, Netlify, or Firebase Hosting.
- Make sure to set environment variables in your hosting provider.