# 💣 Boom It! Game

A fun party game where you pass the phone around while answering "Most Likely To" questions until it randomly "booms" and assigns a penalty!

## 🚀 Setup for GitHub Pages

1. **Create a new GitHub repository**
   - Go to github.com and create a new repository (e.g., `boom-it-game`)
   - Make it public

2. **Upload these files to your repository**
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - `icon-192.png` (you'll need to create this - see below)
   - `icon-512.png` (you'll need to create this - see below)

3. **Enable GitHub Pages**
   - Go to repository Settings → Pages
   - Under "Source", select "Deploy from a branch"
   - Select "main" branch and "/ (root)" folder
   - Click Save
   - Your site will be live at `https://yourusername.github.io/boom-it-game/`

4. **Create Icons (Optional for PWA)**
   - Open `create_icons.html` in a browser
   - Download the generated icons
   - Upload them to your repository

## 📝 Managing Suggestions

Currently, suggestions are stored in the browser's localStorage. Here are three ways to manage them:

### Option 1: Manual Browser Console Method (Quick & Easy)
1. Open the game in your browser
2. Press F12 to open Developer Tools
3. Go to the Console tab
4. Type: `JSON.parse(localStorage.getItem('suggestions'))`
5. Review the suggestions, then manually add approved ones to the code

### Option 2: Create a Simple Admin Page (Recommended)
Create a new file called `admin.html` in your repository:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Boom It Admin</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; }
        .suggestion { 
            border: 1px solid #ccc; 
            padding: 15px; 
            margin: 10px 0; 
            border-radius: 8px;
        }
        .question { background: #e3f2fd; }
        .penalty { background: #fff3e0; }
        button { margin: 5px; padding: 8px 15px; cursor: pointer; }
        .approve { background: #4caf50; color: white; border: none; }
        .reject { background: #f44336; color: white; border: none; }
    </style>
</head>
<body>
    <h1>Boom It Suggestions</h1>
    <div id="suggestions"></div>
    
    <script>
        const suggestions = JSON.parse(localStorage.getItem('suggestions') || '[]');
        const container = document.getElementById('suggestions');
        
        if (suggestions.length === 0) {
            container.innerHTML = '<p>No suggestions yet!</p>';
        } else {
            suggestions.forEach((s, i) => {
                const div = document.createElement('div');
                div.className = `suggestion ${s.type}`;
                div.innerHTML = `
                    <strong>${s.type.toUpperCase()}</strong><br>
                    ${s.content}<br>
                    <small>${new Date(s.timestamp).toLocaleString()}</small><br>
                    <button class="approve" onclick="approve(${i}, '${s.type}', '${s.content.replace(/'/g, "\\'")}')">Approve</button>
                    <button class="reject" onclick="reject(${i})">Reject</button>
                `;
                container.appendChild(div);
            });
        }
        
        function approve(index, type, content) {
            alert(`To approve, add this to index.html:\n\nIn the ${type}s array:\n"${content}"`);
            reject(index);
        }
        
        function reject(index) {
            const suggestions = JSON.parse(localStorage.getItem('suggestions') || '[]');
            suggestions.splice(index, 1);
            localStorage.setItem('suggestions', JSON.stringify(suggestions));
            location.reload();
        }
    </script>
</body>
</html>
```

Access it at `https://yourusername.github.io/boom-it-game/admin.html`

### Option 3: GitHub Issues (Most Robust)
You can modify the `submitSuggestion()` function to create actual GitHub issues:

1. Create a Personal Access Token in GitHub (Settings → Developer settings → Personal access tokens → Tokens (classic))
2. Give it `public_repo` scope
3. Update the JavaScript in `index.html` to POST to GitHub's API

## 🎮 Adding New Questions/Penalties

To add approved questions or penalties, edit `index.html`:

Find these arrays:
```javascript
const questions = [
    "accidentally send a text to the wrong person",
    "become a millionaire",
    // Add new questions here
];

const penalties = [
    "Do 10 pushups",
    "Tell an embarrassing story",
    // Add new penalties here
];
```

## 🎨 Customization

- **Colors**: Edit CSS variables in the `:root` section
- **Boom timing**: Change the random range in `boomThreshold`
- **Fonts**: Update the Google Fonts import
- **Animations**: Modify the `@keyframes` animations

## 📱 Installing as PWA

On mobile devices:
- **iOS**: Tap Share → Add to Home Screen
- **Android**: Tap Menu (⋮) → Add to Home Screen or Install App

## 🔧 Local Testing

Simply open `index.html` in a browser, or use a local server:
```bash
python -m http.server 8000
# Then visit http://localhost:8000
```

Enjoy the game! 💥
