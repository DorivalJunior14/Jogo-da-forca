# Jogo da forca
 exercicio
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link href="https://fonts.googleapis.com/css2?family=Inter&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="style.css" />
    <title>Jogo da Forca</title>
  
    
    
  </head>
  <body>
    <header>
      <img src="imagens/logo.png" alt="logo da alura">

  </header>
    <div class="container">
      <div id="options-container"></div>
      <div id="letter-container" class="letter-container hide"></div>
      <div id="user-input-section"></div>
      <canvas id="canvas"></canvas>
      <div id="novo-jogo-container" class="novo-jogo-popup hide">
        <div id="result-text"></div>
        <button id="novo-jogo-button">Novo Jogo</button>
      </div>
    </div>
   
    <script src="script.js"></script>
  </body>
</html>
css
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: "inter", sans-serif;
  }
  body {
    background-color: gray;
  }
  .logo.png{
    position: absolute;
    left: 36.67%;
    right: 37.43%;
    top: 0%;
    bottom: 0%;
    background: #0A3871;}
  .container {
    font-size: 16px;
    background-color: #ffffff;
    width: 90vw;
    max-width: 34em;
    position: absolute;
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
    padding: 3em;
    border-radius: 0.6em;
    box-shadow: 0 1.2em 2.4em rgba(111, 85, 0, 0.25);
  }
  #options-container {
    text-align: center;
  }
  #options-container div {
    width: 100%;
    display: flex;
    justify-content: space-between;
    margin: 1.2em 0 2.4em 0;
  }
  #options-container button {
    padding: 0.6em 1.2em;
    border: 3px solid #000000;
    background-color: #ffffff;
    color: #000000;
    border-radius: 0.3em;
    text-transform: capitalize;
  }
  #options-container button:disabled {
    border: 3px solid #808080;
    color: #808080;
    background-color: #efefef;
  }
  #options-container button.active {
    
    background-color: rgb(78, 50, 205);
    border: 13px solid #000000;
    color: #000000;
  }
  .letter-container {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 0.6em;
  }
  #letter-container button {
    height: 2.4em;
    width: 2.4em;
    border-radius: 0.3em;
    background-color: #ffffff;
  }
  .novo-jogo-popup {
    background-color: #ffffff;
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    border-radius: 0.6em;
  }
  
  #user-input-section {
    display: flex;
    justify-content: center;
    font-size: 1.8em;
    margin: 0.6em 0 1.2em 0;
  }
  canvas {
    display: inline-block;
    
   
  }
  
  .hide {
    display: none;
  }
  #result-text h2 {
    font-size: 1.8em;
    text-align: center;
  }
  #result-text p {
    font-size: 1.25em;
    margin: 1em 0 2em 0;
  }
  #result-text span {
    font-weight: 600;
    
  }
  #novo-jogo-button {
    font-size: 1.25em;
    padding: 0.5em 1em;
    background-color: rgb(103, 146, 17);
    border: 3px solid #000000;
    color: #000000;
    border-radius: 0.2em;
  }
  .ganhou-msg {
    color: #16719b;
  }
  .perdeu-msg {
    color: #4b0505;
  }
  script
  //Initial References
const letterContainer = document.getElementById("letter-container");
const optionsContainer = document.getElementById("options-container");
const userInputSection = document.getElementById("user-input-section");
const newGameContainer = document.getElementById("novo-jogo-container");
const newGameButton = document.getElementById("novo-jogo-button");
const canvas = document.getElementById("canvas");
const resultText = document.getElementById("result-text");

//Options values for buttons
let options = {
  Estudos: [
    "Alura",
    "Logica",
    "Sistemas",
    "Phyton",
    "Javascript",
    "Oracle",
    "html",
  ],

};

//count
let winCount = 0;
let count = 0;

let chosenWord = "";

//Display option buttons
const displayOptions = () => {
  optionsContainer.innerHTML += `<h3>Uma chance, vc estuda?<br>click em estudos e digite uma letra</h3>`;
  let buttonCon = document.createElement("div");
  for (let value in options) {
    buttonCon.innerHTML += `<button class="options" onclick="generateWord('${value}')">${value}</button>`;
  }
  optionsContainer.appendChild(buttonCon);
};

//Block all the Buttons
const blocker = () => {
  let optionsButtons = document.querySelectorAll(".options");
  let letterButtons = document.querySelectorAll(".letters");
  //disable all options
  optionsButtons.forEach((button) => {
    button.disabled = true;
  });

  //disable all letters
  letterButtons.forEach((button) => {
    button.disabled.true;
  });
  newGameContainer.classList.remove("hide");
};

//Word Generator
const generateWord = (optionValue) => {
  let optionsButtons = document.querySelectorAll(".options");
  //If optionValur matches the button innerText then highlight the button
  optionsButtons.forEach((button) => {
    if (button.innerText.toLowerCase() === optionValue) {
      button.classList.add("active");
    }
    button.disabled = true;
  });

  //initially hide letters, clear previous word
  letterContainer.classList.remove("hide");
  userInputSection.innerText = "";

  let optionArray = options[optionValue];
  //choose random word
  chosenWord = optionArray[Math.floor(Math.random() * optionArray.length)];
  chosenWord = chosenWord.toUpperCase();

  //replace every letter with span containing dash
  let displayItem = chosenWord.replace(/./g, '<span class="dashes">_</span>');

  //Display each element as span
  userInputSection.innerHTML = displayItem;
};

//Initial Function (Called when page loads/user presses new game)
const initializer = () => {
  winCount = 0;
  count = 0;

  //Initially erase all content and hide letteres and new game button
  userInputSection.innerHTML = "";
  optionsContainer.innerHTML = "";
  letterContainer.classList.add("hide");
  newGameContainer.classList.add("hide");
  letterContainer.innerHTML = "";

  //For creating letter buttons
  for (let i = 65; i < 91; i++) {
    let button = document.createElement("button");
    button.classList.add("letters");
    //Number to ASCII[A-Z]
    button.innerText = String.fromCharCode(i);
    //character button click
    button.addEventListener("click", () => {
      let charArray = chosenWord.split("");
      let dashes = document.getElementsByClassName("dashes");
      //if array contains clciked value replace the matched dash with letter else dram on canvas
      if (charArray.includes(button.innerText)) {
        charArray.forEach((char, index) => {
          //if character in array is same as clicked button
          if (char === button.innerText) {
            //replace dash with letter
            dashes[index].innerText = char;
            //increment counter
            winCount += 1;
            //if winCount equals word lenfth
            if (winCount == charArray.length) {
              resultText.innerHTML = `<h2 class='ganhou-msg'>Voce Ganhou!!</h2><p>Resposta correta <span>${chosenWord}</span></p>`;
              //block all buttons
              blocker();
            }
          }
        });
      } else {
        //perdeu count
        count += 1;
        //for drawing man
        drawMan(count);
        //Count==6 because head,body,left arm, right arm,left leg,right leg
        if (count == 6) {
          resultText.innerHTML = `<h2 class='perdeu-msg'>Voce Perdeu!!</h2><p>Ahh.. palavra era  <span>${chosenWord}</span></p>`;
          blocker();
        }
      }
      //disable clicked button
      button.disabled = true;
    });
    letterContainer.append(button);
  }

  displayOptions();
  //Call to canvasCreator (for clearing previous canvas and creating initial canvas)
  let { initialDrawing } = canvasCreator();
  //initialDrawing would draw the frame
  initialDrawing();
};

//Canvas
const canvasCreator = () => {
  let context = canvas.getContext("2d");
  context.beginPath();
  context.strokeStyle = "#000";
  context.lineWidth = 2;

  //For drawing lines
  const drawLine = (fromX, fromY, toX, toY) => {
    context.moveTo(fromX, fromY);
    context.lineTo(toX, toY);
    context.stroke();
  };

  const head = () => {
    context.beginPath();
    context.arc(70, 30, 10, 0, Math.PI * 2, true);
    context.stroke();
  };

  const body = () => {
    drawLine(70, 40, 70, 80);
  };

  const leftArm = () => {
    drawLine(70, 50, 50, 70);
  };

  const rightArm = () => {
    drawLine(70, 50, 90, 70);
  };

  const leftLeg = () => {
    drawLine(70, 80, 50, 110);
  };

  const rightLeg = () => {
    drawLine(70, 80, 90, 110);
  };

  //initial frame
  const initialDrawing = () => {
    //clear canvas
    context.clearRect(0, 0, context.canvas.width, context.canvas.height);
    //bottom line
    drawLine(10, 130, 130, 130);
    //left line
    drawLine(10, 10, 10, 131);
    //top line
    drawLine(10, 10, 70, 10);
    //small top line
    drawLine(70, 10, 70, 20);
  };

  return { initialDrawing, head, body, leftArm, rightArm, leftLeg, rightLeg };
};


const drawMan = (count) => {
  let { head, body, leftArm, rightArm, leftLeg, rightLeg } = canvasCreator();
  switch (count) {
    case 1:
      head();
      break;
    case 2:
      body();
      break;
    case 3:
      leftArm();
      break;
    case 4:
      rightArm();
      break;
    case 5:
      leftLeg();
      break;
    case 6:
      rightLeg();
      break;
    default:
      break;
  }
};

//Novo jogo
newGameButton.addEventListener("click", initializer);
window.onload = initializer;
parte foi copiada pois nas aulas não ensinam nada disso
