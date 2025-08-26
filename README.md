# Janno Portfolio

A beautiful, interactive portfolio website built with Svelte and Vite. Features a dynamic animated background with floating planets and particles, along with a modern commenting system using Discord OAuth2 authentication.

## ✨ Features

- **Interactive Animated Background**: Dynamic canvas animation with floating planets and particles
- **Modern UI Design**: Clean, responsive design with glassmorphism effects
- **Discord Authentication**: Secure commenting system using Discord OAuth2
- **Real-time Comments**: Users can leave comments after authenticating with Discord
- **Responsive Design**: Works perfectly on desktop and mobile devices
- **Fast Performance**: Built with Vite for optimal loading speeds

## 🚀 Live Demo

Visit the live portfolio: [Your Portfolio URL]

## 🛠️ Tech Stack

- **Frontend**: Svelte 4 + Vite
- **Styling**: CSS3 with Glassmorphism effects
- **Authentication**: Discord OAuth2
- **Deployment**: Cloudflare Pages
- **Icons**: Lucide Svelte
- **Animation**: HTML5 Canvas

## 📦 Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/janno-portfolio.git
   cd janno-portfolio
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Set up Discord OAuth2:**
   - Follow the [Discord Setup Guide](./DISCORD_SETUP.md)
   - Create a Discord application in the [Discord Developer Portal](https://discord.com/developers/applications)
   - Configure OAuth2 settings and get your Client ID and Secret

4. **Update configuration:**
   - Replace `YOUR_DISCORD_CLIENT_ID` in `src/components/Comments.svelte`
   - Update `wrangler.toml` with your Discord credentials

5. **Run development server:**
   ```bash
   npm run dev
   ```

6. **Build for production:**
   ```bash
   npm run build
   ```

## 🌐 Deployment

### Deploy to Cloudflare Pages

1. **Install Wrangler CLI:**
   ```bash
   npm install -g wrangler
   ```

2. **Login to Cloudflare:**
   ```bash
   wrangler login
   ```

3. **Deploy functions:**
   ```bash
   wrangler deploy
   ```

4. **Deploy to Cloudflare Pages:**
   - Go to [Cloudflare Dashboard](https://dash.cloudflare.com)
   - Navigate to Pages
   - Create a new project
   - Connect your GitHub repository
   - Set build settings:
     - Framework preset: None
     - Build command: `npm run build`
     - Build output directory: `dist`
   - Add environment variables for Discord OAuth2

## 🔧 Configuration

### Discord OAuth2 Setup

The commenting system requires Discord OAuth2 authentication. See [DISCORD_SETUP.md](./DISCORD_SETUP.md) for detailed setup instructions.

### Environment Variables

Required environment variables for Discord authentication:

```env
DISCORD_CLIENT_ID=your_discord_client_id
DISCORD_CLIENT_SECRET=your_discord_client_secret
DISCORD_REDIRECT_URI=https://your-domain.com/auth/callback
```

## 📁 Project Structure

```
janno-portfolio/
├── src/
│   ├── components/
│   │   ├── Comments.svelte      # Discord authentication & comments
│   │   └── AuthCallback.svelte  # OAuth2 callback handler
│   ├── App.svelte               # Main application component
│   ├── main.js                  # Application entry point
│   └── app.css                  # Global styles
├── functions/
│   └── api/
│       └── auth.js              # Cloudflare Worker for OAuth2
├── dist/                        # Build output
├── _headers                     # Cloudflare Pages headers
├── _redirects                   # SPA routing configuration
├── wrangler.toml               # Cloudflare Workers configuration
└── DISCORD_SETUP.md            # Discord OAuth2 setup guide
```

## 🎨 Customization

### Colors and Styling

The portfolio uses a custom color scheme that can be easily modified in the CSS. Main color variables:

- Primary: `#8b5cf6` (Purple)
- Background: `#1a1a2e`, `#16213e`, `#0f3460` (Dark gradient)
- Text: `#E0D9D9` (Light gray)
- Accent: `#ec4899` (Pink)

### Animation Settings

Modify the animation behavior by adjusting values in `App.svelte`:

- Planet count and properties
- Particle generation rate
- Animation speed and physics

### Comment System

The commenting system includes:

- Discord OAuth2 authentication
- Real-time comment posting
- User avatars and information
- Timestamp formatting
- Responsive design

## 🔒 Security Features

- **OAuth2 Authentication**: Secure Discord login
- **CORS Protection**: Proper CORS headers in API
- **Environment Variables**: Sensitive data protection
- **Input Validation**: Comment content validation
- **Rate Limiting**: Built-in protection against spam

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Commit your changes: `git commit -am 'Add feature'`
4. Push to the branch: `git push origin feature-name`
5. Submit a pull request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Svelte](https://svelte.dev/) for the amazing framework
- [Vite](https://vitejs.dev/) for the build tool
- [Lucide](https://lucide.dev/) for the beautiful icons
- [Discord](https://discord.com/) for OAuth2 authentication
- [Cloudflare](https://cloudflare.com/) for hosting and Workers

## 📞 Contact

- **GitHub**: [@janno30](https://github.com/janno30)
- **Discord**: [Your Discord Handle]
- **Portfolio**: [Your Portfolio URL]

---

Made with ❤️ by Janno
