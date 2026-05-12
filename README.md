# Guess the Quote
A browser-based quote guessing game where player's attempt to guess the source of the quote

## Features
- Randomized quote order
- Score tracking system
- Category filtering (Movies / TV shows / All)
- Attempt tracking (max 3 attempts per quote)
- Game reset functionality
- Instructions popup
- Responsive screen switching (menu &harr; game)

## How to Play
- Select a mode
- Read the quote and guess where it comes from
- You get **3 tries** per quote before the answer is revealed
- You can skip a quote at any time
- Type your guess and press submit guess or enter to check your answer

## Code Overview

The game consists of several important functions:

### 1. Random Quote Generation

```javascript
function shuffleArray(quotes) {
    
    let m = quotes.length;
    while (m > 0) {
        const i = Math.floor(Math.random() * m);
        m--;

        [quotes[m], quotes[i]] = [quotes[i], quotes[m]];
    }
    return quotes;
}
```

### 2. Start game function

```javascript
// Filters quotes by mode and starts the game
function startGame(mode) {
    currentMode = mode;

    if (mode === 'all') {
        activeQuotes = [...quotes];
    } else {
        activeQuotes = quotes.filter(q => q.category === mode);
    }

    shuffledQuote = shuffleArray([...activeQuotes]);
    currentIndex = 0;

    // Switches screens
    document.getElementById('modeSelector').classList.replace('active-screen', 'hidden-screen');
    document.getElementById('gameScreen').classList.replace('hidden-screen', 'active-screen');

    displayQuote();
}
```

### 3. Display Quote Function

```javascript
// Displays current quote
function displayQuote() {
    document.getElementById('quote-container').textContent = `"${shuffledQuote[currentIndex].quote}"`;
    guessInput.value ='';
}
```

### 4. Guess Checking Function

```javascript
function checkGuess(){
   
    // Tracks score and informs user they guessed correctly
    if (userGuess === correctGuess){
        feedback.textContent = `Congratulations! You guessed correct.`;
        feedback.style.color = 'green';
        guesses = 0;

        score++;

        if (score > 0) {
            tracker.textContent = `Score: ${score}`;
            tracker.className = 'score';
        }
       
        // moves to next quote after delay
        setTimeout(nextQuote, 1500);

        return;

    } 
    // Asks user to enter input
    else if (userGuess === '') {
        feedback.textContent = "Please input a guess.";
        feedback.style.color = 'orange';
    }
    // increases guess and tracks wrong answer
    else {
        guesses++
        feedback.textContent = `Wrong Answer! (${guesses}/3)`;
        feedback.style.color = 'red';
    }

    // Reveals answer after too many wrong guesses
    if (guesses >= 3) {
        feedback.textContent = `Correct Answer: ${shuffledQuote[currentIndex].answer}`;
        feedback.style.color = 'navy';
        guesses = 0;

        setTimeout(nextQuote, 2000);
        return;
    }

    guessInput.value ='';
}
```

### 4. Next Quote Function

```javascript
// Moves to the next quote
function nextQuote() {
    currentIndex++;

    // checks if the game is over
    if (currentIndex >= shuffledQuote.length) {
        gameOver();
        setTimeout(resetGame, 3000);
        return;
    }

    displayQuote();
    feedback.textContent = '';
}
```

### 5. Reset Game Function

```javascript
function resetGame() {
   
    feedback.textContent = '';
    tracker.textContent = '';
    tracker.className = '';

    shuffledQuote = shuffleArray([...activeQuotes]);
    currentIndex = 0;
    guesses = 0;
    score = 0;

    displayQuote();
}
```

## Technologies Used

- HTML
- CSS
- JavaScript

## Implementation

To implement this game you need:

1. An HTML file with:
- A menu screen with:
    - Mode selection buttons
    - Instructions popup
- A game screen with:
    - Quote display area
    - Input field
    - Submit button
    - Skip button
    - Go back to menu button
    - Element to track score
    - Element to display feedback

2. The JavaScript code provided above

3. Optional CSS for styling

## Example HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guess the Quote</title>
    <link rel="stylesheet" href="style.css">
    <script src="script.js" defer></script>
</head>
<body>
    <h1>Guess the Quote</h1>

    <div id="modeSelector"  class="active-screen">
       
        <h2>Select a Mode:</h2>
        <button id="allBtn">All</button>
        <button id="tvBtn">TV Shows</button>
        <button id="movieBtn">Movies</button>
        
    </div>

    <div id="gameScreen" class="hidden-screen">

        <div id="quote-container"></div>

        <p id="feedback"></p>

        <input type="text" id="guessInput" placeholder="Enter a guess" accesskey="i">

        <p id="scoreTracker"></p>
        <br/>

        <button id="submitBtn" accesskey="g">Submit Guess</button>
        <button id="skipBtn" accesskey="s">Skip Quote</button>
        <button id="menuBtn" accesskey="m">Back to Menu</button>
    </div>
</body>
</html>
```

## Known Issues/Future Improvements
- Improve mobile responsiveness
- Add more stand-up comedy quotes
- Add more quote categories