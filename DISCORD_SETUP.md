# Discord OAuth2 Setup Guide

This guide will help you set up Discord OAuth2 authentication for the commenting system on your portfolio.

## Step 1: Create a Discord Application

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications)
2. Click "New Application"
3. Give your application a name (e.g., "Janno Portfolio")
4. Click "Create"

## Step 2: Configure OAuth2 Settings

1. In your Discord application dashboard, go to the "OAuth2" section in the left sidebar
2. Click on "General"
3. Copy the "Client ID" - you'll need this later
4. Click on "OAuth2" → "Redirects"
5. Add your redirect URI: `https://your-domain.com/auth/callback`
   - Replace `your-domain.com` with your actual domain
   - For local development, you can use: `http://localhost:5173/auth/callback`
6. Click "Save Changes"

## Step 3: Get Your Client Secret

1. In the OAuth2 section, click on "General"
2. Click "Reset Secret" to generate a new client secret
3. Copy the client secret - you'll need this for the backend

## Step 4: Update Configuration Files

### Update `src/components/Comments.svelte`
Replace `YOUR_DISCORD_CLIENT_ID` with your actual Discord Client ID:

```javascript
const DISCORD_CLIENT_ID = '1234567890123456789'; // Your actual client ID
```

### Update `wrangler.toml`
Replace the placeholder values with your actual credentials:

```toml
[vars]
DISCORD_CLIENT_ID = "1234567890123456789"
DISCORD_CLIENT_SECRET = "your-actual-client-secret"
DISCORD_REDIRECT_URI = "https://your-domain.com/auth/callback"
```

## Step 5: Deploy to Cloudflare Pages

1. **Install Wrangler CLI:**
   ```bash
   npm install -g wrangler
   ```

2. **Login to Cloudflare:**
   ```bash
   wrangler login
   ```

3. **Deploy your functions:**
   ```bash
   wrangler deploy
   ```

4. **Deploy your frontend to Cloudflare Pages:**
   - Go to [Cloudflare Dashboard](https://dash.cloudflare.com)
   - Navigate to Pages
   - Create a new project
   - Connect your GitHub repository
   - Set build settings:
     - Framework preset: None
     - Build command: `npm run build`
     - Build output directory: `dist`
   - Deploy

## Step 6: Update Environment Variables

1. In your Cloudflare Pages dashboard, go to Settings → Environment variables
2. Add the following variables:
   - `DISCORD_CLIENT_ID`: Your Discord Client ID
   - `DISCORD_CLIENT_SECRET`: Your Discord Client Secret
   - `DISCORD_REDIRECT_URI`: Your redirect URI

## Step 7: Test the Authentication

1. Visit your deployed portfolio
2. Click on "Comments" to open the comments section
3. Click "Login with Discord"
4. You should be redirected to Discord for authorization
5. After authorizing, you should be redirected back to your portfolio
6. You should now be able to post comments

## Security Considerations

1. **Never expose your client secret in frontend code**
2. **Use environment variables for sensitive data**
3. **Implement rate limiting for comments**
4. **Add content moderation for comments**
5. **Consider implementing CSRF protection**

## Troubleshooting

### Common Issues:

1. **"Invalid redirect URI" error:**
   - Make sure the redirect URI in Discord matches exactly
   - Check for trailing slashes or protocol mismatches

2. **"Invalid client" error:**
   - Verify your Client ID is correct
   - Make sure your Discord application is properly configured

3. **CORS errors:**
   - Ensure your Cloudflare Worker is properly configured
   - Check that CORS headers are being sent

4. **Authentication not working:**
   - Verify all environment variables are set correctly
   - Check the browser console for errors
   - Ensure your domain is properly configured in Discord

## Advanced Features

### Adding Comment Moderation

You can enhance the commenting system by:

1. **Adding a moderation queue**
2. **Implementing keyword filtering**
3. **Adding user reputation system**
4. **Creating admin controls**

### Database Integration

For a production system, consider:

1. **Using Cloudflare D1 (SQLite) for comments storage**
2. **Implementing comment threading**
3. **Adding comment editing/deletion**
4. **Creating comment analytics**

## Support

If you encounter any issues:

1. Check the browser console for errors
2. Verify all configuration steps were completed
3. Ensure your Discord application is properly set up
4. Check Cloudflare Workers logs for backend errors

## Example Configuration

Here's a complete example of what your configuration should look like:

```javascript
// src/components/Comments.svelte
const DISCORD_CLIENT_ID = '1234567890123456789';
const DISCORD_REDIRECT_URI = 'https://janno-portfolio.pages.dev/auth/callback';
```

```toml
# wrangler.toml
[vars]
DISCORD_CLIENT_ID = "1234567890123456789"
DISCORD_CLIENT_SECRET = "abcdefghijklmnopqrstuvwxyz123456"
DISCORD_REDIRECT_URI = "https://janno-portfolio.pages.dev/auth/callback"
```

Remember to replace all placeholder values with your actual Discord application credentials!
