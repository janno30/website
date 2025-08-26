# Ushie Portfolio

A personal portfolio website built with Svelte that matches the design from the provided image.

## Features

- Responsive design that works on desktop and mobile
- CSS-only character illustration
- Interactive buttons with hover effects
- Modern, clean UI with dark theme
- Built with Svelte and Vite

## Getting Started

### Prerequisites

- Node.js (version 16 or higher)
- npm or yarn

### Installation

1. Clone or download this project
2. Install dependencies:
   ```bash
   npm install
   ```

### Development

To start the development server:

```bash
npm run dev
```

The website will be available at `http://localhost:5173`

### Building for Production

To build the project for production:

```bash
npm run build
```

To preview the production build:

```bash
npm run preview
```

## Project Structure

```
├── src/
│   ├── App.svelte      # Main application component
│   └── main.js         # Application entry point
├── index.html          # HTML template
├── package.json        # Project dependencies and scripts
├── vite.config.js      # Vite configuration
└── svelte.config.js    # Svelte configuration
```

## Customization

You can easily customize the website by modifying the following in `src/App.svelte`:

- **Text content**: Update the greeting, description, and social information
- **Colors**: Modify the CSS custom properties for different color schemes
- **Character illustration**: Adjust the CSS for the character's appearance
- **Links**: Add actual URLs to the buttons for GitHub, Discord, and donation links

## Technologies Used

- **Svelte**: Frontend framework
- **Vite**: Build tool and development server
- **Lucide Svelte**: Icon library
- **Inter Font**: Typography from Google Fonts
