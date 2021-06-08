// Global variable to store the classifier
              
    let classifier;

// Label
               
    let label = 'listening...';

// Teachable Machine model URL:
       
    let soundModel = 'https://teachablemachine.withgoogle.com/models/r3pZzouCY/';

    let img;

    function preload() {
  // Load the model
    
    classifier = ml5.soundClassifier(soundModel + 'model.json');

    img = loadImage('couple.png');


    }

    let snake;
    let square; 
    let rez = 20;
    let food;
    let w;
    let h;

    let image1;

    function setup() {
    classifier.classify(gotResult);
    createCanvas(640, 480);
  // Start classifying
  // The sound model will continuously listen to the microphone
  
    w = floor(width / rez);
    h = floor(height/ rez);
    frameRate(5);
    snake = new Snake ();
    square = new Snake(); 
  
    foodLocation();

    }

    function foodLocation() {
    let x = floor(random(w));
    let y = floor(random(h));
    food = createVector(x, y);
  
    }

    function controlSnake(){
    if (label === "DO") { //LEFT
    snake.setDir(-1, 0);
    } else if (label === "RE") { //RIGHT
    snake.setDir(1, 0);
    } else if (label === "MI") { //DOWN
    snake.setDir(0, 1);  
    } else if (label === "FA") { //UP
    snake.setDir(0, -1);
    } 

    }

    function draw() {
    scale(rez);
    background(0,0,255);
    if (snake.eat(food)) {
    foodLocation();
    }
    snake.update();
    snake.show();


    if (snake.endGame()) {
    print("END GAME");
    background(255, 0, 0);
    noLoop();
    }

    noStroke();
    fill(255, 255, 0);
    rect(food.x, food.y, 1, 1);
  
    fill(0,0,255);
    textSize(32);
    textAlign(CENTER, CENTER);
    text(label, width / 2, height / 2);
  
    }
// The model recognizing a sound will trigger this event

    function gotResult(error, results) {
    if (error) {
    console.error(error);
    return;
    }
  
  // The results are in an array ordered by confidence.
  // console.log(results[0]);
    
    label = results[0].label;
    controlSnake();
    }
