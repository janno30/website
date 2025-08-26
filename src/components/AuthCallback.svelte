<script>
  import { onMount } from 'svelte';
  import { CheckCircle, XCircle, Loader } from 'lucide-svelte';
  
  let status = 'loading';
  let message = 'Processing authentication...';
  
  onMount(async () => {
    const urlParams = new URLSearchParams(window.location.search);
    const code = urlParams.get('code');
    const error = urlParams.get('error');
    
    if (error) {
      status = 'error';
      message = 'Authentication failed. Please try again.';
      return;
    }
    
    if (!code) {
      status = 'error';
      message = 'No authorization code received.';
      return;
    }
    
    try {
      // In a real application, you would send this code to your backend
      // For now, we'll simulate the process with a mock user
      await simulateAuth(code);
    } catch (err) {
      status = 'error';
      message = 'Authentication failed. Please try again.';
    }
  });
  
  async function simulateAuth(code) {
    // Simulate API call delay
    await new Promise(resolve => setTimeout(resolve, 2000));
    
    // Mock user data (in real app, this would come from Discord API)
    const mockUser = {
      id: '123456789',
      username: 'JannoUser',
      discriminator: '1234',
      avatar: null
    };
    
    // Store user data
    localStorage.setItem('discord-user', JSON.stringify(mockUser));
    
    status = 'success';
    message = 'Authentication successful! Redirecting...';
    
    // Redirect back to main page
    setTimeout(() => {
      window.location.href = '/';
    }, 1500);
  }
</script>

<div class="auth-callback">
  <div class="callback-container">
    {#if status === 'loading'}
      <Loader size={48} class="spinner" />
      <h2>Authenticating...</h2>
      <p>{message}</p>
    {:else if status === 'success'}
      <CheckCircle size={48} class="success-icon" />
      <h2>Success!</h2>
      <p>{message}</p>
    {:else}
      <XCircle size={48} class="error-icon" />
      <h2>Authentication Failed</h2>
      <p>{message}</p>
      <button class="retry-btn" on:click={() => window.location.href = '/'}>
        Go Back
      </button>
    {/if}
  </div>
</div>

<style>
  .auth-callback {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
    padding: 20px;
  }
  
  .callback-container {
    background-color: rgba(224, 192, 192, 0.95);
    backdrop-filter: blur(10px);
    border-radius: 20px;
    padding: 40px;
    text-align: center;
    max-width: 400px;
    width: 100%;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
    border: 1px solid rgba(255, 255, 255, 0.1);
  }
  
  :global(.spinner) {
    color: #8b5cf6;
    animation: spin 1s linear infinite;
    margin-bottom: 20px;
  }
  
  :global(.success-icon) {
    color: #10b981;
    margin-bottom: 20px;
  }
  
  :global(.error-icon) {
    color: #ef4444;
    margin-bottom: 20px;
  }
  
  h2 {
    color: #2C2020;
    margin-bottom: 10px;
    font-size: 1.5rem;
  }
  
  p {
    color: #2C2020;
    opacity: 0.8;
    margin-bottom: 20px;
  }
  
  .retry-btn {
    padding: 12px 24px;
    background-color: #8b5cf6;
    color: white;
    border: none;
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
  }
  
  .retry-btn:hover {
    background-color: #7c3aed;
    transform: translateY(-1px);
  }
  
  @keyframes spin {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }
</style>
