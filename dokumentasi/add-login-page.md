# Login Page

Here is whole code

CSS
```
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f9; 
}

.login-container {
  background: #fff;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
 
}

.login-form h2,
.content h2 {
  margin-bottom: 1.5rem;
  color: #333;
}

.content h6 {
 text-align: center;
}

.input-group {
  margin-bottom: 1rem;
}

.input-group label {
  display: block;
  margin-bottom: 0.5rem;
  color: #555;
  text-align: left;
}

.input-group input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.input-group input:focus {
  border-color: #007bff;
  outline: none;
}

.btn {
  width: 100%;
  padding: 0.75rem;
  background-color: #444;;
  color: #fff;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  cursor: pointer;
}

.btn:hover {
  background-color: #0056b3;
}

.error-message {
  color: red;
  text-align: center;
  margin-top: 1rem;
  display: none;
}

.content {
  margin-top: 1rem;
}

@media (max-width: 600px) {
  .login-container {
    padding: 1.5rem;
  }
}
```

JS
```
  document.addEventListener('DOMContentLoaded', function () {
  const loginForm = document.getElementById('loginForm');
  const content = document.getElementById('content');
  const logoutBtn = document.getElementById('logoutBtn');
  const errorMessage = document.getElementById('error-message');
  const expiryDateElement = document.getElementById('expiryDate');

  // Define valid email-password pairs with validity periods (in days)
  const validUsers = [
    { email: 'bandarlaundry@gmail.com', password: 'password123', validityPeriod: 7 }, // 7 days
    { email: 'bandardeterjen@gmail.com', password: 'password456', validityPeriod: 14 }, // 14 days
    { email: 'bezimeni.id@gmail.com', password: 'mypassword', validityPeriod: 30 } // 30 days
  ];

  // Check if the user is already logged in
  const storedData = localStorage.getItem('loginData');
  if (storedData) {
    const { email, expiry } = JSON.parse(storedData);
    const now = new Date().getTime();

    if (now < expiry) {
      // User is still logged in
      showContent(email, expiry);
    } else {
      // Login expired
      localStorage.removeItem('loginData');
    }
  }

  // Handle login form submission
  loginForm.addEventListener('submit', function (event) {
    event.preventDefault();

    const email = document.getElementById('email').value.trim();
    const password = document.getElementById('password').value.trim();

    // Validate email and password against the validUsers array
    const user = validUsers.find(user => user.email === email && user.password === password);

    if (user) {
      errorMessage.style.display = 'none';

      // Calculate expiration date based on the user's validity period
      const now = new Date().getTime();
      const validityInMilliseconds = user.validityPeriod * 24 * 60 * 60 * 1000; // Convert days to milliseconds
      const expiry = now + validityInMilliseconds;

      // Store login data in localStorage
      const loginData = { email, expiry };
      localStorage.setItem('loginData', JSON.stringify(loginData));

      // Show content
      showContent(email, expiry);
    } else {
      errorMessage.textContent = 'Invalid email or password';
      errorMessage.style.display = 'block';
    }
  });

  // Handle logout button click
  logoutBtn.addEventListener('click', function () {
    localStorage.removeItem('loginData'); // Remove login data
    hideContent(); // Hide content and show login form
  });

  // Function to show content and set the expiry date
  function showContent(email, expiry) {
    loginForm.style.display = 'none';
    content.style.display = 'block';
    expiryDateElement.textContent = new Date(expiry).toLocaleString();
  }

  // Function to hide content and show login form
  function hideContent() {
    loginForm.reset();
    loginForm.style.display = 'block';
    content.style.display = 'none';
    errorMessage.style.display = 'none';
  }
});

 var myElement = document.getElementById("myElement");
    myElement.addEventListener("contextmenu", function(event) {
        event.preventDefault(); // Prevent the default right-click menu behavior
    });
```

Posts/Index
```
<div class="login-container">
    <!-- Login Form -->
    <form id="loginForm" class="login-form">
      <h2>Login</h2>
      <div class="input-group">
        <label for="email">Email</label>
        <input type="email" id="email" name="email" placeholder="Enter your email" required>
      </div>
      <div class="input-group">
        <label for="password">Password</label>
        <input type="password" id="password" name="password" placeholder="Enter your password" required>
      </div>
      <button type="submit" class="btn">Login</button>
      <p id="error-message" class="error-message"></p>
    </form>

 <div id="content" class="content" style="display: none;">
     <div class="container">
<div id="header"></div> 
<div class="content">
    <h2>Latest Posts</h2>
    <div class="post-list">
      <div class="post-item">
        <h3><a href="posts/post1.html">My First Blog Post</a></h3>
        <p>Posted on March 5, 2025</p>
        <p>This is a short excerpt from the blog post...</p>
      </div>
     <div class="post-item">
        <h3><a href="posts/post2.html">My Second Blog Post</a></h3>
        <p>Posted on March 5, 2025</p>
        <p>This is a short excerpt from the blog post...</p>
      </div>
    </div>
  </div>
       <button id="logoutBtn" class="btn"><span id="expiryDate"></span> Logout</button>
        <div id="footer"></div>
    </div>
  </div>
```
