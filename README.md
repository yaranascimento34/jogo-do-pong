let leftPaddle;
let rightPaddle;
let ball;

function setup() {
  createCanvas(600, 400);
  leftPaddle = new Paddle(true);
  rightPaddle = new Paddle(false);
  ball = new Ball();
}

function draw() {
  background(0);

  leftPaddle.show();
  rightPaddle.show();
  ball.show();

  leftPaddle.update();
  rightPaddle.update();
  ball.update();

  leftPaddle.edges();
  rightPaddle.edges();
  ball.edges();

  leftPaddle.hit(ball);
  rightPaddle.hit(ball);
}

function keyPressed() {
  if (keyCode === UP_ARROW) {
    rightPaddle.move(-1);
  } else if (keyCode === DOWN_ARROW) {
    rightPaddle.move(1);
  }

  if (key === 'W' || key === 'w') {
    leftPaddle.move(-1);
  } else if (key === 'S' || key === 's') {
    leftPaddle.move(1);
  }
}

function keyReleased() {
  if (keyCode === UP_ARROW || keyCode === DOWN_ARROW) {
    rightPaddle.move(0);
  }

  if (key === 'W' || key === 'w' || key === 'S' || key === 's') {
    leftPaddle.move(0);
  }
}

class Paddle {
  constructor(isLeft) {
    this.width = 10;
    this.height = 80;
    this.y = height / 2 - this.height / 2;
    this.isLeft = isLeft;
    this.speed = 5;
    this.direction = 0;
  }

  show() {
    fill(255);
    noStroke();
    rect(
      this.isLeft ? 0 : width - this.width,
      this.y,
      this.width,
      this.height
    );
  }

  update() {
    this.y += this.direction * this.speed;
  }

  edges() {
    this.y = constrain(this.y, 0, height - this.height);
  }

  hit(ball) {
    if (
      ball.x - ball.radius < this.width &&
      ball.y > this.y &&
      ball.y < this.y + this.height
    ) {
      ball.xSpeed *= -1;
    }
  }

  move(dir) {
    this.direction = dir;
  }
}

class Ball {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.radius = 10;
    this.xSpeed = 5;
    this.ySpeed = 3;
  }

  show() {
    fill(255);
    noStroke();
    ellipse(this.x, this.y, this.radius * 2);
  }

  update() {
    this.x += this.xSpeed;
    this.y += this.ySpeed;
  }

  edges() {
    if (this.y - this.radius < 0 || this.y + this.radius > height) {
      this.ySpeed *= -1;
    }

    if (this.x - this.radius < 0 || this.x + this.radius > width) {
      this.reset();
    }
  }

  reset() {
    this.x = width / 2;
    this.y = height / 2;
  }
}
