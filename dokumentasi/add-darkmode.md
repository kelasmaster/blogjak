# Add Dark/Light Mode

CSS:

```
/* Define CSS Variables for Light Mode */
:root {
  --background-color: #ffffff;
  --text-color: #000000;
}

/* Define CSS Variables for Dark Mode */
[data-theme="dark"] {
  --background-color: #121212;
  --text-color: #ffffff;
}

/* Apply Variables to Body */
body {
  background-color: var(--background-color);
  color: var(--text-color);
  font-family: Arial, sans-serif;
  line-height: 1.6;
  transition: background-color 0.3s ease, color 0.3s ease;
  padding: 0 20px;
}
/* Button Styling */
button {
  padding: 10px 20px;
  background-color: #007BFF;
  color: #ffffff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: opacity 0.3s ease;
}

button:hover {
  opacity: 0.9;
}
```

JS:

```
// Select the toggle button
const toggleButton = document.getElementById('toggle-mode');

// Check if user has a preferred theme in localStorage
const savedTheme = localStorage.getItem('theme');
if (savedTheme) {
  document.body.setAttribute('data-theme', savedTheme);
}

// Toggle theme function
toggleButton.addEventListener('click', () => {
  const currentTheme = document.body.getAttribute('data-theme');
  const newTheme = currentTheme === 'dark' ? 'light' : 'dark';

  // Set the new theme
  document.body.setAttribute('data-theme', newTheme);

  // Save the theme preference to localStorage
  localStorage.setItem('theme', newTheme);
});
```

HTML:

```
<button id="toggle-mode">Toggle Dark/Light Mode</button>
```
