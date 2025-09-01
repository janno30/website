<script>
  import { onMount } from 'svelte';
  import { MessageCircle, Send, LogIn, LogOut, User, Clock } from 'lucide-svelte';
  
  // Discord OAuth2 configuration
  const DISCORD_CLIENT_ID = '1409762260771012629';
  const DISCORD_REDIRECT_URI = 'https://janno.pages.dev/auth/callback';
  const DISCORD_SCOPE = 'identify';
  
  let comments = [];
  let newComment = '';
  let user = null;
  let isLoading = false;
  let showComments = false;
  
  // Load comments from localStorage (in a real app, this would be from a database)
  onMount(() => {
    const savedComments = localStorage.getItem('portfolio-comments');
    if (savedComments) {
      comments = JSON.parse(savedComments);
    }
    
    // Check if user is already authenticated
    const savedUser = localStorage.getItem('discord-user');
    if (savedUser) {
      user = JSON.parse(savedUser);
    }
  });
  
  function saveComments() {
    localStorage.setItem('portfolio-comments', JSON.stringify(comments));
  }
  
  function handleDiscordLogin() {
    const authUrl = `https://discord.com/api/oauth2/authorize?client_id=${DISCORD_CLIENT_ID}&redirect_uri=${encodeURIComponent(DISCORD_REDIRECT_URI)}&response_type=code&scope=${DISCORD_SCOPE}`;
    window.location.href = authUrl;
  }
  
  function handleLogout() {
    user = null;
    localStorage.removeItem('discord-user');
  }
  
  function handleSubmitComment() {
    if (!newComment.trim() || !user) return;
    
    const comment = {
      id: Date.now(),
      text: newComment.trim(),
      user: {
        id: user.id,
        username: user.username,
        avatar: user.avatar,
        discriminator: user.discriminator
      },
      timestamp: new Date().toISOString(),
      likes: 0
    };
    
    comments.unshift(comment);
    saveComments();
    newComment = '';
  }
  
  function handleKeyPress(event) {
    if (event.key === 'Enter' && !event.shiftKey) {
      event.preventDefault();
      handleSubmitComment();
    }
  }
  
  function formatTime(timestamp) {
    const date = new Date(timestamp);
    const now = new Date();
    const diff = now - date;
    
    const minutes = Math.floor(diff / 60000);
    const hours = Math.floor(diff / 3600000);
    const days = Math.floor(diff / 86400000);
    
    if (minutes < 1) return 'Just now';
    if (minutes < 60) return `${minutes}m ago`;
    if (hours < 24) return `${hours}h ago`;
    if (days < 7) return `${days}d ago`;
    
    return date.toLocaleDateString();
  }
  
  function getAvatarUrl(user) {
    if (user.avatar) {
      return `https://cdn.discordapp.com/avatars/${user.id}/${user.avatar}.png`;
    }
    return `https://cdn.discordapp.com/embed/avatars/${user.discriminator % 5}.png`;
  }
</script>

<div class="comments-section">
  <button 
    class="toggle-btn" 
    on:click={() => showComments = !showComments}
  >
    <MessageCircle size={20} />
    <span>Comments ({comments.length})</span>
  </button>
  
  {#if showComments}
    <div class="comments-container">
      <!-- Authentication Section -->
      <div class="auth-section">
        {#if user}
          <div class="user-info">
            <img 
              src={getAvatarUrl(user)} 
              alt={user.username} 
              class="user-avatar"
            />
            <div class="user-details">
              <div class="username">{user.username}</div>
              <div class="discriminator">#{user.discriminator}</div>
            </div>
            <button class="logout-btn" on:click={handleLogout}>
              <LogOut size={16} />
            </button>
          </div>
        {:else}
          <div class="login-prompt">
            <div class="login-text">
              <User size={20} />
              <span>Sign in with Discord to comment</span>
            </div>
            <button class="discord-login-btn" on:click={handleDiscordLogin}>
              <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
                <path d="M20.317 4.37a19.791 19.791 0 0 0-4.885-1.515a.074.074 0 0 0-.079.037c-.21.375-.444.864-.608 1.25a18.27 18.27 0 0 0-5.487 0a12.64 12.64 0 0 0-.617-1.25a.077.077 0 0 0-.079-.037A19.736 19.736 0 0 0 3.677 4.37a.07.07 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057a.082.082 0 0 0 .031.057a19.9 19.9 0 0 0 5.993 3.03a.078.078 0 0 0 .084-.028a14.09 14.09 0 0 0 1.226-1.994a.076.076 0 0 0-.041-.106a13.107 13.107 0 0 1-1.872-.892a.077.077 0 0 1-.008-.128a10.2 10.2 0 0 0 .372-.292a.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127a12.299 12.299 0 0 1-1.873.892a.077.077 0 0 0-.041.107c.36.698.772 1.362 1.225 1.993a.076.076 0 0 0 .084.028a19.839 19.839 0 0 0 6.002-3.03a.077.077 0 0 0 .032-.054c.5-5.177-.838-9.674-3.549-13.66a.061.061 0 0 0-.031-.03zM8.02 15.33c-1.183 0-2.157-1.085-2.157-2.419c0-1.333.956-2.419 2.157-2.419c1.21 0 2.176 1.096 2.157 2.42c0 1.333-.956 2.418-2.157 2.418zm7.975 0c-1.183 0-2.157-1.085-2.157-2.419c0-1.333.955-2.419 2.157-2.419c1.21 0 2.176 1.096 2.157 2.42c0 1.333-.946 2.418-2.157 2.418z"/>
              </svg>
              <span>Login with Discord</span>
            </button>
          </div>
        {/if}
      </div>
      
      <!-- Comment Input -->
      {#if user}
        <div class="comment-input-section">
          <div class="input-container">
            <textarea
              bind:value={newComment}
              on:keypress={handleKeyPress}
              placeholder="Write a comment..."
              class="comment-input"
              rows="3"
            ></textarea>
            <button 
              class="submit-btn" 
              on:click={handleSubmitComment}
              disabled={!newComment.trim()}
            >
              <Send size={16} />
            </button>
          </div>
        </div>
      {/if}
      
      <!-- Comments List -->
      <div class="comments-list">
        {#if comments.length === 0}
          <div class="no-comments">
            <MessageCircle size={24} />
            <p>No comments yet. Be the first to comment!</p>
          </div>
        {:else}
          {#each comments as comment (comment.id)}
            <div class="comment">
              <div class="comment-header">
                <img 
                  src={getAvatarUrl(comment.user)} 
                  alt={comment.user.username} 
                  class="comment-avatar"
                />
                <div class="comment-meta">
                  <div class="comment-author">
                    {comment.user.username}
                    <span class="discriminator">#{comment.user.discriminator}</span>
                  </div>
                  <div class="comment-time">
                    <Clock size={12} />
                    {formatTime(comment.timestamp)}
                  </div>
                </div>
              </div>
              <div class="comment-content">
                {comment.text}
              </div>
            </div>
          {/each}
        {/if}
      </div>
    </div>
  {/if}
</div>

<style>
  .comments-section {
    margin-top: 30px;
  }
  
  .toggle-btn {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 12px 20px;
    background-color: rgba(224, 192, 192, 0.9);
    color: #2C2020;
    border: none;
    border-radius: 12px;
    font-size: 0.95rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
    backdrop-filter: blur(5px);
  }
  
  .toggle-btn:hover {
    background-color: rgba(224, 192, 192, 1);
    transform: translateY(-1px);
  }
  
  .comments-container {
    margin-top: 20px;
    background-color: rgba(224, 192, 192, 0.9);
    border-radius: 12px;
    padding: 20px;
    backdrop-filter: blur(5px);
  }
  
  .auth-section {
    margin-bottom: 20px;
    padding-bottom: 20px;
    border-bottom: 1px solid rgba(44, 32, 32, 0.2);
  }
  
  .user-info {
    display: flex;
    align-items: center;
    gap: 12px;
  }
  
  .user-avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 2px solid rgba(44, 32, 32, 0.2);
  }
  
  .user-details {
    flex: 1;
  }
  
  .username {
    font-weight: 600;
    color: #2C2020;
  }
  
  .discriminator {
    font-size: 0.8rem;
    opacity: 0.7;
    color: #2C2020;
  }
  
  .logout-btn {
    padding: 8px;
    background-color: rgba(44, 32, 32, 0.1);
    border: none;
    border-radius: 8px;
    color: #2C2020;
    cursor: pointer;
    transition: all 0.2s ease;
  }
  
  .logout-btn:hover {
    background-color: rgba(44, 32, 32, 0.2);
  }
  
  .login-prompt {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 15px;
  }
  
  .login-text {
    display: flex;
    align-items: center;
    gap: 8px;
    color: #2C2020;
    font-weight: 500;
  }
  
  .discord-login-btn {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 10px 16px;
    background-color: #5865F2;
    color: white;
    border: none;
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
  }
  
  .discord-login-btn:hover {
    background-color: #4752C4;
    transform: translateY(-1px);
  }
  
  .comment-input-section {
    margin-bottom: 20px;
  }
  
  .input-container {
    display: flex;
    gap: 10px;
    align-items: flex-end;
  }
  
  .comment-input {
    flex: 1;
    padding: 12px;
    border: 1px solid rgba(44, 32, 32, 0.2);
    border-radius: 8px;
    background-color: rgba(255, 255, 255, 0.9);
    color: #2C2020;
    font-family: inherit;
    resize: vertical;
    min-height: 60px;
  }
  
  .comment-input:focus {
    outline: none;
    border-color: #8b5cf6;
    box-shadow: 0 0 0 2px rgba(139, 92, 246, 0.2);
  }
  
  .submit-btn {
    padding: 12px 16px;
    background-color: #8b5cf6;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s ease;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .submit-btn:hover:not(:disabled) {
    background-color: #7c3aed;
    transform: translateY(-1px);
  }
  
  .submit-btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .comments-list {
    max-height: 400px;
    overflow-y: auto;
  }
  
  .no-comments {
    text-align: center;
    padding: 40px 20px;
    color: #2C2020;
    opacity: 0.7;
  }
  
  :global(.no-comments svg) {
    margin-bottom: 10px;
  }
  
  .comment {
    padding: 15px 0;
    border-bottom: 1px solid rgba(44, 32, 32, 0.1);
  }
  
  .comment:last-child {
    border-bottom: none;
  }
  
  .comment-header {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 8px;
  }
  
  .comment-avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
  }
  
  .comment-meta {
    flex: 1;
  }
  
  .comment-author {
    font-weight: 600;
    color: #2C2020;
    font-size: 0.9rem;
  }
  
  .comment-author .discriminator {
    font-weight: normal;
    opacity: 0.7;
    margin-left: 4px;
  }
  
  .comment-time {
    display: flex;
    align-items: center;
    gap: 4px;
    font-size: 0.8rem;
    opacity: 0.7;
    color: #2C2020;
    margin-top: 2px;
  }
  
  .comment-content {
    color: #2C2020;
    line-height: 1.5;
    margin-left: 42px;
  }
  
  /* Scrollbar styling */
  .comments-list::-webkit-scrollbar {
    width: 6px;
  }
  
  .comments-list::-webkit-scrollbar-track {
    background: rgba(44, 32, 32, 0.1);
    border-radius: 3px;
  }
  
  .comments-list::-webkit-scrollbar-thumb {
    background: rgba(44, 32, 32, 0.3);
    border-radius: 3px;
  }
  
  .comments-list::-webkit-scrollbar-thumb:hover {
    background: rgba(44, 32, 32, 0.5);
  }
  
  @media (max-width: 768px) {
    .login-prompt {
      flex-direction: column;
      align-items: stretch;
    }
    
    .input-container {
      flex-direction: column;
      align-items: stretch;
    }
    
    .submit-btn {
      align-self: flex-end;
      width: fit-content;
    }
  }
</style>
